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
* 

### Chapter 2. Working with Feedback

* 

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

