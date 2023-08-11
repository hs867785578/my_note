# Design Patterns

Design patterns are typical solutions to commonly occurring problems in software design. They are likely pre-made blueprints that you can customize to solve a recurring design problem in your code.

You can't just find a pattern and copy it into your problem, as the way you can with off-the-shelf functions or libraries. The pattern is not a specific piece of code, but a general concept for solving a particular problem. You can follow the pattern details and implement a solution that suits the realities of your own problem.

## Benefits
Why should we spend time learning design patterns?

- Design patterns are a toolkit of `tried and tested solutions` to common problems in software design. Even if you never encounter those problems, knowing patterns is still useful because it teaches you how to solve all sorts of problems using principles of object-oriented design.
- Design patterns define a common language that you and your teammates can use to communicate more efficiently. You can say "just use a Singleton for that", and everyone will understand the idea behind your suggestion. No need to explain what a singleton is if you know the pattern and its name.

## Classification of Patterns
Design patterns differ by their complexity, level of detail and scale of applicability to the entire system being designed. 

The most basic and low-level patterns are often called `idioms`. They usually apply only to a single programming language.

The most universal and high-level patterns are architectural patterns. Developers can implement these patterns in virtually any language. Unlike other patterns, they can be used to design the architecture of an entire application.

In addition, all patterns can be categorized by their `intent`, we cover three main groups of patterns:

- `Creational patterns`, provide object creation mechanisms that increase flexibility and reuse of existing code.
- `Structural patterns`, explain how to assemble objects and classes into large structures, while keeping the structures flexible and efficient.
- `Behavioral patterns`, take care of effective communication and the assignment of responsibilities between objects.

## Creational Pattern
Creational patterns provide various object creation mechanisms, which increase flexibility and reuse of existing code.

### Factory Method
`Factory Method` is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclass to alter the type of objects that will be created.

#### Structure
![factory](images/design_pattern/factory.png?raw=true)

- The `Product` declares the interface, which is common to all objects that can be produced by the creator and its subclasses.
- `Concrete Products` are different implementations of the product interface.
- The `Creator` class declares the factory method that returns new product objects. It’s important that the return type of this method matches the product interface.
You can declare the factory method as abstract to force all subclasses to implement their own versions of the method. As an alternative, the base factory method can return some default product type.
Note, despite its name, product creation is not the primary responsibility of the creator. Usually, the creator class already has some core business logic related to products. The factory method helps to decouple this logic from the concrete product classes. Here is an analogy: a large software development company can have a training department for programmers. However, the primary function of the company as a whole is still writing code, not producing programmers.
- `Concrete Creators` override the base factory method so it returns a different type of product.
Note that the factory method doesn’t have to create new instances all the time. It can also return existing objects from a cache, an object pool, or another source.

#### Advantage

- You avoid tight coupling between the creator and the concrete products.
- `Single Responsiblity Principle`. You can move the product creation code into one place in the program, making the code easier to support.
- `Open/Close Principle`. You can introduce new types of products into the program without breaking existing client code.

#### Disadvantage

- The code may become more complicated since you need to introduce a lot of new subclasses to implement the pattern. The best case scenario is when you're introducing the pattern into an existing hierarchy of creator classes.

### Abstract Factory
Abstract Factory is a creational design pattern that lets you produce families of related objects without specifying their concrete classes.

#### Structure
![abstract_factory](images/design_pattern/abstract_factory.png?raw=true)

- `Abstract Products` declare interfaces for a set of distinct but related products which make up a product family.
- `Concrete Products` are various implementations of abstract products, grouped by variants. Each abstract product (chair/sofa) must be implemented in all given variants (Victorian/Modern).
- The `Abstract Factory` interface declares a set of methods for creating each of the abstract products.
- `Concrete Factories` implement creation methods of the abstract factory. Each concrete factory corresponds to a specific variant of products and creates only those product variants.
- Although concrete factories instantiate concrete products, signatures of their creation methods must return corresponding abstract products. This way the client code that uses a factory doesn’t get coupled to the specific variant of the product it gets from a factory. The Client can work with any concrete factory/product variant, as long as it communicates with their objects via abstract interfaces.

#### Advantage

- You can be sure that the products you're getting from a factory are compatible with each other.
- You avoid tight coupling between the creator and the concrete products.
- `Single Responsiblity Principle`. You can move the product creation code into one place in the program, making the code easier to support.
- `Open/Close Principle`. You can introduce new types of products into the program without breaking existing client code.

#### Disadvantage

- The code may become more complicated since you need to introduce a lot of new subclasses to implement the pattern. The best case scenario is when you're introducing the pattern into an existing hierarchy of creator classes.

### Builder
Builder is a creational design pattern that lets you construct complex objects step by step. The pattern allows you to produce different types and representations of an object using the same construction.

#### Structure
![builder](images/design_pattern/builder.png?raw=true)

- The `Builder` interface declares product construction steps that are common to all types of builders.
- `Concrete Builders` provide different implementations of the construction steps. Concrete builders may produce products that don’t follow the common interface.
- `Products` are resulting objects. Products constructed by different builders don’t have to belong to the same class hierarchy or interface.
- The `Director` class defines the order in which to call construction steps, so you can create and reuse specific configurations of products.
- The `Client` must associate one of the builder objects with the director. Usually, it’s done just once, via parameters of the director’s constructor. Then the director uses that builder object for all further construction. However, there’s an alternative approach for when the client passes the builder object to the production method of the director. In this case, you can use a different builder each time you produce something with the director.

#### Advantage

- You can construct objects step-by-step, defer construction steps or run steps recursively.
- You can reuse the same construction code when building various representations of products.
- `Single Responsibility Principle`. You can isolate complex construction code from the business logic of the product.

#### Disadvantage

- The overall complexity of the code increases since the pattern requires creating multiple new classes.

### Prototype
Prototype is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.

#### Structure
![prototype](images/design_pattern/prototype.png?raw=true)

- The `Prototype` interface declares the cloning methods. In most cases, it’s a single `clone` method.
- The `Concrete Prototype` class implements the cloning method. In addition to copying the original object’s data to the clone, this method may also handle some edge cases of the cloning process related to cloning linked objects, untangling recursive dependencies, etc.
- The `Client` can produce a copy of any object that follows the prototype interface.

#### Advantage

- You can clone objects without coupling to their concrete classes.
- You can get rid of repeated initialization code in favor of cloning pre-built prototypes.
- You can produce complex objects more conveniently.
- You get an alternative to inheritance when dealing with configuration presets for complex objects.

#### Disadvantage

- Cloning complex objects that have circular references might be very tricky.

### Singleton
Signleton is a creational design pattern that lets you ensure a class has only one instance, while providing a global access point to this instance.

#### Structure
![singleton](images/design_pattern/singleton.png?raw=true)

- The `Singleton` class declares the static method `getInstance` that returns the same instance of its own class.
The Singleton's constructor should be hidden from the client code. Calling the `getInstance` method should be the only way of getting the Singleton object.

#### Advantage

- You can be sure that a class has only a single instance.
- You gain a global access point to that instance.
- The singleton object is initialized only when it's requested for the first time.

#### Disadvantage

- Violates the `Single Responsibility Principle`. The pattern solves two problems at the time.
- The Singleton pattern can mask bad design, for instance, when components of the program know too much about each other.
- The pattern requires special treatment in a multithreaded environment so that multiple threads won't create a singleton object several times.
- It may be difficult to unit test the client code of the Singleton because many test frameworks rely on inheritance when producing mock objects. Since the constructor of the singleton class is private and override static methods is impossible in most languages, you will need to think of a creative way to mock the singleton. Or just don't write the tests. Or don't use the Singleton pattern.

## Structural Pattern
Structural patterns explain how to assemble objects and classes into larger structures while keeping these structures flexible and efficient.

### Adapter
Adapter allows objects with incompatible interfaces to collaborate.

#### Strucure
![adapter](images/design_pattern/adapter.png?raw=true)

- The `Client` is a class that contains the existing business logic of the program.
- The `Client Interface` describes a protocol that other classes must follow to be able to collaborate with the client code.
- The `Service` is some useful class (usually 3rd-party or legacy). The client can’t use this class directly because it has an incompatible interface.
- The `Adapter` is a class that's able to work with both the client and the service: it implements the client interface, while wrapping the service object. The adapter receives calls from the client via the adapter interface and translates them into calls to the wrapped service object in a format it can understand.
- The client code doesn’t get coupled to the concrete adapter class as long as it works with the adapter via the client interface. Thanks to this, you can introduce new types of adapters into the program without breaking the existing client code. This can be useful when the interface of the service class gets changed or replaced: you can just create a new adapter class without changing the client code.

#### Advantage

- `Single Responsibility Principle`.
- `Open/Close Principle`.

#### Disadvantage

- The overall complexity of the code increases becuase you need to introduce a set of new interfaces and classes. Sometimes it's simpler just to change the service class so that it matches the rest of your code.

### Bridge
Bridge lets you split a large class or a set of closely related classes into two separate hierarchies: abstraction and implementation, which can be developed independently of each other.

#### Structure
![bridge](images/design_pattern/bridge.png?raw=true)

- The `Abstraction` provides high-level control logic. It relies on the implementation object to do the actual low-level work.
- The `Implementation` declares the interface that’s common for all concrete implementations. An abstraction can only communicate with an implementation object via methods that are declared here.
The abstraction may list the same methods as the implementation, but usually the abstraction declares some complex behaviors that rely on a wide variety of primitive operations declared by the implementation.
- `Concrete Implementations` contain platform-specific code.
- `Refined Abstractions` provide variants of control logic. Like their parent, they work with different implementations via the general implementation interface.
- Usually, the `Client` is only interested in working with the abstraction. However, it’s the client’s job to link the abstraction object with one of the implementation objects.

#### Advantage

- You can create platform-independent classes and apps.
- The client code works with high-level abstractions. It isn't exposed to the platform details.
- `Open/Close Principle`.
- `Single Responsibility Principle`.

#### Disadvantage

- You might make the code more complicated by applying the pattern to a highly cohesive class.

### Composite
Composite lets you compose objects into tree structures and then work with these structures as if they were individual objects.

#### Structure
![composite](images/design_pattern/composite.png?raw=true)

- The `Component` interface describes operations that are common to both simple and complex elements of the tree.
- The `Leaf` is a basic element of a tree that doesn’t have sub-elements.
Usually, leaf components end up doing most of the real work, since they don’t have anyone to delegate the work to.
- The `Container` (aka composite) is an element that has sub-elements: leaves or other containers. A container doesn’t know the concrete classes of its children. It works with all sub-elements only via the component interface.
Upon receiving a request, a container delegates the work to its sub-elements, processes intermediate results and then returns the final result to the client.
- The `Client` works with all elements through the component interface. As a result, the client can work in the same way with both simple or complex elements of the tree.

#### Advantage

- You can work with complex tree structures more conveniently: use polymorphism and recursion to your advantage.
- `Open/Close Principle`.

#### Disadvantage

- It might be difficult to provide a common interface for classes whose functionality differs too much. In certain scenarios, you'd need to overgeneralize the component interface, making it harder to comprehend.

### Decrator(Wrapper)
Decorator lets you attach new behaviors to objects by placing these objects inside special wrapper objects that contain the behavior.

#### Structure
![decorator](images/design_pattern/decorator.png?raw=true)

- The `Component` declares the common interface for both wrappers and wrapped objects.
- `Concrete Component` is a class of objects being wrapped. It defines the basic behavior, which can be altered by decorators.
- The `Base Decorator` class has a field for referencing a wrapped object. The field’s type should be declared as the component interface so it can contain both concrete components and decorators. The base decorator delegates all operations to the wrapped object.
- `Concrete Decorators` define extra behaviors that can be added to components dynamically. Concrete decorators override methods of the base decorator and execute their behavior either before or after calling the parent method.
- The `Client` can wrap components in multiple layers of decorators, as long as it works with all objects via the component interface.

#### Advantage

- You can extand an object's behavior without making a new subclass.
- You can add or remove resonsibilities from an object at runtime.
- You can combine several behaviors by wrapping an object into multiple decorates.
- `Single Responsibility Principle`.

#### Disadvantage

- It's hard to remove a specific wrapper from the wrapper stack.
- It's hard to implement a decorator in such a way that its behavior doesn't depend on the order in the decorators stack.
- The initial configuration code of layers might look pretty ugly.

### Facade
Facade provides a simplified interface to a library, a framework, or any other complex set of classes.

#### Structure
![facade](images/design_pattern/facade.png?raw=true)

- The `Facade` provides convenient access to a particular part of the subsystem’s functionality. It knows where to direct the client’s request and how to operate all the moving parts. 
- An `Additional Facade` class can be created to prevent polluting a single facade with unrelated features that might make it yet another complex structure. Additional facades can be used by both clients and other facades.
- The `Complex Subsystem` consists of dozens of various objects. To make them all do something meaningful, you have to dive deep into the subsystem’s implementation details, such as initializing objects in the correct order and supplying them with data in the proper format.
Subsystem classes aren’t aware of the facade’s existence. They operate within the system and work with each other directly.
- The `Client` uses the facade instead of calling the subsystem objects directly.

#### Advantage
- You can isolate your code from the complexity of a subsystem.

#### Disadvantage
- A facade can become a god object coupled to all classes of an app.

### Flyweight
Flyweight lets you fit more objects into the available amount of RAM by sharing common parts of state between multiple objects instead of keeping all of the data in each objects.

#### Structure
![flyweight](images/design_pattern/flyweight.png?raw=true)

- The Flyweight pattern is merely an optimization. Before applying it, make sure your program does have the RAM consumption problem related to having a massive number of similar objects in memory at the same time. Make sure that this problem can’t be solved in any other meaningful way.
- The `Flyweight` class contains the portion of the original object’s state that can be shared between multiple objects. The same flyweight object can be used in many different contexts. The state stored inside a flyweight is called *intrinsic*. The state passed to the flyweight’s methods is called *extrinsic*.
- The `Context class` contains the extrinsic state, unique across all original objects. When a context is paired with one of the flyweight objects, it represents the full state of the original object.
- Usually, the behavior of the original object remains in the flyweight class. In this case, whoever calls a flyweight’s method must also pass appropriate bits of the extrinsic state into the method’s parameters. On the other hand, the behavior can be moved to the context class, which would use the linked flyweight merely as a data object.
- The `Client` calculates or stores the extrinsic state of flyweights. From the client’s perspective, a flyweight is a template object which can be configured at runtime by passing some contextual data into parameters of its methods.
- The `Flyweight Factory` manages a pool of existing flyweights. With the factory, clients don’t create flyweights directly. Instead, they call the factory, passing it bits of the intrinsic state of the desired flyweight. The factory looks over previously created flyweights and either returns an existing one that matches search criteria or creates a new one if nothing is found.

#### Advantage
- You can save lots of RAM, assuming your program has tons of similar objects.

#### Disadvantage
- You might be trading RAM over CPU cycles when some of the context data needs to be recalculated each time somebody calls a flyweight method.
- The code becomes more complicated. New team members will always be wondering why the state of an entity was separated in such a way.

### Proxy
Proxy lets you provide a substitute or placeholder for another object. A proxy controls access to the original object, allowing you to perform something either before or after the request gets through to the original object.

#### Structure
![proxy](images/design_pattern/proxy.png?raw=true)

- The `Service Interface` declares the interface of the Service. The proxy must follow this interface to be able to disguise itself as a service object.
- The `Service` is a class that provides some useful business logic.
- The `Proxy` class has a reference field that points to a service object. After the proxy finishes its processing (e.g., lazy initialization, logging, access control, caching, etc.), it passes the request to the service object.
Usually, proxies manage the full lifecycle of their service objects.
- The `Client` should work with both services and proxies via the same interface. This way you can pass a proxy into any code that expects a service object.

#### Advantage

- You can control the service object without clients knowing about it.
- You can manage the lifecycle of the service object when clients don't care about it.
- The proxy works even if the service object isn't ready or is not available.
- `Open/Close Principle`.

#### Disadvantage
- The code may become more complicated since you need to introduce a lot of new classes.
- The response from the service might get delayed.

## Behavioral Pattern

### Chain of Responsibility
Chain of Responsibility is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain.

#### Structure
![chain](images/design_pattern/chain.png?raw=true)

- The `Handler` declares the interface, common for all concrete handlers. It usually contains just a single method for handling requests, but sometimes it may also have another method for setting the next handler on the chain.
- The `Base Handler` is an optional class where you can put the boilerplate code that’s common to all handler classes.
Usually, this class defines a field for storing a reference to the next handler. The clients can build a chain by passing a handler to the constructor or setter of the previous handler. The class may also implement the default handling behavior: it can pass execution to the next handler after checking for its existence.
- `Concrete Handlers` contain the actual code for processing requests. Upon receiving a request, each handler must decide whether to process it and, additionally, whether to pass it along the chain.
Handlers are usually self-contained and immutable, accepting all necessary data just once via the constructor.
- The `Client` may compose chains just once or compose them dynamically, depending on the application’s logic. Note that a request can be sent to any handler in the chain—it doesn’t have to be the first one.

#### Advantage
- You can control the order of request handling.
- `Single Responsibility Principle`.
- `Open/Close Principle`.

#### Disadvantage
- Some requets may end up unhandled.

### Command
Command turns a request into a stand-alone object that contains all information about the request. This transformation lets you parameterize methods with different requests. This transformation lets you parameterize methods with different requests, delay or queue a request's execution, and support undoable operation.

#### Structure
![command](images/design_pattern/command.png?raw=true)

- The `Sender` class (aka invoker) is responsible for initiating requests. This class must have a field for storing a reference to a command object. The sender triggers that command instead of sending the request directly to the receiver. Note that the sender isn’t responsible for creating the command object. Usually, it gets a pre-created command from the client via the constructor.
- The `Command` interface usually declares just a single method for executing the command.
- `Concrete Commands` implement various kinds of requests. A concrete command isn’t supposed to perform the work on its own, but rather to pass the call to one of the business logic objects. However, for the sake of simplifying the code, these classes can be merged.
Parameters required to execute a method on a receiving object can be declared as fields in the concrete command. You can make command objects immutable by only allowing the initialization of these fields via the constructor.
- The `Receiver` class contains some business logic. Almost any object may act as a receiver. Most commands only handle the details of how a request is passed to the receiver, while the receiver itself does the actual work.
- The `Client` creates and configures concrete command objects. The client must pass all of the request parameters, including a receiver instance, into the command’s constructor. After that, the resulting command may be associated with one or multiple senders.

#### Advantage
- `Single Responsibility Principle`.
- `Open/Close Principle`.
- You can implement undo/redo.
- You can implement deferred execution of operation.
- You can assemble a set of simple commands into a complex one.

#### Disadvantage
- The code may become more complicated since you're introducing a whole new layer between senders and receivers.

### Iterator
Iterator lets you traverse elements of a collection without exposing its underlying representation(list, stack, tree, etc.).

#### Structure
![iterator](images/design_pattern/iterator.png?raw=true)

- The `Iterator` interface declares the operations required for traversing a collection: fetching the next element, retrieving the current position, restarting iteration, etc.
- `Concrete Iterators` implement specific algorithms for traversing a collection. The iterator object should track the traversal progress on its own. This allows several iterators to traverse the same collection independently of each other.
- The `Collection` interface declares one or multiple methods for getting iterators compatible with the collection. Note that the return type of the methods must be declared as the iterator interface so that the concrete collections can return various kinds of iterators.
- `Concrete Collections` return new instances of a particular concrete iterator class each time the client requests one. You might be wondering, where’s the rest of the collection’s code? Don’t worry, it should be in the same class. It’s just that these details aren’t crucial to the actual pattern, so we’re omitting them.
- The `Client` works with both collections and iterators via their interfaces. This way the client isn’t coupled to concrete classes, allowing you to use various collections and iterators with the same client code.
Typically, clients don’t create iterators on their own, but instead get them from collections. Yet, in certain cases, the client can create one directly; for example, when the client defines its own special iterator.

#### Advantage
- `Single Responsibility Principle`.
- `Open/Close Principle`.
- You can iterate over the same collection in parallel because each iterator object contains its own iteration state.
- For the same reason, you can delay an iteration and continue it when needed.

#### Disadvantage
- Applying the pattern can be overkill if your app only works with simple collections.
- Using an iterator may be less efficient than going through elements of some specialized collections directly.

### Mediator
Mediator lets you reduce chaotic dependencies between objects. The pattern restricts direct communications between the objects and forces them to collaborate only via a mediator object.

#### Structure
![mediator](images/design_pattern/mediator.png?raw=true)

- `Components` are various classes that contain some business logic. Each component has a reference to a mediator, declared with the type of the mediator interface. The component isn’t aware of the actual class of the mediator, so you can reuse the component in other programs by linking it to a different mediator.
- The `Mediator` interface declares methods of communication with components, which usually include just a single notification method. Components may pass any context as arguments of this method, including their own objects, but only in such a way that no coupling occurs between a receiving component and the sender’s class.
- `Concrete Mediators` encapsulate relations between various components. Concrete mediators often keep references to all components they manage and sometimes even manage their lifecycle.
- `Components` must not be aware of other components. If something important happens within or to a component, it must only notify the mediator. When the mediator receives the notification, it can easily identify the sender, which might be just enough to decide what component should be triggered in return.
From a component’s perspective, it all looks like a total black box. The sender doesn’t know who’ll end up handling its request, and the receiver doesn’t know who sent the request in the first place.

#### Advantage
- `Single Responsibility Principle`.
- `Open/Close Principle`.
- You can reduce coupling between various components of a program.
- You can reuse individual components more easily.

#### Disadvantage
- Over time a mediator can evolve into a `God Object`.

### Memento
Memento lets you save and restore the previous state of an object without revealing the detail of its implementation.

#### Structure
![memento](images/design_pattern/memento.png?raw=true)

- The `Originator` class can produce snapshots of its own state, as well as restore its state from snapshots when needed.
- The `Memento` is a value object that acts as a snapshot of the originator’s state. It’s a common practice to make the memento immutable and pass it the data only once, via the constructor.
- The `Caretaker` knows not only “when” and “why” to capture the originator’s state, but also when the state should be restored.
A caretaker can keep track of the originator’s history by storing a stack of mementos. When the originator has to travel back in history, the caretaker fetches the topmost memento from the stack and passes it to the originator’s restoration method.
- In this implementation, the memento class is nested inside the originator. This lets the originator access the fields and methods of the memento, even though they’re declared private. On the other hand, the caretaker has very limited access to the memento’s fields and methods, which lets it store mementos in a stack but not tamper with their state.

#### Advantage
- You can reduce snapshots of the object's state without violating its encapsulation.
- You can simplify the originator's code by letting the caretaker maintain the history of the originator's state.

#### Disadvantage
- The app might consume lots of RAM if clients create mementos too often.
- Catetaker's should track the originator's lifecycle to be able to destroy obsolete mementos.
- Most dynamic programming languages, such as PHP, Python, and JavaScript, can't guarantee that the state within the memento stays untouched.

### Observer
Observer lets you define a subscription mechanism to notify multiple objects about any events that happen to the object they're observing. 
#### Structure
![observer](images/design_pattern/observer.png?raw=true)

- The `Publisher` issues events of interest to other objects. These events occur when the publisher changes its state or executes some behaviors. Publishers contain a subscription infrastructure that lets new subscribers join and current subscribers leave the list.
- When a new event happens, the publisher goes over the subscription list and calls the notification method declared in the subscriber interface on each subscriber object.
- The `Subscriber` interface declares the notification interface. In most cases, it consists of a single `update` method. The method may have several parameters that let the publisher pass some event details along with the update.
- `Concrete Subscribers` perform some actions in response to notifications issued by the publisher. All of these classes must implement the same interface so the publisher isn’t coupled to concrete classes.
- Usually, subscribers need some contextual information to handle the update correctly. For this reason, publishers often pass some context data as arguments of the notification method. The publisher can pass itself as an argument, letting subscriber fetch any required data directly.

#### Advantage
- `Open/Close Principle`.
- You can establish relations between objects at run time.

#### Disadvantage
- Subscribers are notified in random order.

### State
State lets an object alter its behavior when its internal state changes. It appears as if the object changed its class.

#### Structure
![state](images/design_pattern/state.png?raw=true)

- `Context` stores a reference to one of the concrete state objects and delegates to it all state-specific work. The context communicates with the state object via the state interface. The context exposes a setter for passing it a new state object.
- The `State` interface declares the state-specific methods. These methods should make sense for all concrete states because you don’t want some of your states to have useless methods that will never be called.
- `Concrete States` provide their own implementations for the state-specific methods. To avoid duplication of similar code across multiple states, you may provide intermediate abstract classes that encapsulate some common behavior.
State objects may store a backreference to the context object. Through this reference, the state can fetch any required info from the context object, as well as initiate state transitions.
- Both context and concrete states can set the next state of the context and perform the actual state transition by replacing the state object linked to the context.

#### Advantage
- `Single Responsibility Principle`.
- `Open/Close Principle`.
- Simplify the code of the context by eliminating the code of the context by eliminating bulky state machine conditionals.

#### Disadvantage
- Applying the pattern can be overkill if a state mechine has only a few states or rarely changes.

### Strategy
Strategy lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable.

#### Structure
![strategy](images/design_pattern/strategy.png?raw=true)

- The `Context` maintains a reference to one of the concrete strategies and communicates with this object only via the strategy interface.
- The `Strategy` interface is common to all concrete strategies. It declares a method the context uses to execute a strategy.
- `Concrete Strategies` implement different variations of an algorithm the context uses.
- The context calls the execution method on the linked strategy object each time it needs to run the algorithm. The context doesn’t know what type of strategy it works with or how the algorithm is executed.
- The `Client` creates a specific strategy object and passes it to the context. The context exposes a setter which lets clients replace the strategy associated with the context at runtime.

#### Advantage
- You can swap algorithms used inside an object at runtime.
- You can isolate the implementation details of an algorithm from the code that uses it.
- You can replace inheritance with composition.
- `Open/Close Principle`.

#### Disadvantage
- If you only have a couple of algorithms and they rarely change, there's no real reason to overcomplicate the program with new classes and interfaces that come along with the pattern.
- Clients must be aware of the differences between strategies to be able to select a proper one.
- A lot of modern programming languages have functional type support that lets you implement different versions of an algorithm inside a set of anonymous functions. Then you could use these functions exactly as you'd have used the strategy objects, but without bloating your code with extra classes and interfaces.

### Template Method
Template Method defines the skeleton of an algorithm in the superclass but lets subclasses override specific steps of the algorithm without changing its structure.

#### Structure
![template](images/design_pattern/template.png?raw=true)

- The `Abstract Class` declares methods that act as steps of an algorithm, as well as the actual template method which calls these methods in a specific order. The steps may either be declared `abstract` or have some default implementation.
- `Concrete Classes` can override all of the steps, but not the template method itself.

#### Advantage
- You can let clients override only certain parts of a large algorithm, making them less affected by changes that happen to other parts of the algorithm.
- You can pull the duplicate code into a superclass.

#### Disadvantage
- Some clients may be limited by the provided skeleton of an algorithm.
- You might violate the `Liskov Substitution Principle` by supperessing a default step implementation via a super calss.
- Template methods tend to be harder to maintain the more steps they have.

### Visitor
Visitor lets you separate algorithms from the objects on which they operate.

#### Structure
![visitor](images/design_pattern/visitor.png?raw=true)

- The `Visitor` interface declares a set of visiting methods that can take concrete elements of an object structure as arguments. These methods may have the same names if the program is written in a language that supports overloading, but the type of their parameters must be different.
- Each `Concrete Visitor` implements several versions of the same behaviors, tailored for different concrete element classes.
- The `Element` interface declares a method for “accepting” visitors. This method should have one parameter declared with the type of the visitor interface.
- Each `Concrete Element` must implement the acceptance method. The purpose of this method is to redirect the call to the proper visitor’s method corresponding to the current element class. Be aware that even if a base element class implements this method, all subclasses must still override this method in their own classes and call the appropriate method on the visitor object.
- The `Client` usually represents a collection or some other complex object (for example, a Composite tree). Usually, clients aren’t aware of all the concrete element classes because they work with objects from that collection via some abstract interface.

#### Advantage
- `Single Responsibility Principle`.
- `Open/Close Principle`.
- A visitor object can accumulate some useful information while working with various objects. This might be hady when you want to traverse some complex object structure, such as an object tree, and apply the visitor to each object of this structure.

#### Disadvantage
- You need to update all visitors each time a class gets added or removed from the element hierarchy.
- Visitors might lack the necessary access to the private fields and methods of the elements that they're supposed to work with.

## Reference
- [Design Patterns](https://refactoring.guru/design-patterns)
