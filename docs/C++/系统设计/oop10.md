# The 10 Object-Oriented-Principles(OOP)

The Object-Oriented Design Principles are the core of OOP programming, and we programmer should pay more attention to them.
It's important to learn the basics of Object-Oriented Programming like `Abstraction`, `Encapsulation`, `Polymorphism` and `Inheritance`, and meanwhile, it's equally important to know object-oriented design principles.

## 1. Don't Repeat Yourself(DRY)
This means don't write duplicated code, you can use `abstraction` to abstract common things in one place.
If you have a block of code in more than two places, make it a separate method; if you use a hard-coded value more than one time, make them `public constant`. The benefit of this principle is in maintenance.
> It's important not to abuse it, duplication is not for code, but for functionality.

Just don't combine any code that looks alike but not related.

## 2. Encapsulate What Changes
There is only one thing which is constant in software field and that is "Change". So encapsulate the code you expect or suspect to be changed in the future.
> The benefit of this principle is that it's easy to test and maintain proper encapsulated code.

The `Factory Design Pattern` is one example of this principle which encapsulates object creation code and provides flexibility to introduce a new product later with no impact on existing code.

## 3. Open Close Principle(OCP)
Classes, methods or functions should be `open` for extension(new functionality) and `close` for modification. This prevents someone from changing already tried and tested code.

> The benefit of this principle is that tested code will not be touched so that they won't break.

If you just add a new function, your code should be tested and not affect the existing code, that's the goal of this principle.

## 4. Single Responsibilty Principle(SRP)
As for this pinciple, there should not be more than one reason for a class to change, or a class should always handle single functionality.

> The benefit is that it reduces coupling between the individual component of the software and code.

For example, if you put more than one functionality in one class, this will introduce coupling between two functionality. And enven if you change one functionality there is a chance you broke coupled functionality, which requires another round of testing to avoid any surprise on the production environment.

## 5. Dependency Injection or Inversion Principle

> The beauty of this design principle is that any class which is injected by framework is easy to test with the mock object ans easier to maintain because object creation code is centralized in the framework and client code is not littered with that.

## 6. Favor Composition over Inheritance

There are two general ways to reuse the code you have already written: `inheritance` and `composition`, both have their own advantage and disadvantage. But, in general, you should always favor composition over inheritance, if possible.

> Composition is the lot more flexible than inheritance.

Composition alllows changing the behavior of a class at run-time by setting property during run-time and by using `interface` to compose a class we use `polymorphism` which provides flexibility to replace with better implementation any time.

## 7. Liskov Substitution Principle(LSP)
Subtypes must be substitutable for supertype, so that methods or functions which use superclass can work with the object of subclass without any issue.

> If a class has more functionality than subcalss, subclass might not support some of the functionality, this does violate LSP.

In order to follow LSP, derived class or subclass must enhance functionality, but not reduce them.

## 8. Interface Segregation Principle(ISP)
A client should not implement an interface if it doesn't use that.
This happens mostly when one interface contains more than one functionality, and the client only need one functionality.

> The benefit is, single functionality means less method to implement

## 9. Programming for Interface not Implementation
A programmer should always program for the `interface` and not for `implementation`, this will lead to flexible code which can work with any new implementation of the interface.

## 10. Delegation Principle
Don't do all stuff by yourself, delegate it to the respective class.

> The key benefit of this principle is no duplication of code and pretty easy to modify behavior.
