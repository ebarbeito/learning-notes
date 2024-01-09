# DDD Surrounded by Legacy Software

by Eric Evans © 2013, Domain Language, Inc.

https://domainlanguage.com/ddd/surrounded-by-legacy-software/

> In this paper, I'll describe three strategies for getting started with DDD when you have a big commitment to legacy systems. These strategies emphasize different goals and require different levels of organizational commitment

## Strategy 1. Bubble context

* Applying effectively the tactical techniques of DDD requires a clean, bounded context. Legacy systems are often tangled, and even when they are orderly they are usually not suited to DDD
* (...) This leads some organizations to introduce DDD with legacy replacement projects and other ambitious initiatives. I advise against this also. Such efforts **are high-risk** even for a team that already has mastered whatever techniques are to be employed
* Legacy replacement is usually a bad strategy. Introducing a difficult new set of development principles and techniques is **best done incrementally**.
  * This allows members of the team to gain experience
  * Allows the organization to assess the approach. The "_bubble context_" is a good candidate in this situation
* The carefully designed code is gradually reabsorbed by the legacy

* Main Characteristics of the Bubble Context Strategy
  * Modest commitment to DDD
  * No synchronization risk (uses legacy database)
  * Works when there is a limited range of data needed from legacy

### How we form the bubble

* A '_bubble_' is a small bounded context established using an Anticorruption Layer (ACL) for the purpose of a particular development effort and not intended necessarily to be expanded or used for a long time
* **Anticorruption Layer and Translation**
  * This context boundary (_bubble_) isolates your new work from the larger system, allowing you to have a very different model in your context than exists just on the other side of the border
  * Each of the legacy contexts defines some concepts which are mapped to the abstractions of our bubble context
    * Legacy objects can be mapped to the new model
    * Translation itself can be tricky. There are alternative ways to populate the new objects from the legacy data
  * Ideally the translator should just translate
  * So working out the translation is an important part of the analysis when using an anticorruption layer
  * The implementation of that translator can take many forms. As much as practical, **it should be kept loosely coupled** to the rest of the ACL
* **ACL-backed Repository**
  * A nice technique that is sometimes used to set up a bubble context is the _ACL-backed Repository_

  * When we set up a bubble context, our data is coming from one or more legacy systems
  * We **don't create a new database** within the new context. We query the legacy database and rearrange the data into the new concepts in the ACL
  * The DDD Building Block used to represent access to pre-existing objects is the **Repository**: a thin layer over a data access layer, which is mapping some kind of data store into our objects
* At this point, it's clear that the ACL is a significant piece of software in its own right.
  * It should be explicitly addressed in budgeting and planning
  * It should be designed explicitly as other components are, with particular care to keep it decoupled from the actual business logic of the new system
  * A good ACL has a clean interface entirely in terms of the downstream model (in our case the bubble model) that provides access to the information and services of the upstream system

### What becomes of the bubble

* Development in the bubble could go on for several months. Occasionally, in cases where the interface with the other context is relatively narrow, for a few years
* More often, the bubble is temporary. Perhaps, as more is done in the new context, the discipline of the early work is not maintained
* Holes appear in the ACL. The bubble bursts. Sometimes, with the initial work completed, interest dissipates. (...) and the bubble's isolation is gone
* Created bubbles allow a team and an organization to gain experience with DDD, with a **minimal commitment and low risk**
* Also, they allow an intricate problem to be solved which would have been difficult to solve in the legacy context
* The bubble context is **a great way to get started with DDD while coexisting with legacy systems**


##  Strategy 2. Autonomous bubble

* ...

## Strategy 3. Exposing legacy assets as services

* ...

## Strategy 4. Expanding a bubble

* ...

