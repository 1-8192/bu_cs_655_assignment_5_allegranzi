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
as the subject for this assignment. Caffeine is a near optmial, in-memory cache modeled after Google's Guava java cache
API. I have actually used Caffeine before at work for a Spring Boot service's caching layer, and was interested to take 
a closer look at the source code.

Overall, the documentation is quite extensive, and the source code is very clear. However, it is also a decently-sized project. 
The classes are quite large and complex, as well, so I simplified a lot of the information for the diagrams to focus
on displaying the design pattern relationships. I started my research by searching for design pattern keywords like 
"adapter" and "factory" using the IDE's search function, and looking at the classes that came up. I then traced dependencies 
and relationships between classes to build out the pattern structures. The results are below. The corresponding UML 
diagrams for the design patterns are included in this project in the [diagrams](diagrams/) directory.

## Patterns used

### Adapter Pattern

#### Observations

The first software design pattern that jumped out to me from the repository's README was the adapter pattern.
The pattern "converts the interface of a class into another interface clients expect. Adapter lets
classes work together that couldn't otherwise because of incompatible interfaces" (Gang of Four text).
The documentation refers to a Guava cache interface adapter, so I started looking into the files in the guava package. The repo
provides two adapters for the Guava cache interface and the Guava LoadingCache interface. The LoadingCache provides some
specialized methods for loading values into cache.

The class comments mention "facades," but they seem to be referring to the target interface rather than the facade
pattern we discussed in class. They are not covering a complex system of classes, but rather adapting the Google Guava Cache
interface to work with Caffeinca cache classes.

1. Identify the class or interface that plays a specific role in the design pattern. For
   example, if the design pattern is the "Strategy" pattern, identify which class plays the role
   of the context, which class plays the role of the strategy, etc.

Adaptee: com.github.benmanes.caffeine.cache.Cache is the existing interface that needs to be adapted to fit into the Guava Cache interface.
Target: Cache (from the google Guava package) is the interface that clients interact with, defining the desired functionality.
Adapter: CaffeinatedGuavaCache is the adapter class that bridges between the Cache interface and the com.github.benmanes.caffeine.cache.Cache implementation.

The LoadingCache version is the same as above but features the LoadingCache classes from the respective packages and external
Guava libraries.


2. Describe the collaboration between the roles as found in the source code. For example,
   if the "Strategy" pattern is used, describe how the context class delegates the actual
   implementation of an algorithm to the strategy class.

CaffeinatedGuavaCache implements the Cache interface (from Google Guava) and internally uses an instance of com.github.benmanes.caffeine.cache.Cache for actual caching operations.
Methods in CaffeinatedGuavaCache delegate functionality to the corresponding methods in the com.github.benmanes.caffeine.cache.Cache instance.
The adapter class translates calls from the Cache interface to the methods of the com.github.benmanes.caffeine.cache.Cache memeber variable instance.


3. Identify where another class can be easily added and describe why it is useful to do so.
   For example, if the design pattern is the "Observer" pattern, identify where another
   observer class can be added and explain why it is easy and useful to add another
   observer class to the project.

Another class implementing the Cache interface can be added easily to the project. This class could provide caching functionality using a different caching library or mechanism.
Adding another implementation of the Cache interface allows for flexibility in choosing different caching strategies or libraries without affecting the existing code that depends on the Cache interface. 
This promotes modularity and maintainability.

#### UML Diagram

[Adapter pattern diagram](diagrams/adapter.pdf)

### Decorator or Observer

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

#### Observations

I found a few instances of the Factory method pattern used in the Caffeine repository. I focused on the LocalCacheFactory
Interface for this assignment. The Factory Method pattern "defines an interface for creating an object, but lets
subclasses decide which class to instantiate" (Lecture Notes). The implementation in the code looked more complex than examples
we have seen in class. The Interface has a nested final class that implements the interface inside its own file, for example.

1. Identify the class or interface that plays a specific role in the design pattern. For
   example, if the design pattern is the "Strategy" pattern, identify which class plays the role
   of the context, which class plays the role of the strategy, etc.

Factory Interface: LocalCacheFactory is the interface defining the method for creating instances of BoundedLocalCache.
It abstracts the process of object creation.
Concrete Factory: MethodHandleBasedFactory is a concrete implementation of the LocalCacheFactory interface. 
It provides the actual implementation for creating instances of BoundedLocalCache.
Product: BoundedLocalCache is the product class being created by the factory. 
It represents the objects that the factory produces.

2. Describe the collaboration between the roles as found in the source code. For example,
   if the "Strategy" pattern is used, describe how the context class delegates the actual
   implementation of an algorithm to the strategy class.

The LocalCacheFactory interface defines the method newInstance() for creating instances of BoundedLocalCache.
MethodHandleBasedFactory implements this interface and provides its own implementation of newInstance(), utilizing 
MethodHandles for dynamic method invocation. The loadFactory() method in the LocalCacheFactory interface is responsible 
for dynamically loading the appropriate factory implementation based on the configuration provided.

3. Identify where another class can be easily added and describe why it is useful to do so.
   For example, if the design pattern is the "Observer" pattern, identify where another
   observer class can be added and explain why it is easy and useful to add another
   observer class to the project.

Another concrete factory class implementing the LocalCacheFactory interface can be added easily to the project. This could allow for 
different strategies in creating instances of BoundedLocalCache, such as using reflection-based factories, configuration-driven factories, 
or factories based on different performance considerations. Adding another concrete factory class enhances flexibility and 
maintainability, as it allows for the easy extension of the factory system to accommodate different object creation 
strategies without modifying existing code. Finally, adding more products or configurations is easy to do and is just a matter
of adding another case in the factory class.

#### UML Diagram

[Factory Method pattern diagram](diagrams/factory.pdf)