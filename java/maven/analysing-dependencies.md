## Looking for unused dependencies in Maven projects

Add the maven dependency plugin:
```
<plugin>
   <artifactId>maven-dependency-plugin</artifactId>
   <version>2.8</version>
   <executions>
       <execution>
           <id>analyze</id>
           <goals>
               <goal>analyze-only</goal>
           </goals>
           <configuration>
               <failOnWarning>false</failOnWarning>
               <outputXML>true</outputXML>
           </configuration>
       </execution>
   </executions>
</plugin>
```

Run `mvn dependency:analyze` to see unused declared dependencies and used undeclared dependencies

Used undeclared are usually those which have been brought in via other dependencies. It's worth declaring them so that they are transparent / you can specify their versions. I.e. your project A uses library B which depends on library C. If you also use C directly You don't have to declare it - maven will automatically pull it in through the transitive dependency. But without declaring the dependency you don't have any control over the version. E.g. if B decides to update to the next version of C your project A may break unless the update is backwards compatible

For more info run `mvn dependency:tree` to see the hierarchy of dependencies.

To exclude a transitive dependency from a library you are bringing in (eg. to avoid version conflict?) do something like this:

```
<dependency>
  <groupId>group.id</groupId>
  <artifactId>artifact-id</artifactId>
  <version>version</version>
  <exclusions>
      <exclusion>
          <groupId>org.jboss.resteasy</groupId>
          <artifactId>resteasy-jaxb-provider</artifactId>
      </exclusion>
      <exclusion>
          <groupId>org.jboss.resteasy</groupId>
          <artifactId>resteasy-multipart-provider</artifactId>
      </exclusion>
  </exclusions>
</dependency>
```

Remember - when you remove a dependency from the pomfile, it may remain in your external libraries because it is a used undeclared dependency (transitive from another dependency), use the tools above to diagnose it.
