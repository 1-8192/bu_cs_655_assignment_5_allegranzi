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
The Adapter pattern is a structural pattern that "converts the interface of a class into another interface clients expect. Adapter lets
classes work together that couldn't otherwise because of incompatible interfaces" (Gang of Four text).
The documentation refers to a Guava cache interface adapter, so I started looking into the files in the guava package. The repo
provides two adapters for the Guava cache interface and the Guava LoadingCache interface. The LoadingCache provides some
specialized methods for loading values into cache.

The class comments mention "facades," but they seem to be referring to the target interface rather than the facade
pattern we discussed in class. They are not covering a complex system of classes, but rather adapting the Google Guava Cache
interface to work with Caffeine cache classes.

1. Identify the class or interface that plays a specific role in the design pattern. For
   example, if the design pattern is the "Strategy" pattern, identify which class plays the role
   of the context, which class plays the role of the strategy, etc.

Adaptee: com.github.benmanes.caffeine.cache.Cache is the existing interface that needs to be adapted to fit into the Guava Cache interface.

Target: Cache (from the google Guava package) is the interface that clients interact with, defining the desired functionality.

Adapter: CaffeinatedGuavaCache is the adapter class that bridges between the Cache interface and the com.github.benmanes.caffeine.cache.Cache implementation.

The LoadingCache version is the same as above but features the LoadingCache classes from the respective packages and external
Guava libraries. I only diagrammed the CaffeinatedGuavaCache relationships as it is enough to diplay the adapter pattern
implementation.

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

### Proxy

#### Observations

The proxy design pattern is a structural pattern that "provides a surrogate or placeholder for another object to control 
access to it" (Gang of Four Text). The source code conveniently uses "proxy" to name the class, so searching for the pattern 
was straightforward. The LoadingCacheProxy acts as a proxy for the real cache implementation (LoadingCache). 
It intercepts method calls to the cache, allowing for additional actions such as exception handling, statistics tracking, 
and asynchronous operation management. This abstraction enables the implementation of cross-cutting concerns while 
keeping the core cache logic intact, facilitating easier maintenance and scalability of the caching system.

1. Identify the class or interface that plays a specific role in the design pattern. For
   example, if the design pattern is the "Strategy" pattern, identify which class plays the role
   of the context, which class plays the role of the strategy, etc.

Proxy Class: LoadingCacheProxy acts as the proxy for the real cache implementation. It provides a similar interface to the real cache and controls access to it.

Subject Interface: The Cache interface defines the common methods that both the real cache and the proxy must implement.

Real Subject: The LoadingCache class is the real cache implementation that LoadingCacheProxy delegates to.


2. Describe the collaboration between the roles as found in the source code. For example,
   if the "Strategy" pattern is used, describe how the context class delegates the actual
   implementation of an algorithm to the strategy class.

LoadingCacheProxy extends CacheProxy, which provides basic functionality for interacting with the cache.
The proxy delegates most of the method calls to the real cache instance (LoadingCache) and adds additional functionality such as handling exceptions, updating statistics, and managing asynchronous operations.
Methods in LoadingCacheProxy intercept calls to the real cache, allowing for additional actions to be taken before or after the method invocation.

3. Identify where another class can be easily added and describe why it is useful to do so.
   For example, if the design pattern is the "Observer" pattern, identify where another
   observer class can be added and explain why it is easy and useful to add another
   observer class to the project.

Another proxy class could be added to implement a different caching strategy or to add additional functionality specific to certain use cases.
For example, a LoggingCacheProxy class could log all cache interactions, providing useful debugging and monitoring information.
Adding another proxy class allows for customization and extension of cache behavior without modifying the existing cache implementation, promoting modularity and maintainability.

### Factory Pattern

#### Observations

I found a few instances of the Factory method pattern used in the Caffeine repository. I focused on the LocalCacheFactory
Interface for this assignment. The Factory Method pattern is a creational pattern that "defines an interface for creating an object, but lets
subclasses decide which class to instantiate" (Lecture Notes). The implementation in the code looked more complex than examples
we have seen in class. The Interface has a nested final class that implements the interface inside its own file, for example. 

Additionally, the BoundedLocalCache class, the concrete product for the pattern, is itself an abstract class that implements the LocalCache
product interface. The BoundedLocalCache itself also has a builder method that takes in configurations, so the overall architecture
looks more complex than examples we've seen so far.

1. Identify the class or interface that plays a specific role in the design pattern. For
   example, if the design pattern is the "Strategy" pattern, identify which class plays the role
   of the context, which class plays the role of the strategy, etc.

Factory Interface: LocalCacheFactory is the interface defining the method for creating instances of BoundedLocalCache.
It abstracts the process of object creation.

Concrete Factory: MethodHandleBasedFactory is a concrete implementation of the LocalCacheFactory interface. 
It provides the actual implementation for creating instances of BoundedLocalCache.

Product interface: LocalCache. The interface implemented by the concrete class.

Concrete Product: BoundedLocalCache is the product class being created by the factory. 
It represents the objects that the factory produces.

2. Describe the collaboration between the roles as found in the source code. For example,
   if the "Strategy" pattern is used, describe how the context class delegates the actual
   implementation of an algorithm to the strategy class.

The LocalCacheFactory interface defines the method newInstance() for creating instances of BoundedLocalCache.
MethodHandleBasedFactory implements this interface and provides its own implementation of newInstance(), utilizing 
MethodHandles for dynamic method invocation. The loadFactory() method in the LocalCacheFactory interface is responsible 
for dynamically loading the appropriate factory implementation based on the configuration provided.

The above is all very generalized, as frankly the factory method logic is pretty complex and features many different
configuration options.

3. Identify where another class can be easily added and describe why it is useful to do so.
   For example, if the design pattern is the "Observer" pattern, identify where another
   observer class can be added and explain why it is easy and useful to add another
   observer class to the project.

Another concrete factory class implementing the LocalCacheFactory interface can be added easily to the project. This could allow for 
different strategies in creating instances of BoundedLocalCache, such as using reflection-based factories, configuration-driven factories, 
or factories based on different performance considerations. Adding another concrete factory class is easy and requires only
implenting the factory interface. It enhances flexibility and maintainability, as it allows for the easy extension of the 
factory system to accommodate different object creation strategies without modifying existing code. Finally, 
adding more products or configurations is easy to do and is just a matter of adding another case in the factory class.

#### UML Diagram

[Factory Method pattern diagram](diagrams/factory.pdf)