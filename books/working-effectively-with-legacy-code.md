# Working Effectively With Legacy Code

by Michael C. Feathers

------

> Get more out of your legacy systems, more performance, functionality, reliability, and manageability. Is your code easy to change? Can you get nearly instantaneous feedback when you do change it? Do you understand it? If the answer to any of these questions is no, you have legacy code, and it is draining time and money away from your development efforts

* [Goodreads](https://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code)
* [WorkingEffectivelyWithLegacyCode](https://wiki.c2.com/?WorkingEffectivelyWithLegacyCode)

------

## Foreword and preface

* The nice little system we built turns into a horrible mess of functions and variables next year
  * Why can't they stay clean? → Requirements change
  * Designs that cannot tolerate changing requirements are poor designs to begin with
  * It is the goal of every competent software developer to create designs that tolerate change
* (...) ways to prevent code from becoming legacy → Prevention is imperfect
* This book is about reversing the rot (...) gradually, piece by piece, step by step
* Legacy code is often used as slang term for difficult-to-change code that we don't understand

* Legacy code is simply code without tests
* Code without tests is bad code. It doesn't matter how well written it is. Without them, we really don't know if our code is getting better or worse

* What about clean code? (..) isn't that enough → It's not enough
  * Teams take serious chances when they try to make large changes without tests
* This book describes techniques that you can use to understand code, get it under test, refactor it, and add features
* This book is not about pretty code, it is even less about pretty design (...) Good design in legacy code, is something that we arrive at in discrete steps
* Code bases can become healthier and easier to work in
* I often feel that Extreme Programming is less a way to develop software than it is a way to make a well-jelled work team that just happens to deliver great software every two weeks

## Part I. The Mechanics of Change

### Chapter 1. Changing Software

* Four reasons to change software
  * Adding a feature
  * Fixing a bug
  * Improving the design
  * Optimizing resource usage

#### Adding features and Fixing bugs

* Seems like the most straighforward type of change to make
* A code change can be seen as a bug fixing or as a new feature
  * From the point of view of the customer, he/she can definitely asking us to fix a problem
  * From a developer's point of view, the change could be seen as a completely new feature
* Bug fixing and feature addition, both mask something that is much more important to us technically: **behavional change**
  * Behaviour is the most important thing about software
  * It is what users depend on
* Adding a method doesn't change behaviour unless it is called somehow

#### Improving Design

* It's a different kind of software change (...) we want to keep its behaviour intact
* The act of improving design without changing its behaviour is called refactoring
  * (_Sidenote_) It's not a refactor if design does not improve, or if behaviour is altered
  * (_Sidenote_) You cannot ensure the intact behaviour if code is not first test-harnessed
* Write tests to make sure that existing behaviour doesn't change. And take small steps to verify that all along the process
* Refactoring differs from general cleanup (...) We are making a series of small **structural** modifications, supported by tests to **make the code easier to change**
  * (_Sidenote_) Structural modification → Make code easier to change → Design improvement achieved
  * (_Sidenote_) Nothing about "beautiful code", or "performant", or "less lines of code", or "less/better comments", etc.

#### Optimization

* Is like refactoring, but when we do it, we have a different goal.
  * In refactoring, we focus in program structure; we want to make it easier to maintain
  * In optimization, we focus in some resource used by the program, usually time or memory
* The thing  that is common between refactoring and optimization is that we hold behaviour invariant while we let something else change

#### Putting it all together

* The big deal is that we often don't know how much of behaviour is at risk when we make our changes. To mitigate risk, we have to ask three questions:
  1. What **changes** do **we have to make**?
  2. How will we know that **we've done them correctly**?
  3. How will we know that **we haven't broken anything**?
* How much change can you afford if changes are risky?
  * Most teams/people try to manage risk in a very conservative way (...) "_it involves less editing and it's safer_", they think.
* Avoiiding change has bad consequences.  When you do it, it becomes routine
  * The last consequence of avoiding change is fear
  * Often they aren't aware of how much fear they have until they learn better techniques and the fear starts to fade away

### Chapter 2. Working with Feedback

* Changes in systems can be made by two primary ways: Edit and Pray, and Cover and Modify
* **Edit and Pray** → When you make changes, you are hoping and praying that you'll get them right
  * You plan changes to make, make sure you understand the code to modify, you make the cnahges and run the system to make sure (Sidenote: "_make sure_" subjectively) you didn't break anything
  * This way seems like "working with care", a professional thing to do. But "safety" isn't solely a function of care
  * Working with care diesn't do much for you if you don't use the right tools and techniques
* **Cover and Modify** → Is different; is working with a safety net when we change software
  * Covering software means covering it with tests
  * We still apply the same care, but with the feedback we get, we are able to make changes more carefully
* We can do "testing to detect change", this is calles regression testing
* When we have tests that detect change, it is like having a vise around our code (Software Vise). So, we are changing only one piece of behaviour at a time; changing only what you intend to → we're in control of our work
* The feedback we can get from regression testing is very useful
* The tests won't catch everything (...) After every little change that we make, we run that little suite of unit tests (...) We add some tests to verify the new behaviour
* **Unit testing is one of the most important components in legacy code work**. They can give you feedback as you develop and allow you to refactor with much more safety

#### What is Unit Testing?

* They are tests in isolation of individual components of software
* What are components? → The most atomic behavioral units of a system
  * In procedural code, the units are often functions
  * In object-oriented, the units are classes
* Test Harness → Term for the testing code that we write to exercise some piece of software and the code that is needed to run it
* Unit tests have qualities to be good: they run fast, and help us localize problems
  * A unit test that takes 1/10th of a second to run is a slow one
  * They run fasts. If the  don't, then they aren't unit tests
* A test is not a unit test if:
  * It talks to a database
  * It communicates across a network
  * It touches the file system
  * You have to do special things to your environment (editing conf files, e.g)
* Last is not bad, but not unit ones. It's important to be able to separate them from true unit tests, to keep a set of ones that can be run fast whenever you make changes

##### Test Coverings

* How can we **start** making changes in a legacy system?
  * The **safest way** (_Sidenote_: the best; given a choice) is to **have tests around** the changes
* We want to make changes, so we identify a "**change point**". Can we do a test harness around?
  * Sometimes this might be tricky, because **dependencies**
  * Dependencies bring us different problems in order to allow to tests first the code you want to change
* Dependency is one of the **most critical** problems in software development
  * **Much legacy code involves breaking dependencie**s, so that change can be easier
  * (_Sidenote_: So, working with legacy code involves knowing a lot about techniques to do this breaks in a safer way)
* **The Legacy Code Dilema**. When we change code, we should have tests in place. To put tests in place, we often have to change code
  * This requires dependency breaks. Some basic refactoring techniques (from the Catalog) to this are: "*Primitivize Parameter*", and "*Extract Interface*"
  * The **trick** is to do these **initial refactorings very conservatively**
* When you break dependencies in legacy code, you often have to suspend your sense of aesthetics a bit. Some dependencies break cleanly, but others not.
  * They are like incision points in surgery. There might be a scar left in code after your work
  * If you can cover code around the point where you broke dependencies later, you can heal the scar too

#### The Legacy Code Change: Algorithm

* Here is an algorithm when you have to make a change in a legacy code base:
  1. Identify change points
  2. Find test points
  3. Break dependencies
  4. Make changes and refactor
* Day-to-day goal in legacy code: to make changes. But **functional changes that deliver value** while bringing more of the system under test
* **Identify change points**
  * Places you need to make changes, it depends on your architecture
  * If you're not sure, go to Chapter 16 "*I don't understand the code well enough to change it*", and Chapter 17 "*My application has no structure*"
* **Find test points**
  * Sometimes is easy, but often it isn't
  * Chapters 11 and 12 offer techniques that you can use to determine where you need to write your tests for particular changes
* **Break dependencies**
  * Often, the most obvious impediment to testing. Two ways this impediment manifests are:
    * Difficulty instantiating objects in test harnesses
    * Difficulty running methods in test harnesses
  * Go to Chapter 23 to see some practices to make first safer incisions in a system, as you start to bring it under tests
  * Dependency problems also show up when we have an idea for a test but we can't write it easily. Go to Chapter 22 for this
  * If you find that you can break dependencies but it takes too long, then go to Chapter 17
* **Write tests**
  * Tests that you write for legacy code are different from those that you write for new code
  * Chapter 13 gives information about the role of tests while working with legacy code
* **Make changes and refactor**
  * I advocate **using TDD to add features in legacy code**
  * Chapter 8 "*How do I add a feature?*" gives description about TDD and some other feature addition technique
  * Chapters 20, 22 and 21 cover many of the techniques you can use to start to move your legacy code toward better structure
    * The things described in these chapters are "**baby steps**"
    * They don't show you how to make your design ideal, clean, or pattern-enriched (not the goal of this book)

### Chapter 3. Sensing and Separation

* 

### Chapter 4. The Seam Model

* 

### Chapter 5. Tools

* 

## Part II. Changing Software

### Chapter 6. I don't have much time and I have to change it

* 

### Chapter 7. It takes forever to make a change

* 

### Chapter 8. How do I add a feature?

* 

### Chapter 9. I can't not get this class a Test Harness

* 

### Chapter 10. I can't run this method in a Test Harness

* 

### Chapter 11. I need to make a change. What methods should I test?

* 

### Chapter 12. I need to make many changes in one area

* 

### Chapter 13. I need to make a change, but I don't know what tests to write

* 

### Chapter 14. Dependencies on libraries are killing me

* 

### Chapter 15. My application is all API calls

* 

### Chapter 16. I don't understand the code well enough to change it

* 

### Chapter 17. My application has no structure

* 

### Chapter 18. My test code is in the way

* 

### Chapter 19. My project is not object oriented. How do I make safe changes?

* 

### Chapter 20. This class is too big and I don't want it to get any bigger

* 

### Chapter 21. I'm changing the same code all over the place

* 

### Chapter 22. I need to chan ge a monster method and I can't write tests for it

* 

### Chapter 23. How do I know that I'm not breaking anything?

* 

### Chapter 24. We feel overwhelmed. It isn't going to get any better

* 

## Part III. Dependency-Breaking Techniques

### Chapter 25. Dependency-Breaking Techniques

* 

## Appendix: Refactoring

### Extract Method

* 

## Glossary

* 

### 

