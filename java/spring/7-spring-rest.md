New annotation @RestController is extension of @Controller
Spring REST automatically converts JSON for us

```java
@RestController
@RequestMapping("/test")
public class DemoRestController {

  @GetMapping("/hello")
  public String getHello() {
    return "hello";
  }

}
```

### add maven dependency for Spring MVC and Jackson project

### add code for All Java Config @Configuration

Make config class

```Java
@Configuration
@EnableWebMvc
@ComponentScan("com.package.name")
public class DemoAppConfig {
}
```

### add code for All Java Config Servlet Initialiser

```java
public class MySpringMvcDispatcherServletInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

  @Override
  protected Class<?>[] getRootConfigClasses() {
    return null;
  }

  @Override
  protected Class<?>[] getServletConfigClasses() {
    // wire in cconfig class from above
    return new Class[] { DemoAppConfig.class };
  }

  @Override
  protected String[] getServletMappings() {
    // map to root path
    return new String[] { "/"};
  }


}
```

### create Spring REST service using @RestController

```java
@RestController
@RequestMapping("/test")
public class DemoRestController {

  @GetMapping("/hello")
  public String getHello() {
    return "hello";
  }

}
```
