<br />

<p style="text-align:center; width:100%">
  <img align="center" src="gopher.png" />
  <img align="center" src="gopher.png" />
  <img align="center" src="gopher.png" />
  <img align="center" src="gopher.png" />
  <img align="center" src="gopher.png" />
</p>

---

# Design Patterns In Golang
1. [Creational](#Creational)
2. [Structural](#Structural)
3. [Behavioral](#Behavioral)
4. [Synchronization Patterns](#Synchronization-Patterns)
5. [Messaging Patterns](#Messaging-Patterns)

---
<br />


## Creational

| Pattern | Description |
| ----------- | ----------- |
| [Singleton](/creational/singleton.md) | Restricts the instantiation of a class to one "single" instance.  |
| [Factory Method](/creational/factory_method.md) | Provides a way to create objects without having to specify the exact type of the object that will be created.  |
| [Abstract Factory](/creational/abstract_factory.md) | Provides an interface for creating families of related or dependent objects without specifying their concrete classes. |
| [Builder](/creational/builder.md) | Separates the construction of a complex object from its representation, allowing the same construction process to create various representations. |
| [Prototype](/creational/prototype.md) | Provides a way to create copies of objects.  |
| [Object Pool](/creational/object_pool.md) | Is a set of initialized expensive objects that are ready to use |

---

## Structural

| Pattern | Description |
| ----------- | ----------- |
| [Adapter](/structural/adapter.md) | Converts an existing incompatible interface to aother interface that clients expect without touching the source code.  |
| [Decorator](/structural/decorator.md) | Allows to add more functionality to an existing object without modifying it's code.  |
| [Proxy](/structural/proxy.md) | Is a wrapper or agent object that is being called by the client to access the real serving object behind the scenes. |
| [Facade](/structural/facade.md) | Helps to hide the complexities of a system and provides a simple interface to clients. |
| [Bridge](/structural/bridge.md) | Decouples abstraction from implementation so that the two can vary independently |
| [Composite](/structural/composite.md) | Describes a group of objects that are treated the same way as a single instance of the same type of object. Lets clients treat individual objects and compositions uniformly |
| [Flyweight](/structural/flyweight.md) | Helps to minimize memory usage by sharing as much data as possible with other similar objects. |


## Behavioral

| Pattern | Description |
| ----------- | ----------- |
| [Chain Of Responsibility](/behavioral/chain_of_responsibility.md) | Provides a way that each incoming request is passed through a chain of processing objects.  |
| [Command](/behavioral/command.md) | Helps to encapsulate all information needed to perform an action at a later time. |
| [Iterator](/behavioral/iterator.md) | Provides a function that return the next value in a sequence each time it is called.  |
| [Mediator](/behavioral/mediator.md) | Defines an object that encapsulates how a set of objects interact. |
| [Observer](/behavioral/observer.md) | Provides a way to notify other objects when a state change ocuures in an object.|
| [Memento](/behavioral/memento.md) | Provides the ability to restore an object to its previous states |
| [State](/behavioral/state.md) | Provides a way to alter an object's behavior when its internal state changes.  |
| [Strategy](/behavioral/strategy.md) | Provides a way to change the behavior of an object at run time without any change in the class of that object. |
| [Template Method](/behavioral/template_method.md) | Provides a way to implement the invariant parts of an algorithm once and leave it up to subclasses to implement the behavior that can vary |
| [Visitor](/behavioral/visitor.md) | Provides a way to define new operations to be performed on the elements of an struct without modifying the struct. |

## Synchronization Patterns 

| Pattern | Description |
| ----------- | ----------- |
| [Mutex](/synchronization/lock_mutex.md) | A concurrency lock mechanism to prevent the race conditions. |
| [RWMutex](/synchronization/read_write_lock.md) | Provides a lock that can be held by an arbitrary number of readers or a single writer. |
| [Monitor](/synchronization/monitor.md) | Provides a way to make a goroutine wait till some event occurs. |
| [Semaphore](/synchronization/semaphore.md) | Semaphore provides a way to limit the access to at most N threads |

## Messaging Patterns 

| Pattern | Description |
| ----------- | ----------- |
| [Fan-In](/messaging/fan_in.md) | A mechanism in which a function can read from multiple inputs |
| [Fan-Out](/messaging/fan_out.md) | A mechanism in which multiple functions can read from the same channel until that channel is closed |
| [Future](/messaging/future.md) | Is a value that is going to be set asynchronously.  |

