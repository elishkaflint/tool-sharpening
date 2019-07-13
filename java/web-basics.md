# Web-based Java

JAX-RS (Java API for RESTful Web Services) is a set of Java API that provides support in creating REST APIs.

JAX-RS annotations act as placeholders for the specification, then frameworks such as Jersey and Resteasy provide an implementation.

> RESTEasy is the JBoss provided portable implementation of JAX-RS specification, and can be used to create a simple RESTful Web service.

### Web.xml

Java web applications use a deployment descriptor file to determine how URLs map to servlets, which URLs require authentication, and other information. This file is named web.xml, and resides in the app's WAR under the WEB-INF/ directory. web.xml is part of the servlet standard for web applications.

This is good documentation with examples of web.xml files used to map different URL paths to different servlet classes https://cloud.google.com/appengine/docs/standard/java/config/webxml

### Servlets

A Servlet is simply a Java class which can handle HTTP requests. Servlets are usually used to implement web applications but there are also various frameworks which operate on top of servlets to give a higher level of abstraction than a servlet can provide. Servlets have 3 main life cycle methods - init(), service(), destroy(). They are initialised at the point that a request comes into the application.

Servlets run in servlet containers (one of the best-known is Tomcat) which handle the complexity of the networking side (eg. parsing http request, connection handling). The servlet container is responsible for the creation, execution and destruction of multiple servlets to handle these requests.

This is how a request is processed by a servlet container and web server:

- Web server receives HTTP request
- Web server forwards the request to servlet container
- The servlet is dynamically retrieved and loaded into the address space of the container, if it is not in the container.
- The container invokes the init() method of the servlet for initialization(invoked once when the servlet is loaded first time)
- The container invokes the service() method of the servlet to process the HTTP request, i.e., read data in the request and formulate a response. The servlet remains in the container’s address space and can process other HTTP requests.
- Web server return the dynamically generated results to the correct location

https://dzone.com/articles/what-servlet-container

#### Servlet Listener

A Servlet Listener is used for listening to events in a servlet container, such as when you create a session, or place an attribute in an session. To subscribe to these events you can configure a listener in web.xml, for example HttpSessionListener.

Listeners are targeted for lifecycle changes, without having to have a client request coming in. So for one client request, there could be many more lifecycle events may happen before the request is disposed of. Example: You want to log all the sessions that timeout. Please note that SesionTimeout is a lifecycle event, which can happen without having the user to do anything. For such a scenario, a listener will be appropriate.

#### Servlet Filter

A Servlet Filter is used for monitoring requests and responses from client to the servlet, or to modify the request and response, or to audit and log. Where you need to interact only at the request / response level, rather than the servlet container lifecycle level, a filter is adequate.


### Using Google Guice

https://github.com/google/guice/wiki/Servlets

Import the core Guice and Guice Servlet Extensions libraries into the pom.xml:
```
<dependency>
    <groupId>com.google.inject</groupId>
    <artifactId>guice</artifactId>
    <version>${guice.version}</version>
</dependency>
<dependency>
    <groupId>com.google.inject.extensions</groupId>
    <artifactId>guice-servlet</artifactId>
    <version>${guice.version}</version>
</dependency>
```

Then all you have to do is add a Filter and map it to all possible requests in the web.xml:
```
<filter>
   <filter-name>guiceFilter</filter-name>
   <filter-class>com.google.inject.servlet.GuiceFilter</filter-class>
 </filter>

 <filter-mapping>
   <filter-name>guiceFilter</filter-name>
   <url-pattern>/*</url-pattern>
 </filter-mapping>
 ```

Read next
https://github.com/google/guice/wiki/ServletModule
https://github.com/google/guice/wiki/BindingAnnotations


### Providers and Resources

From SO on Providers:

Providers are a simply a way of extending and customizing the JAX-RS runtime. You can think of them as plugins that (potentially) alter the behavior of the runtime, in order to accomplish a set of (program defined) goals.

Providers are not the same as resources classes, they exist, conceptually, at a level in-between resources classes and the JAX-RS implementation. If it helps, you can think of them in the same light as device drivers (existing between user and kernel space). This is a broad generalization.

In our classes we annotate our Mapper classes with @Provider notation.

There are three classes of providers defined by the current JAX-RS specification. The commonality between them is that all providers must be identified by the @Provider annotation and follow certain rules for constructor declaration. Apart from that, different provider types may have additional annotations, and will implement different interfaces.

#### Entity Providers

These providers control the mapping of data representations (like XML, JSON, CSV) to their Java object equivalents.

#### Context Providers

These providers control the context that resources can access via @Context annotations.

#### Exception Providers

These providers control the mapping of Java exceptions to a JAX-RS Response instance.

Your runtime will come with a number of predefined providers that will be responsible for implementing a base level of functionality (e.g for mapping to and from XML, translating the most common exceptions etc etc). You can also create your own providers as needed.

The JAX-RS specification is a good reference for reading up on these different provider types and what they do (see Chapter 4).
