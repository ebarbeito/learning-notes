# Advanced Design Patterns: Design Principles

by Eric Freeman – Linkedin Learning

------

> You may be familiar with the fundamental concepts of object-oriented design—inheritance, encapsulation, polymorphism, and abstraction—but there is a set of higher-level design principles that can be used to take your design to the next level. Design principles guide your design decisions to produce software that is more reliable, flexible, and maintainable. Join instructor Eric Freeman as he goes beyond the standard concepts of object-oriented programming to introduce you to the most notable design principles, including encapsulate what varies, favor composition over inheritance, loose coupling, and the SOLID principles. Each lesson includes examples that show how these principles can be used to avoid costly design mistakes and create more maintainable, high-quality software

**Available resources**

-  [Course in Linkedin Learning](https://www.linkedin.com/learning/advanced-design-patterns-design-principles/)

🏷️ Tags: `course`, `2020`, `linkedin`, `oop`, `software-design`, `patterns`, `solid`, `principles`

------

## Design principles

* Design principles help us to improve our **object-oriented design**
* They're guidelines and/or advice, but **not absolute laws** that have to be followed
* Help us avoid **bad** object-oriented design. Symptoms of this?
  * **Regidity**. Design that is rigid
  * **Fragility**. Hard to change due to some dependencies
  * **Immobility**. It's hard to reuse, not possible to reuse it in places it wasn't designed for
  * **Inflexible design**, hard to maintain and not resilient to change
* Design principles go beyond those core object-oriented principles: Inheritance, Encapsulation, Polymorphism, Abstraction
* Blindly following these core concepts can often lead in the other direction to the bad quality
* Design principles, bring an **additional set of guidelines** on top of the core object-oriented concepts
* They give us **key insights** into how to and not to approach object-oriented design

![Object-oriented Design](.assets/advanced-design-patterns-design-principles-linkedin.md/object-oriented-design.png)

* For **design patterns**, we have standard catalogs that document them
* There are no standard catalogs for **design principles**
* That said, there are a set of fundamental principles:
  * **Encapsulate what varies**. It tells us that if we have part of our design that is changing, say with every new requirement, well then we should encapsulate that part away from the rest of the design
  * **Favor composition over inheritance**. It warrants against the overuse of inheritance and suggests composition as a powerful alternative for extending behavior in our designs
  * **Program to interfaces not implementations**. It encourages us to keep our designs high-level and referring where possible to abstractions or interfaces and not concrete implementations
  * **The loose coupling principle**. It tell us to strive for loosely coupled designs between objects that interact
  * Now there's another set of principles known as the **SOLID principles**
    * Which were introduced by the software engineer Robert Martin

## Encapsulate what varies

* Many consider it as the most important design principle. It forms the basis for every design pattern

  * Think of the encapsulate what varies principle as kind of the master principle

* Identify the aspects of your application that vary and **separate them** from what stays the same

* Is the same code changing with **every** new requirement? Then, you've got a behavior that really needs to be pulled out and separated from all the stuff that doesn't change

  * Then you can **alter or extend** the parts that vary, but do it without affecting the parts that don't vary

* There are many ways to pull something out of your design to encapsulate it, and that's often where patterns actually come in because patterns demonstrate different methods of separating aspects of your design

  ![Encapsulate what varies](.assets/advanced-design-patterns-design-principles-linkedin.md/encapsulate_what_varies.png)

* An example is the simple factory pattern, which is a very simple way of encapsulating what varies

* It's all about letting one part of the system vary independently of another part

  * To make use of encapsulate what varies, always look for code that's changing often
  * That's your signal that something needs to be pulled out and encapsulated on its own
  * Pay attention to how each pattern makes use of this principle, that let some part of the system vary independent of the other parts.

* The only constant thing about requirements is that they change

## Favor composition over inheritance

* Has-a is better than is-a
* With inheritance, duplication behaviours can be avoided
* Is a powerfull technique, but also one can easily be overused.
* It's also a technique that can lead to designs that are far too rigged and not extensible
* Many design patterns make use of composition to solve different typ
* Instead of inheriting behaviour, we can compose our objects with new behaviours
* Composition gives us more flexibility. Even allowing behavior changes dynamically, at runtime

## Loose coupling

* 