# Inversion of Control

IOC is the approach of outsourcing construction and management of your objects - this will be handled by an "object factory".

We could instantiate and manage all our objects and dependencies in our Main class. But by using a framework like Spring, we hand control for that activity to the framework itself (in the case of Spring to the Spring IoC container which assembles everything for us based on the configuration we specify in our code).

A good article by Martin Fowler on IoC:

> Inversion of Control is a key part of what makes a framework different to a library. A library is essentially a set of functions that you can call, these days usually organized into classes. Each call does some work and returns control to the client.

> A framework embodies some abstract design, with more behavior built in. In order to use it you need to insert your behavior into various places in the framework either by subclassing or by plugging in your own classes. The framework's code then calls your code at these points.

https://martinfowler.com/bliki/InversionOfControl.html

Another way to think about IoC in the context of a framework like Spring:

> Inversion of control is turning your sequentially written code and turning it into an delegation structure. Instead of your program explicitly controlling everything, your program sets up a class or library with certain functions to be called when certain things happen.

IoC and DI are used often used interchangeably, however maybe we should think of dependency injection as a way to facilitate inversion of control, in addition to having a number of other benefits (cleaner, more flexible code which is decoupled and easier to test).

## Scenario

App will call a baseball coach for a daily workout
Management wants an app that is *configurable*, it should work for different sports

### Initial prototype

```java
class BaseballCoach {

    public String getWorkout() {
      return "practice passes";
    }

}
```

```java
class MyApp {

  public static void main(String[] args) {

    // create the object
    BaseballCoach theCoach = new BaseballCoach();

    // use the object
    System.out.println(theCoach.getWorkout());

  }

}
```

Run the app, we can run this, it will return "practice passes"

### Making it configurable

Now we should use best practice of *coding to an interface*

```java
public interface Coach {

  public String getWorkout();

}
```

```java
class BaseballCoach implements Coach {

    @Override
    public String getWorkout() {
      return "practice passes";
    }

}
```

```java
class TrackCoach implements Coach {

    @Override
    public String getWorkout() {
      return "go for a run";
    }

}
```

```java
class MyApp {

  public static void main(String[] args) {

    // create the object
    Coach theCoach = new TrackCoach();

    // use the object
    // getWorkout can now work with any implementation of Coach
    System.out.println(theCoach.getWorkout());

  }

}
```

Getting towards this being configurable, but TrackCoach is still hardcoded in MyApp, we still have to change the sourcecode.

### Using Spring IOC to get this *really* configurable

Spring provides an object factory, which gives us the right implementation of our object based on our configuration.

Spring Container
- creates and manages objects (IOC)
- injects object's dependencies (DI)

Spring Bean
- think of them as *simply a Java object*
- when Java objects are created by the Spring Container, then Spring refers to them as "Spring Beans"

3 ways to configure Spring Container:

1. XML config file

#### Configure spring beans

_in applicationContext.xml_
```java
<beans>
  <bean id="myCoach" class="package.name.BaseballCoach">
</beans>
```

#### Create spring container and retrieve beans

Generally known as an ApplicationContext

```java
class MyApp {

  public static void main(String[] args) {
    // create a spring container
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

    // retrieve the bean from spring container
    Coach theCoach = context.getBean("myCoach", Coach.class);

    // call methods on the bean
    System.out.println(theCoach.getWorkout());

    // close the context
    context.close();

  }  
}
```

Now we can configure the application via the applicationContext.xml, _rather than the source code_, eg:

_in applicationContext.xml_
```java
<beans>
  <bean id="myCoach" class="package.name.TrackCoach">
</beans>
```
