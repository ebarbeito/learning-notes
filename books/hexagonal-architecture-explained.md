# Hexagonal Architecture Explained

by Alistair Cockburn and Juan Manuel Garrido de Paz

------

> How the Ports & Adapters architecture simplifies your life, and how to implement it

« _Looking at the screen of my laptop, I realized that it was full of code that didn’t let me understand what it did regarding business logic. From that moment I began to search until I discovered the architecture that decouples the business logic from the frameworks: Hexagonal Architecture, more correctly called Ports & Adapters. From that moment until now, I haven’t stopped reading and learning about this pattern._ »

Related resources

* [jmgarridopaz/discounter](jmgarridopaz/discounter) — An implementation of the application included in the "Sample Code" section of "Hexagonal Architecture" pattern
* [Hexagonal architecture](https://alistair.cockburn.us/hexagonal-architecture/)
* [Hexagonal Me](https://jmgarridopaz.github.io/), by Juan Manuel Garrido de Paz
* [Hexagonal Architecture (Ports & Adapters) The 2023 version](https://alistaircockburn.com/Hexagonal%20Budapest%2023-05-18.pdf)
* https://github.com/HexArchBook

------

## Introduction

* This is a preview edition. 1st edition may come next 2025
* Small sample code piece (copy, paste it, give it a try), brief timeline of the pattern, associated costs and benefits

```java
// (...)/ports/driving
interface ForCalculatingTaxes {
  double taxOn(double amount);
}

// (...)/ports/driven
interface ForGettingTaxRates {
  double taxRate(double amount);
}

// (...)
class TaxCalculator implements ForCalculatingTaxes {
  public TaxCalculator(
    private ForGettingTaxRates taxRateRepository
  ) {}
	public double taxOn(double amount) {
		return amount * taxRateRepository.taxRate(amount);
	}
}

// (...)
class FixedTaxRateRepository implements ForGettingTaxRates {
	public double taxRate(double amount) {
		return 0.15;
	}
}

// (...)
class Main {
	public static void main(String[] args) {
    ForGettingTaxRates taxRateRepository = new FixedTaxRateRepository;
    TaxCalculator myCalculator = new TaxCalculator(taxRateRepository);
    System.out.printin(myCalculator.taxOn(100));
}
```

* History and name of the pattern

  * 1988 Pain #1— 1994 Pain #2 — 2000 — 2005 "Ports & Adapters" — 2015 notions of Driving / Driven adapters — 2022 special case of "Component + Strategy" / *required interfaces* concept — 2023 "Configurable Receiver" pattern replacing the "Configurable Dependency" — 2024 This book

  * Why "Hexagonal Architecture"

    * It was a placeholder name before Alistair understood what the sides of the hexagon stood for
    * Not appropriate name, since number six has no meaning

  * History of the name — [Ward's wikiwiki site](http://c2.com/cgi/wiki?PortsAndAdaptersArchitecture)

    > *Somewhere in the mid-90s I started drawing a symmetric architecture in which (...) To break up perceptions about top and bottom and left and right, I drew it with a hexagonal shape (...) I could not identify think of what the "hexagon" meant, but knew it had to have facets, and no number smaller than 5 made visual sense (...)*

* Benefits of this pattern: **Testing** (run system-level tests without production connections; swap out the production connections), **Leakage protection** (detect whenever someone leaks UI and/or technology details into the business logic and **vice versa). Large system separations** (develop/test sections of code independently, connect them according to defined and tested interfaces). **Long-running systems** (replace connections over years). **Domain-Driven Design** (technology elements safety outside application boundary, then free to focus on the domain design)

* Complexity cost

  * Higher cost for type-declared languages than dynamic or type-inferred ones
  * Need of an instance variable / accessor for each driver actor
  * Adds a constructor parameter (or setter function) for each driven actor
  * In type-declared languages
    * You must declare "Required Interfaces"
    * Additional folder structure for the port declarations


### The Original Articles

* History of the pattern, evolution. Original 3 articles
* The Hexagonal (Ports & Adapters) Architecture — HaT Technical Report 2005.02. Date: 2005-09-04
* 

## Ports & Adapters defined

* Explains the pattern. Definition of terms. The four elements of the pattern (Apps, Ports, Actor, Adapters), the configurator. WHat is required and specified. Useful tricks

### Glossary

* **Application**, **App**, **Hexagon**, **Core**, **System**, **SuD**, **SuT**. All the business logic, without references to tech nor UI
* **Extended system**. The wider system, including adapters and directly connected technologies (databases, network, UI, external systems)
* **Actor**. _Anything_ (no matter what) with behavior / A thing has behaviour if it is able to execute an "if" statement
* **Primary or driving actor**. Actor that calls / initiates  request / sends message to the app / service. Actors can be primary in some scenarios, secondary in others
  * Analogy: *A Driving Actor presses the gas pedal, initiating action*

* **Secondary or driven actor**. A component or external system that the application interacts with to fulfill its tasks. It is “driven” by requests or commands from the application’s core
  * Analogy: *A Driven Actor is the engine or wheels, responding to the input to produce a result*

* **Interactor**. Piece of software that interacts direcly with the port
  * Analogy 1: Conductor of an Orchestra. The interactor ensures the musicians (driving and driven actors) perform their roles in harmony to produce the desired music (application behavior)
  * Analogy 2. Switchboard Operator. It connects callers (driving actors) to the right lines (driven actors) based on the context of the request

* **Interface**. Set of method definitions declsred by an actor. Specifies a contract
  * **Provided interface**. Defines the services offered by the app. Used by driving interactors, implemented by the app
    * Driving Actors (Adapters) interact with the provided interface of the application

  * **Required interface**. Defines the services needed by the app to perform its function. Implemented by the driven interactors
    * Driven Actors (Adapters) implement the required interface that the application core depends on

* **Port**. Provided or Required interface defined by the app. Created for some intention (eg. `ForPlacingOrders`) 
  * Boundary between the application core and the outside world
  * **Primary or Driving Port**.
  * **Secondary or Driven Port**.

* **Adapter**. Translates requests from an external actor linked to a specific technology, into technologically neutral requests at a port, and vice versa
  * **Primary or Driving adapter**.
  * **Secondary or Driven adapter**.


### Elements: App, Ports, Actors, Adapters

* This pattern is made by these four elements: **Application** (or system) aka the app; the **Ports**, the driving and driven external **Actors**; **Adapters** as needed at each port
* There is a fifth element (officially outside the pattern) that you need: the configurator. The code that connect these four parts
* The applicaion, app, or system
  * Whatever business logic you need. Written only in terms of the business
  * Technology-agnostic. No reference to any particular technology/platform connected to it
  * Ports & Adapters has you identify the places where your application meets the outside world
  * Creates a component, which declares two interfaces
    * Provided interface(s): set of services the app offers to the outside world
    * Required interface(s): for all external entities (promary or secondary) the app defines "I will only talk to you if you talk in my language", and explicitly defines that language
* The ports
  * Defines the boundaries of the application
  * Every interaction between the app and the outside world happens at a port interface
  * Each set of interactions **with a given purpose** (intention) is a port. Interactions grouped by their intentions
  * Convention name: "For doing something" (For + verb-ing + whatever else needed to make the purpose clear). Samples: "For managing he contents of the shopping cart", "For configuring the system", "For sending notifications"
  * Within type-declared languages (eg. Java) the port has to be declared explicitly
  * Within dynamic or type-inferred (eg. Ruby) ports are not explicitly visible
* The external actors
  * Actors that drive the app (Driving actors) — Primary
    * Any entity (human or electronic) that **kicks the app** into action
    * It makes a service request **of the app**
  * Actors that are driven by the app (Driven actors) — Secondary
    * Any entity (human or electronic) that the **app kicks** into action
    * Requesting a service from it
  * Match perfectly with the ports
    * Primary actors become driving actors, which interfact at the driving (primary) ports
    * Secondary actors become driven actors, which interact at the driven (secondary) ports
* The adapters
  * A human sitting at the computer can't directly call the app. Instead, they interact via some UI (CLI, GUI, voice, ...something)
  * These adapters convert human movements into the needed inerface, sllowing the app and the human to interact
  * Adapters are generally needed for ant sott of real.world technology
  * Adapters exist outside the app
* The configurator
  * Something has to connect all the pieces: tell the drivers how to reach the app. Tell the app which driven actors to use
  * It has to know all the players: the app, the driving actors (or their adapters), and the driven actors (or their adapters)
  * How you design the configurator is not specified by the pattern
  * In testing, the test cases act both as the configurator and the driving actor: creates and connects the players, and then drives the app

## Code Samples

* A series of samples in several langs. From dead simple to complete samples
* Explore samples, understand how the pattern functions in different envitonments
* 

## FAQ — What and How?

* 

## FAQ — Related Concepts

* 

## Summary

* Synopsis of the book. Patern definition in short form

