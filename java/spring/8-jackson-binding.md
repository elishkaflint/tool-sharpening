## Java JSON Data Binding

Data binding is the process of converting JSON data to a Java POJO
aka mapping, serialisation / deserialisation

Spring uses Jackson behind the scenes automatically behind the scenes.

Jackson example:

We create a POJO with setter methods

#### JSON to Java POJO

Jackson calls the setter methods and creates the POJO

```java
import com.fasterxml.jackson.databind.ObjectMapper;

ObjectMapper mapper = new ObjectMapper();
Student student = mapper.readValue(new File("data/sample.json"), Student.class);

System.out.println("First name = " + student.getFirstName());
```

#### Java POJO to JSON

Jackson calls the getter methods and creates the json

```java
import com.fasterxml.jackson.databind.ObjectMapper;

// convert JSON to POJO
ObjectMapper mapper = new ObjectMapper();
Student student = mapper.readValue(new File("data/sample.json"), Student.class);

// convert POJO to JSON
mapper.enable(SerializationFeature.INDENT_OUTPUT); // just to make it pretty
mapper.writeValue(new File("data/output.json"), myStudent);
```

#### Nested objects

POJO has to include objects as fields to represent nested objects

With an array, just set up a `String[]`

Remember to include getters and setters for this new array

#### Ignoring new properties

If json includes a new property, it will break the jackson deserialisation process becuase it won't know where to map it in our POJO

Annotate POJO with `@JsonIgnoreProperties(ignoreUnknown=true)`
