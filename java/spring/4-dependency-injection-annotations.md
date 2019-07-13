# Java Annotations

- added to classes
- provide metadata
- processed at compile time or runtime
- easier than xml

- Spring scans Java classes for annotations
- It automatically registers any beans it finds in the Spring container

### Example

#### Enable component scanning in Spring config file

```
<beans>
  <context:component-scan base-package="package.name" />
</beans>
```

#### Add @Component annotations to class

```java
@Component("thatSillyCoach")
public class TennisCoach implements Coach {

  @Override
  public String getWorkout() {
    return "Practice volleys";
  }

}
```

#### Same code as before in our  to retrieve the bean from the Spring container

```
Coach theCoach = context.getBean("thatSillyCoach", Coach.class);
```

Default beanId is class name with lower case eg TennisCoach => tennisCoach

### Dependency Injection with Annotations and Autowiring

Now we will inject fortuneService using autowiring

Spring will look for a class that matches the property by class or interface, then will inject it automatically

To inject FortuneService into a Coach implementation using the @Autowired annotation:
- Spring will scan @Components
- See if anyone implements FortuneService interface?
- If so, inject it eg. HappyFortuneService

1. Constructor injection

#### Define the dependency interface and class

```java
public interface FortuneService {
  public String getFortune();
}

@Component
public class HappyFortuneService implements FortuneService {
  public String getFortune() {
    return "Today is your lucky day";
  }
}
```

#### Create a constructor class

```java
@Component
public class TennisCoach implements Coach {

  private FortuneService fortuneService;

  @Autowired
  public TennisCoach(FortuneService fortuneService) {
    this.fortuneService = fortuneService;
  }

}
```

MyApp main method is the same, we are just using a different way of setting up our dependencies.

Now when we retrieve our Coach object, we will get it along with all its dependencies.

NOTE As of Spring Framework 4.3, an @Autowired annotation on such a constructor is no longer necessary if the target bean only defines one constructor to begin with. However, if several constructors are available, at least one must be annotated to teach the container which one to use.


2. Setter injection

Very similar to constructor injection. Just follow the same principle.

Add the @Autowired annotation to the setter method.

3. Inject dependencies by calling any method

```java
@Component
public class TennisCoach implements Coach {

  private FortuneService fortuneService;

  public TennisCoach() {}

  @Autowired
  public void doSomeCrazyStuff(FortuneService fortuneService) {
    this.fortuneService = fortuneService;
  }

}
```

4. Field injection

```java
@Component
public class TennisCoach implements Coach {

  @Autowired
  private FortuneService fortuneService;

  public TennisCoach() {}

  // no need for setter methods or constructor, uses Java technology called reflections and does it automatically

}
```

### Which injection type to use?

Choose a style and stay consistent!!

They all give the same functionality.

Even in Spring docs, they say the same.


### Qualifiers

What happens if we have multiple implementations of an interface like FortuneService?
Use @Qualifier annotation, on any type of annotation.

```java
@Component
public class TennisCoach implements Coach {

  @Autowired
  // use qualifier with bean id (default is lower case class name if nothing specified)
  @Qualifier("HappyFortuneService")
  private FortuneService fortuneService;

  public TennisCoach() {}

}
```

### Injecting properties file using annotations

properties file - sport.properties
```
foo.email=myeasycoach@luv2code.com
foo.team=Silly Java Coders
```

```
<beans>
  <context:component-scan base-package="package.name" />
  <context:property-placeholder location="classpath:sport.properties"/>
</beans>
```

```java
@Component
public class SwimmingCoach implements Coach {

  @Value("${foo.email}")
  private String email;

  @Value("${foo.team}")
  private String team;

}
```
