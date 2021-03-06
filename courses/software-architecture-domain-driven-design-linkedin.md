# Software Architecture: Domain-Driven Design

by Allen Holub

----

> It's tough to be agile if you're working with a system that can't handle rapid change. Domain-driven design (DDD)—one of the most effective architectural approaches for both agile environments in general and microservices in particular—can help you build systems that can stand up to change. In this course, Allen Holub provides programmers, software architects, business analysts, and product managers/owners with an overview of this essential architectural process, demonstrating how to use DDD to develop a microservice or other domain-focused system. Alan goes over the basics of DDD (and how it fits with agile), microservices, and bounded contexts and entities. Plus, he compares reactive and declarative systems and details how to approach an event storming session.

Course in [Linkedin Learning](https://www.linkedin.com/learning/software-architecture-domain-driven-design)

🏷️ Tags: `architecture`, `software-design`, `ddd`, `agile`, `microservices`, `event-storming`

---

## Domain-Driven Design

* Domain-driven design is authored by Eric Evans

* Things like microservices and agile didn't really exist at the time. Microservices brought domain-driven design back to the fore because as it turns out, it's an almost ideal way to design a set of microservices

* The techniques that we're going to look at work just as well in the monolith as they do with the set of microservices

* This class are introductory. In order to suplement them and dive into the details, I strongly recommend Vaughn Vernon's book called Domain-Driven Design Distilled
  
  * It covers all of the critical stuff that Eric's book covers but it's a little bit more up-to-date and it's a little bit more concise
  
* Characteristics of Domain-driven design:
  1. It is a **collaborative** process.
    * Business people and developers must work together daily throughout the project
    * The underlying philosophy of DDD and the underlying philosophies of agile are almost identical
  2. It is built on the notion of **modeling**.
    * The structure of the code should model or map to the structure of the domain within which the problem is being solved
    * All collaborators (even business users) can make sense of the structure
    * Changes happen in the domain. They don't happen in the code, they happen in the domain. So when that happens, you need to be able to get the equivalent change in the code as easily and as quickly as possible
  3. It is **incremental**.
    * You don't come up with a big architecture up front before you do any coding. Instead, you come up with just enough architecture to solve the inmediate problem
    * The code evolves as you learn more about the problem, and more architecture is added
      * That means that you have to go and change the architecture in order to accommodate whatever new things you learn as the code evolves
    * That's the agile way of working. DDD was designed with that in mind
    * A domain-driven design architecture will allow your code to grow incrementally over time because the architecture itself grows incrementally over time (it should be designed to grow incrementally over time)
  
* The domain is going to influence the system

  * Changes that are made in the domain influence the underlying software
  * This is really not a linear process. As the domain influences the system but the system also influences the domain
  * DDD is designed for a world in which you are releasing incrementally. So there's actually a cycle here. When you create a little bit of code, you release the code to your users, your users start using that code, and that changes the way they work, which is going to cause other changes to happen within the system itself. It's a cyclic process
  ![Domain influences System, cyclic](.assets/software-architecture-domain-driven-design-linkedin.md/domain_influences_system_cyclic.png)

