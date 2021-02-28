# 7 minutes, 26 seconds, and the Fundamental Theorem of Agile Software Development

J. B. Rainsberger - Øredev conference 2013

> Fred Brooks' essay "No Silver Bullet" taught us that no single technique can bring us an order-of-magnitude improvement within a single decade. In spite of this, from his ideas of essential and accidental complication, we can conclude something stunning about the nature of agile software development.

------

**Available resources**

-  [Talk in Youtube](https://youtu.be/WSes_PexXcA)

🏷️ Tags: `talk`, `2013`, `oredev13`, `complication`, `complexity`, `tdd`, `refactoring`, `software-economics`

------

## General notes

* Accidental complication, and essential complication
  * Not complexity, because it implies emergence
  * and i'm just talking about complication
* Essential complication because the problem is hard
  * So the system is complicated
* Accidental complication. We're not so good at our jobs
  * Because, we cut corners, we feel pressure, we don't have to worry about this time, we don't have to refactor so much
* `cost = f(g(e), h(a)) = g(e) + h(a)`
  * The cost of a feature is a function of the cost coming from the essential complication (because the problem is hard) and the cost of accidental complication (because we suck at our jobs)
  * We can add both together
* How do you estimate? If you estimate. Don't estimate
  * The problem is. When you estimate, you underestimate
  * Why? Because the accidental complication
* Most people, most of the time, the cost of a feature is dominated by the accidental complication
  * That means that the cost of the feature mostly has nothing to do with how hard it is and almost everything to do with how much your design sucks
* Test driven development to the rescue
  * First, it enables to think
  * Then, write the test
  * Then, stand back and ask how much does this test suck
  * With TDD you're reducing accidental complication
* More on TDD
  * Run the test, and watch it fail. You would be surprised how often it doesn't actually fail. Why?
  * Am I running the wrong test? Nope. Do I forgot to check something? Nope. I wrote too much code the last time and it already passes the test. Bingo
  * Accidental complication: I should stop doing that
* More on TDD
  * Write just enough code to mait pass. No more. Never
  * Just enough code! Not the line of code **you know** you have to write. Don't
  * This reduces accidental complication
* Only after writing the enough code to make it pass, the next thing we do is we clean the kitchen. We refactor a bit now
  * This reduces accidental complication
* Summary
  1. Think
  2. Write the test
  3. Watch it fail
  4. Write just enough code to make it pass
  5. Refactor, clean the kitchen. Reduce the accidental complication
  6. Ask yourself does the test suck
  7. Reduce accidental complication
* Every few minutes, every few minutes, like a circle/cycle. At the bottom of the circle avoid and squeeze out accidental complication that got it while you weren't looking
* Why all of this? Because the cost of a feature is the cost of the essential complication plus the cost of accidental complication
* If the cost of accidental complication dominates the cost of the feature, then you arrive to the fundamental theorem of agile software development
* Fundamental theorem of agile software development: if you want to estimate little things, you have to refactor, because this is how you reduce accidental complication. Only reducing accidental complication down as much as possible (to Zero) then your relative estimate would have any meaning

## References

* [No Silver Bullet – Essence and Accident in Software Engineering](http://www.cs.unc.edu/techreports/86-020.pdf)