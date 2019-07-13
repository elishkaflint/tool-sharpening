# Spring beans scope and lifecycle

- Scope refers to lifecycle of the bean - how long does it live, how many instances etc


## Singleton Scope

Default scope is singleton

- only one instance of the bean
- cached in memory
- all requests for the bean will return a SHARED reference to the SAME bean


Coach theCoach = context.getBean("myCoach", Coach.class);
Coach alphaCoach = context.getBean("myCoach", Coach.class);

Both objects are accessing the same bean, best use case is for objects which don't hold any state


## Protoype Scope

- creates new bean instance for each container request

<beans>
  <bean id="myCoach" class="package.name.BaseballCoach" scope="prototype">
</beans>

Coach theCoach = context.getBean("myCoach", Coach.class);
Coach alphaCoach = context.getBean("myCoach", Coach.class);

Here Spring is creating 2 different objects, each with their own reference.

### Example with singleton scope:

_in applicationContextBeanScopeDemo.xml_
```java
<beans>

  <bean id="myFortuneService" class="package.name.HappyFortuneService"></bean>

  <bean id="myCoach" class="package.name.TrackCoach">
  // constructor injection
    <constructor-arg ref="myFortuneService" />
  </bean>

</beans>
```

```java
class BeanScopeDemoApp {

  public static void main(String[] args) {

    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContextBeanScopeDemo.xml");

    Coach theCoach = context.getBean("myCoach", Coach.class);
    Coach alphaCoach = context.getBean("myCoach", Coach.class);

    // check to see if these are the same beans
    boolean result = ( theCoach == alphaCoach );
    System.out.println("Pointing to the same object: " + result); // returns true

    // memory locations are the same
    System.out.println("Memory location for coach: " + theCoach);
    System.out.println("Memory location for alpha coach: " + alphaCoach);

    // close the context
    context.close();
  }  
}
```

### Example with prototype scope:

_in applicationContextBeanScopeDemo.xml_
```java
<beans>

  <bean id="myFortuneService" class="package.name.HappyFortuneService"></bean>

  <bean id="myCoach" class="package.name.TrackCoach" scope="prototype">
  // constructor injection
    <constructor-arg ref="myFortuneService" />
  </bean>

</beans>
```

```java
class BeanScopeDemoApp {

  public static void main(String[] args) {

    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContextBeanScopeDemo.xml");

    Coach theCoach = context.getBean("myCoach", Coach.class);
    Coach alphaCoach = context.getBean("myCoach", Coach.class);

    // check to see if these are the same beans
    boolean result = ( theCoach == alphaCoach );
    System.out.println("Pointing to the same object: " + result); // returns false

    // memory locations are not the same
    System.out.println("Memory location for coach: " + theCoach);
    System.out.println("Memory location for alpha coach: " + alphaCoach);

    // close the context
    context.close();
  }  
}
```


## Bean Lifecycle Methods

container started
bean instantiated
dependencies injected
internal spring processing
*custom init method* eg. business logic methods, handle to resources
bean ready for use
*custom shut down method*
container is shut down eg. business logic methods, handle to resources


#### Define init and destroy methods

```java
class TrackCoach implements Coach {

  ...

  // add an init method
  public void doStartupStuff() {
    System.out.println("TrackCoach: inside method doStartUpStuff");
  }

  // add a destroy method
  public void doDestroyStuff() {
    System.out.println("TrackCoach: inside method doDestroyStuff");
  }

}
```

#### Configure methods in Spring config

```java
<beans>

  <bean id="myCoach" class="package.name.TrackCoach" init-method="doStartupStuff" destroy-method="doDestroyStuff">
  </bean>

</beans>
```

NOTE - Spring does not manage the complete lifecycle of a prototype bean, the container instantiates, configures, and otherwise assembles a prototype object, and hands it to the client, with no further record of that prototype instance.

Thus, although initialization lifecycle callback methods are called on all objects regardless of scope, in the case of prototypes, configured destruction lifecycle callbacks are not called. The client code must clean up prototype-scoped objects and release expensive resources that the prototype bean(s) are holding.

This also applies to both XML configuration and Annotation-based configuration.
