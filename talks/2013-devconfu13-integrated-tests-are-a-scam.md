# Integrated Tests Are A Scam

J. B. Rainsberger - DevConFu

------

**Available resources**

-  [Talk in Youtube](https://youtu.be/VDfX44fZoMc)
-  [Slides in Somewhere](https://somewhe.re/EcZ9dI)

🏷️ Tags: `talk`, `2013`, `devconfu`, `testing`, `integration`, `agile`, `xp`, ...

------

## General notes

* JBrains intro to ~~integration~~ integrated test: a self-replicasting virus that invades your projects. It threatens to destroy your codebase, to destroy your sanity and your life. Good evening!
* Integrated test is any test where when it fails I cannot point right here to find the problem
  * So, it's any test where the success or failure (pass or fail result) depends on many different bits of **interesting behavoiur** at once
  * We know that one of the properties of good tests is exactly the opposite
  * It's a test that maybe uses some cluster of things. So, if it fails we don't know where the issue exactly is. It's not so clear
* About the Testing Pyramid, the unit test part is good. But the rest it isn't. He considers all those parts as integrated tests
  * It doesn't matter if you're testing a cluster of objects, or if you're testing the entire application through the user interface. All are integrated tests
* About the unit tests, he prefers to get rid of that word and talk about Isolated tests
  * Isolated tests meaninng tests that test an isolated part of the system
  * He isn't interested in testing, but in checking
* Difference between testing and  checking

## Why are a scan?

* Why don't we just use isolated test for everything?
  * There's a problem. Isolated modules works but then, a module doesn't talk correctly with another module, etc.
  * It can be bugs that we only find when we try to run the system all together
* All the isolated tests pass (100%), but there's a bug, an we found it with an integrated test. So, that means we should probably write more integrated tests, to fill in the holes. To cover the areas where the isolated tests don't work well
* Isolated tests give feedback abouyt the quality of the design, so they help to find design problems
  * The real benefit of isolated tests (test one function at a time) is that they put tremendous pressure on our designs
  * To find dsign problems I must have isolated tests
* In his experience, he sees a strong correlation between a high number of these integrated ntests and design problems
  * The issue is that integrated tests test big parts of the system and they don't put pressure on the design, the more integrated tests we have the less design feedback we get, and with less feedback we get the easier it is to design not so carefully
  * So, more integrated tests encourage us to design more sloppily
* When we design more sloppily, it means that certain parts are really stoerngly depend on other parts, also that I can change something over here and something over there magically breaks
  * Also it means that will be easier to make mistakes (aka bugs)
* Write more integrated tests means less time for isolated tests
  * So, we have a sloppy design, easier to make more mistakes, and less time to isolated tests
  * In this scenario, we increase the likelihood that 100% of the tests pass but we still have mistakes
  * It's called the "positive feedback loop" because the effect becomes bigger, but it's a positive feedback loop of negative feelings
* And this is the scam
  * If someone sells you aspirin that actually gives you more of a headache, you would put this person in jail
  * The people who tell you to solve the problem of 100% test passing plus mistakes with more integrated tests is giving aspiring that causes you a bigger headache
  * The scam is spending more of our time writing integrated tests, then that make us design more sloppily, we make more mistakes, we write fewer tests than we could use to detect those mistakes and then the likelihood happens where all the tests run green but there are mistakes

## References

* Checking vs Testing Michael Bolton [[1](https://www.satisfice.com/blog/archives/856)], [[2](https://www.infoq.com/news/2009/12/testing-or-checking/)], [[3](https://www.google.com/search?q=Checking+vs+Testing+Michael+Bolton)]

