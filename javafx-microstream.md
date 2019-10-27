## Storing JavaFX Properties with MicroStream Java-Native Persistence

This article is about my personal experience with storing JavaFX properties and especially about exploring new framework:
[MicroSream Java-Native Persistence](https://microstream.one)

JavaFX has the concept of properties which are mutable objects that can have listeners registered on them or one property could be bound to another to synchronize values.
For example this is a declaration of a boolean property:

```java
private BooleanProperty active = new SimpleBooleanProperty();
```

Properties are often also used with objects that serve primarily as data holders. It works fine but a problem arises when we need to store an object with such properties. We want to store just the value inside the property and not the additional fields.

### Storing JavaFX Properties


| Approach                               | Pros                                              | Cons                                                               |
| ---------------------------------------| --------------------------------------------------| -------------------------------------------------------------------|
| Separate class                         | Clear separation                                  | Needs a separate class                                             |
|                                        | Can be used on separate thread                    | Necessary to keep in sync with the properties                      |
|                                        | Can be serialized                                 | Needs extra fields                                                 |
| Separate fields in the same class      | Can be serialized                                 | Necessary to keep in sync with the properties                      |
| Getters and setters                    | Simple, no additional fields                      | Cannot be used with serialization without additional settings      |

It is possible to use various technologies implemented by frameworks or libraries. These are some of them with their pros and cons regarding storing JavaFX properties on the local storage.

### Storage Technologies

| Technology                          | Pros                                                            | Cons
| ----------------------------------- | --------------------------------------------------------------- |----------------------------------------------------------------------- |
| Java serialization                  | Simple                                                          | Necessary to store the whole object graph or store into separate files |
|                                     | No extra dependencies                                           | Does not call the default constructor for initialization               |
|                                     | Possible circular object references                             | Does not use getters and setters seamlessly                            |
|                                     | No annotations needed                                           |                                                                        |
| XML (JAXB)                          | Relatively simple, well known                                   | Necessary to store the whole object graph or store into separate files |
|                                     | Possible to use references with special methods and annotations | Requires using annotations and special methods                         |
|                                     | Can be used with fields and getters and setters                 |                                                                        |
|                                     | Calls the default constructor                                   |                                                                        |
| JSON (JSON-B, GSON)                 | Like XML but without annotations                                | Like XML but more difficult to manage circular references.             |
|                                     |                                                                 | Some JSON libraries uses only fields and not getters and setters       |
| NOSQL (JSON documents)              | Relatively lightweight                                          | JSON documents are not very good to manage object references.          |
|                                     | Can be used with getters and setters                                 |                                                                        |
| JPA + RDBMS                         | Solid known technology                                          | Requires annotations                                                   |
|                                     | Can be used with fields and getters and setters                 | Does automatic field enrichment                                        |
|                                     |                                                                 | Usually better to create separate entity classes                       |
|                                     |                                                                 | Rather a lot of dependencies                                           |
|                                     |                                                                 | Slower startup                                                         |
| MicroStream Java-Native Persistence | Like native Java serialization                                  | Does not call the default constructor for initialization               |
|                                     | Possible to store objects separately                            | Does not use getters and setters seamlessly                            |
|                                     | No annotations needed                                           |                                                                        |
|                                     | Possible circular object references                             |                                                                        |

