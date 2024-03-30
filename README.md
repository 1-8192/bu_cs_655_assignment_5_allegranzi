# bu_cs_655_assignment_5_allegranzi
| CS-665       | Software Design & Patterns |
|--------------|----------------------------|
| Name         | ALESSANDRO ALLEGRANZI      |
| Date         | 04/04/2024                 |
| Course       | Spring                     |
| Assignment # | 5                          |

## Summary

The goal of this assignment is to read an existing open source project and identify 3 software design patterns
we have studied in class. I chose the [Caffeine](https://github.com/ben-manes/caffeine) java in-memory cache repository
as the subject for this assignment. I have actually used Caffeine before at work for a Spring Boot service's caching
layer, and was interested to take a closer look at the source code.

## Patterns used

### Adapter Pattern

#### Observations
The first software design pattern that jumped out to me from the repository's README was the adapter pattern.
The documentation refers to a Guava cache interface adapter, so I started looking into the files in the guava package.

1. Identify the class or interface that plays a specific role in the design pattern. For
   example, if the design pattern is the "Strategy" pattern, identify which class plays the role
   of the context, which class plays the role of the strategy, etc.
2. Describe the collaboration between the roles as found in the source code. For example,
   if the "Strategy" pattern is used, describe how the context class delegates the actual
   implementation of an algorithm to the strategy class.
3. Identify where another class can be easily added and describe why it is useful to do so.
   For example, if the design pattern is the "Observer" pattern, identify where another
   observer class can be added and explain why it is easy and useful to add another
   observer class to the project.


#### UML Diagram


### Builder Pattern

1. Identify the class or interface that plays a specific role in the design pattern. For
   example, if the design pattern is the "Strategy" pattern, identify which class plays the role
   of the context, which class plays the role of the strategy, etc.
2. Describe the collaboration between the roles as found in the source code. For example,
   if the "Strategy" pattern is used, describe how the context class delegates the actual
   implementation of an algorithm to the strategy class.
3. Identify where another class can be easily added and describe why it is useful to do so.
   For example, if the design pattern is the "Observer" pattern, identify where another
   observer class can be added and explain why it is easy and useful to add another
   observer class to the project.

### Factory Pattern
