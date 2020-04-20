# Advanced Web Application Architecture

by Matthias Noback

------

> The missing manual for making your web applications future-proof
>
> Web applications deserve to outlive the currently fashionable framework. Your application's core use cases deserve to be decoupled from their surrounding infrastructure. And all of your domain-specific code needs to be testable; it has to be tested after all.
>
> This book helps you get your web applications back in shape. It contains many techniques for decoupling from infrastructure (like the framework, the database, or remote web services).

* [Leanpub](https://leanpub.com/web-application-architecture/)
* [Source code application sample](https://enjoy.gitstore.app/repositories/matthiasnoback/read-with-the-author)

Related resources

* Noback's blogposts
  * [Is all code in vendor infrastructure code?](https://matthiasnoback.nl/2020/02/is-all-code-in-vendor-infrastructure-code/)
  * Dividing responsibilities - [Part 1](https://matthiasnoback.nl/2019/07/dividing-responsibilities-part-1/) and [Part 2](https://matthiasnoback.nl/2019/07/dividing-responsibilities-part-2/)

------

## Preface

* The characteristics of some common types of objects: *controllers*, *entities*, *value objects*, *repositories*, *event subscribers*, etc. (...) how these different types of objects find their natural place in a set of architectural layers
* This book is a showcase of design patterns, about how these different objects work all together in a "well-architected" application
  * As a guide to decoupling your domain model and your application's use from the infrastructure (the framework, the database, and so on)
* Start using a standard set of layers (called **Domain**, **Application**, and **Infrastructure**) you can easily mark:
  * your decoupled use cases as **Ports**
  * and the supporting implementation code as **Adapters**
* Using layers, ports and adapters → A great way to standardizing your high-level architecture
  * If your app is supposed to live longer than two years → decoupling from infra is a safe bet
  * If not, that might be a good reason not to care about
* Software always becomes a mess, even if you follow all the known best practices for software design. But with this practices, it takes more time to become a mess

## Part I. Decoupling from infrastructure

### Preface and Introduction

* Why is separating infrastructure concerns from your core application logic so important?
  * It leads to a domain model that can be developed in a *domain-driven* way
  * It lead to application code that is very easy to test, and to develop in a *test-driven* way
* Make a clear distinction between the core code of your application, and the infrastructure code that supports it
* Both types of core are equally impiortant, but thet shoudn't live together (in the same classes, neither in the same layer)
* Distinction between infrastructure code and non-infrastructure code (core code) → **Not all code in vendor is infrastructure code**
  * Usually, most of the code in `vendor/` should be considered infrastructure code (your web framework which facilitates communication with browsers and external systems via HTTP, the ORM which communicates with the database, etc.)
  * Nevertheless, code placed in a particular directory doesn't determine whether or not something is infrastructure code
  * What matters is what the code does, and what it needs to do that (see the rules)
  * Some `vendor/`code could be considered core code, event though it's not written by you or for your application specifically
* We'll define core code by introducing **two rules for it**. Any code that doesn't follow both rules **at the same time**, should be considered infrastructure code
  * **Rule 1. No dependencies on external systems**. Core code doesn't directly depend on external systems, nor does it depend on code written for interacting with a specific type of external system
  * **Rule 2. No special context needed**. Core code doesn't need a specific environment to run in, nor does it have dependencies that are designed to run in a specific context only
* Notes about the Rule 1
  * An external system is something that lives outside your application (a database, a remote web service, the system's clock, the file system, etc.)
  * Core code should be able to run without these external dependencies
  * When code follows the first rule, it means you can run it in complete isolation
  * Automated test for core code will be very easy to write (no database setup/teardown, no internet connection, no hard disk I/O, etc.)
* Notes about the Rule 2
  * Only if code doesn't require a special context, **and also** hasn't been designed to run in a special context, can it be considered core code
  * Not having to create a soecial context for code to run it makes easy to write automated tests
* All of the domain code and the application's use cases should be core code, and not rely on or be coupled to surrounding infrastructure

### The domain model

* 

### Read models and view models

* 

### Use cases

* 

### Service locators

* 

### External services

* 

### Time and randomness

* 

### Validation

* 

### Conclusion

* 

## Part II. Organizing principles

### Introduction

* 

### Key design patterns

* 

### Architectural layers

* 

### Ports and adapters

* 

### Process modelling

* 

## Part III. Conclusion

### A testing strategy for decoupled applications

* 

### Context is everything

* 
