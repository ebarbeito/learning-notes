# Nothing is Something

Sandi Metz - RailsConf 2015

> Our code is full of hidden assumptions, things that seem like nothing, secrets that we did not name and thus cannot see. These secrets represent missing concepts and this talk shows you how to expose those concepts with code that is easy to understand, change and extend. Being explicit about hidden ideas makes your code simpler, your apps clearer and your life better. Even very small ideas matter. Everything, even nothing, is something

------

**Available resources**

-  [Talk in Somewhere](https://youtu.be/29MAL8pJImQ)
-  [Slides](https://speakerdeck.com/skmetz/nothing-is-something-railsconf)

🏷️ Tags: `talk`, `2015`, `oop`, `ruby`, `inheritance`, `composition`, `dependency-injection`, `patterns`

------

## General notes

![Main ideas](.assets/2015-railsconf15-nothing-is-something.md/main_ideas.png)

This talk goes in four bottom-up parts

### Smalltalk Infected

* I had a different ida about objects than many the people in our community
  * I decided it was because I'm *infected* by SmallTalk
* SmallTalk keywords: `true`, `false`, `nil`, `self`, `super`, `thisContext` (just six)
* I just want to send a message to an object
  * I don't want to have that syntax (`if (true|false) then ... else ...`)
  * I don't want to choose between two different kinds of behaviour
* If you came to Ruby and OO from procedural language (like most of us did) it probably seems normal, even reasonable, to write long conditions to start with `if`, `for`, `case`, whatever
* IF keyword
  * The presence of this keyword in our language, makes it easy to retain that procedural mindset
  * It keeps you away from learning OO and taking advantage of the power of objects and messages

### Condition Averse

* How would you think about objects if there were no `if` statement? What would it mean in your conception about writing OO code

* Sometimes **`nil` is nothing**
  * if `nil` is nothing and don't really care about you can `compact` the array (in the example) to just ignore/throw-away them

  * But, if you're sending it a message, then **`nil` is something**

    ```ruby
    if (some object whose type I know)
        I will supply the behaviour
    else
        I will send a message
    end
    ```

* If I know what type you are I will supply the behaviour, otherwise I will send a message. This is absolutely terrible

* The problem here is because conditions always get worse. Conditions breed, reproduce

  * So, if later those ifs need to change, you will be doing he so called shotgun surgery, because it will be in everywhere

### Message Centric

* I don't want to know these things. I just want to send a message to an object (`puts animal.name`)

  * The root of the problem here is that sometimes I get an `Animal` object back, but other times i get a `Nil` object back (which doesn't know about `name` message)

* Null Object Pattern

  * Bruce Anderson made up a beautiful term to call this pattern which is the Active Nothing

* Prefer knowing an object rather than duplicate behaviour

  * Colorary: Prefer knowing few objects. I don't want to know very many objects

    ```ruby
    class GuaranteedAnimal
      def self.find(id)
        Animal.find(id) || MissingAnimal.new
      end
    end
    ```

* The null object pattern is a small concrete instance of a much larger abstraction. It's just an example of a vert very large idea

### Abstraction Seeking

* Inheritance
  * Is for specialization
  * Is not for sharing behaviour, not for sharing code

* RandomHouse is-a House? EchoHouse is-a House?

  * By naming, you would say of course. We fooled ourselves because of the names
  * Names are incredibly important but they can be incredibly misleading
  * Instead of being trapped by our bad name, What changed? Reveal how things differ by making them more alike

* Order is-a House

  * No. There is no inheritance here. Order is a role

    ```ruby
    class RandomOrder
      def order(data)
        data.shuffle
      end
    end

    class DefaultOrder
      def order(data)
        data
      end
    end
    ```

  * Dependency-injection

    ```ruby
    class House
      attr_reader :data
      def initialize(orderer: DefaultOrder.new)
        @data = orderer.order(DATA)
      end
    end
    ```

* This is composition. Inject an object to play the role of the thing that varies

  ```ruby
  puts House.new(orderer: RandomOrder.new).line(12)
  ```

  1. fIsolate the thing that varies
  2. Name the Concept
  3. Define the Role
  4. Inject the player

* This is Composition + Dependency Injection. And this is what it means to do object-oriented design

  * Understanding these techniques lets you find the underlying abstraction
  * Getting that extraction right is going to dramatically improve your code

### Summary

* If you're talking to `nil`, it's something
* Use the null object pattern
* Beware of Inheritance, and be ready to switch to composition as it's not for sharing behaviour
* There's no such thing as **one** specialization
  * When you see that you have a variant, it means that you will have two, and so on
  * It means you have to isolate the think that varies, then name that concept, define the role and inject the players
* Believe in nothing
  * It's easy to see that real things can be modeled as objects
  * But, also more abstract things can be modeled usefully right; like business rules, ideas, concepts, processes, ...
* There are concepts that are abstractions that reveal their presence by the absence of code
  * I don't want to write a condition, I just want to send a message to an object

## References

* [SmallTalk](https://en.wikipedia.org/wiki/Smalltalk)
* [Shotgun surgery](https://en.wikipedia.org/wiki/Shotgun_surgery)
* Null Object Pattern
* [Practical Object Oriented Design in Ruby](https://www.poodr.com/)
* [99 Bottles of OOP](https://sandimetz.com/99bottles)