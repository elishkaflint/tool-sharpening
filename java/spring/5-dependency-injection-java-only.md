# Java only Spring config (ie. no xml)

3 ways to configure spring container

1. full xml config
2. xml component scan
3. java config class

## Steps

### Create java class and annotate as @Configuration
### Add component scanning support @ComponentScan (optional)
### Read Spring Java config class
### Retrieve bean from Spring container


```java
@Configuration
@ComponentScan("package.name")
public class SportConfig

  @Bean
  public FortuneService happyFortuneService() {
    return new HappyFortuneService();
  }

  @Bean
  public Coach swimCoach() {
    SwimCoach mySwimCoach = new SwimCoach(happyFortuneService);
    return mySwimCoach;
  }

}

// in main class, new object for loading the context
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SportConfig.class);

Coach theCoach = context.getBean("swimCoach", Coach.class);

```
