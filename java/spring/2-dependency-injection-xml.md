# Dependency Injection using xml

We want a CarFactory which gives us a Car object. The car object depends on having eg. chassis, exhaust, wheels etc. The CarFactory assembles the object for us _by injecting all of these dependencies into the car_ - we don't have to worry about it. We outsource the construction of the object to an external entity.

In Spring world, Spring has an ObjectFactory. Our Coach object has dependencies which the ObjectFactory will assemble for us.

## Scenario

Coach will now also provide a fortune, depending on a FortuneService to do so

## Injection types

### 1. Constructor Injection

#### Define the dependency interface and its class

```java
public interface FortuneService {

  public String getFortune();

}

public class HappyFortuneService implements FortuneService {

  @Override
  public String getFortune() {
    return "Today is your lucky day";
  }

}
```

#### Create a constructor in your class for injections

```java

public interface Coach {

  public String getWorkout();

  public String getFortune();

}

class BaseballCoach implements Coach {

  private FortuneService fortuneService;

  public BaseballCoach(FortuneService fortuneService) {
    this.fortuneService = fortuneService;
  }

  @Override
  public String getWorkout() {
    return "practice passes";
  }

  @Override
  public String getFortune() {
    return fortuneService.getFortune();
  }

}

class TrackCoach implements Coach {

  private FortuneService fortuneService;

  public TrackCoach(FortuneService fortuneService) {
    this.fortuneService = fortuneService;
  }

  @Override
  public String getWorkout() {
    return "go for a run";
  }

  @Override
  public String getFortune() {
    return fortuneService.getFortune();
  }

}
```

#### Configure the dependency injection in Spring config file

_in applicationContext.xml_
```java
<beans>
  <bean id="myFortuneService" class="package.name.HappyFortuneService"></bean>
  <bean id="myCoach" class="package.name.BaseballCoach">
    <constructor-arg ref="myFortuneService" /></bean>
</beans>
```

Behind the scenes, this is what Spring is doing:
- HappyFortuneService myFortuneService = new HappyFortuneService();
- BaseballCoach myCoach = new BaseballCoach(myFortuneService);

```java
class MyApp {

  public static void main(String[] args) {
    // create a spring container
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

    // retrieve the bean from spring container, ***now it comes with the right dependencies associated***
    Coach theCoach = context.getBean("myCoach", Coach.class);

    // call methods on the bean
    System.out.println(theCoach.getWorkout());
    // NEW METHOD
    System.out.println(theCoach.getFortune());

    // close the context
    context.close();
  }  
}
```

Now in config file we can just swap out BaseballCoach and drop in TrackCoach and it will all still work.

### 2. Setter Injection

##### Create setter methods in class for dependency injection

```java
public class CricketCoach implements Coach {

  private FortuneService fortuneService;

  public CricketCoach() {
  }

  public void setFortuneService (FortuneService fortuneService) {
    this.fortuneService = fortuneService;
  }

  //@override methods for fortune and workout

}
```

##### Change the config file

_in applicationContext.xml_
```java
<beans>

  <bean id="myFortuneService" class="package.name.HappyFortuneService"></bean>

  // constructor injection
  <bean id="myCoach" class="package.name.BaseballCoach">
    <constructor-arg ref="myFortuneService" />
  </bean>

  // setterInjection, Spring will look for a public setter method like setFortuneService
  <bean id="myCricketCoach" class="package.name.CricketCoach">
    <property name="fortuneService" ref="myFortuneService" />
  </bean>

</beans>
```

This is what Spring is doing with this config:

```java
HappyFortuneService myFortuneService = new HappyFortuneService();
CricketCoach myCricketCoach = new CricketCoach();
myCricketCoach.setFortuneService(myFortuneService);
```

```java
class MyApp {

  public static void main(String[] args) {
    // create a spring container
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

    // retrieve the bean from spring container, ***now it comes with the right dependencies associated***
    Coach theCoach = context.getBean("myCricketCoach", Coach.class);

    // call methods on the bean
    System.out.println(theCoach.getWorkout());
    // NEW METHOD
    System.out.println(theCoach.getFortune());

    // close the context
    context.close();
  }  
}
```

### 3. Injecting Literal Values

CricketCoach now gets a team name and an email address

#### Create setter methods in your class

```
public class CricketCoach implements Coach {

  private String emailAddress;
  private String team;

  // add getters and setters here

  ...

}
```

### Change the config file

_in applicationContext.xml_
```java
<beans>

  <bean id="myFortuneService" class="package.name.HappyFortuneService"></bean>

  // constructor injection
  <bean id="myCoach" class="package.name.BaseballCoach">
    <constructor-arg ref="myFortuneService" />
  </bean>

  // setterInjection, Spring will look for a public setter method like setFortuneService
  <bean id="myCricketCoach" class="package.name.CricketCoach">
    <property name="fortuneService" ref="myFortuneService" />
    // inject other properties
    <property name="emailAddress" value="email@address.com" />
    <property name="team" value="Team Name" />
  </bean>

</beans>
```

```java
class MyApp {

  public static void main(String[] args) {
    // create a spring container
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");

    // retrieve the bean from spring container, ***now it comes with the right dependencies associated***
    // note now we have to bring in CricketCoach because the Coach interface doesn't have getEmailAddress or getName
    CricketCoach theCoach = context.getBean("myCricketCoach", CricketCoach.class);

    // call methods on the bean
    System.out.println(theCoach.getWorkout());
    System.out.println(theCoach.getFortune());

    // print out properties
    System.out.println(theCoach.getEmailAddress());
    System.out.println(theCoach.getName());

    // close the context
    context.close();
  }  
}
```

### 3. Injecting Values from a properties file

In above example, values were hardcoded in the config file, now we want to read these from a propertiesfile

#### Create properties file

Create a file eg sport.properties

```
foo.email=email@address.com
foo.team=teamName
```

#### Load properties file in Spring config

_in applicationContext.xml_
```java
<context:property-placeholder location="classpath:sport.properties"/>
```

#### Reference values from properties file

_in applicationContext.xml_
```java
<context:property-placeholder location="classpath:sport.properties"/>

<beans>
  // setterInjection, Spring will look for a public setter method like setFortuneService
  <bean id="myCricketCoach" class="package.name.CricketCoach">
    <property name="fortuneService" ref="myFortuneService" />
    // inject other properties using properties file
    <property name="emailAddress" value="${foo.email}" />
    <property name="team" value="${foo.team}" />
  </bean>

</beans>
```
