Useful article
https://blog.jayway.com/2014/07/04/integration-testing-a-spring-boot-application/

1. Use a server like Jetty to run up the application
2. Use Springboot Test (Spring Boot web application already comprises an embedded servlet container)
3. Use the Spring MVC Test Framework which allows you to verify application responses without starting a server

Make asseertions with TestRestTemplate and Junit Assert or RestAssured  (more bdd style)

https://www.petrikainulainen.net/programming/spring-framework/integration-testing-of-spring-mvc-applications-write-clean-assertions-with-jsonpath/
https://semaphoreci.com/community/tutorials/testing-rest-endpoints-using-rest-assured


1. Springboot test server + TestRestTemplate
- Have to run up test server
- Easy to make assertions on the response
- Syntax not as clean

2. Springboot test server + RestAssured
- Found it surprisingly inflexible
- Tests take double the time to run

3. Spring MVC Test
- No need to run up a test server
- Runs faster than tests using other frameworks
- Clear syntax, doesn't support given/when/then implicitly in the code but easy to  break steps down using comments
- Lots of good articles about usage
- Keeps us in the Spring ecosystem


RTH Test Frameworks

`Unit tests` - method level, quick tests, classes are isolated - Junit, Mockito, Wiremock

`Integration tests` - tests are run against the service which is running in a container, classes are not isolated, but calls to any external services (settlement-status or other) are mocked - Springboot Test, TestRestTemplate, Wiremock or MockBean (tbc)

`Deployment smoke tests` - a subset of integration tests are run against a deployed application, some external non-settlement-status services are mocked depending on the environment (eg. PCDP in dev1) - RestAssured, Wiremock

`Acceptance (or E2E) Tests` - tests are run against a deployed application, some external non-settlement-status services are mocked depending on the environment (eg. PCDP in dev1) and full business scenarios are tested - Cucumber, Selenium

So-called deployment smoke tests are something we might look to bring in in the future, potentially as part of our liveness and readiness probes.


 - I think we agree on what these are, method level quick tests.
integration or integrated tests - the service is running and we test all its classes at same time, the classes are integrated.  But the external interfaces are mocked
end to end tests - the service is also running but amongst its peers and all its external interfaces are pointed at real services

Each type of test gets gradually harder to write, slower to run and should become less specific. So unit tests can tests all the boundary conditions, integration tests test the input and output of the service and e2e are all about business scenarios.

This is basic breakdown we are aiming for.  So, unit tests are junit, mockito, wiremock on a junit rule and nothing running in a container.  Integration tests are junit but using a framework like rest assured, wiremock, and the service runs in a container.  e2e are cucumber, selenium and run against a live like deployment of all the services.

One other use we *could* have for integrated tests, in our case, is to test a deployed version of the service as well.  Because we tend not to have all external services available we could also use the integrated tests as smoke tests after deployment and as a way to check the environment, etc.  This is a stretch goal. (edited)
I think to remove confusion if we just focus on picking a framework for the integration (integrated) tests, where the key thing is that service is running in a container, then any other use we could put these tests to can be looked at later.
btw I’m not thinking of running all the integration tests against a remote server just a small subset.
