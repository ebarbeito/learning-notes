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

## References

* Checking vs Testing Michael Bolton [[1](https://www.satisfice.com/blog/archives/856)], [[2](https://www.infoq.com/news/2009/12/testing-or-checking/)], [[3](https://www.google.com/search?q=Checking+vs+Testing+Michael+Bolton)]

