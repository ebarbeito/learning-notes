# Domain Modeling Made Functional

Scott Wlaschin - Explore DDD 2019

> Statically typed functional programming languages encourage a very different way of thinking about types. The type system is your friend, not an annoyance, and can be used in many ways that might not be familiar to OO programmers
>
> In this talk, Scott looks at some of the ways you can use types as part of a domain-driven design process, with some simple real-world examples. No jargon, no maths, and no prior FP experience necessary

------

**Available resources**

-  [Talk in Youtube](https://www.youtube.com/watch?v=PLFl95c-IiU)
-  [Slides in Slideshare](https://www.slideshare.net/ScottWlaschin/domain-modeling-made-functional-kandddinsky-2019)

🏷️ Tags: `talk`, `2019`, `eddd`, `ddd`, `domain`, `object-design`,  `functional-programming`

------

## Notes

* There's an intersection between functional programming and domain-driven design, but not a lot of people practice it
* Is FP scary? It isn't, and adoptiong  it (at least some of its *goodies*) is very powerfull while doing DDD
  * FP is really good for Boring Line Of Business Applications (BLOBAs)

### Part I: The importance of DDD

![DDD in a nutshell — Shared mental model](/Users/enrique.barbeito/Projects/_sandbox/learning-notes/talks/.assets/2019-eddd19-domain-modeling-made-functional.md/importance_of_ddd_shared_mental_model.png)

* All involved people have a shared mental model

  * Shared by *everybody*, including the code

* If you can reduce the Garbage-in, the Garbage-out will also be reduced within the software development process

  * Garbage can be bad requirements, or misunderstood
  * This is what software design is, getting the requirements and the modeling right. Reducing garbage in and out

* What DDD source code look like in FP (using F#)

  ```F#
  module CardGame =
    type Suit = Club | Diamond | Spade | Heart
    type Rank = Two | Three | Four | Five | Six | Seven | Eight | Nine | Ten | Jack | Queen | King | Ace
    type Card = Suit * Rank
    type Hand = Card list
    type Deck = Card list
    type Player = { Name:string; Hand:Hand }
    type Game = { Deck:Deck; Players:Player list }
    type Deal = ShuffledDeck -> (ShuffledDeck * Card)
    type PickupCard = (Hand * Card) -> Hand
  ```

  * Shared language: CardGame, Suit, Rank, Club, Diamon, Ten, Jack, Queen, Name, Hand, Deal, ...
  * `|` represents a choice (`OR`). it means "pick one from the list". e.g. `Suit` can be only `Club`, or `Diamond`, or `Spade`, or `Heart`. Nothing more, nothing else
  * `*` represents a pair (`AND`). It means "choose one from each type" e.g. `Card` is a `Suit`and a `Rank`
  * F# has list types built in (`list`keyword)
  * `X -> Y` means a function; input of X, output of Y
    * Scott uses them to represent workflows, or stories, or scenarios, or actions

* Modeling an action with a function

  * FP approach to domain-modeling is to have the actions represented by verbs and as functions
  * `(input) original Deck -> Deal -> (output) remaining Deck * on-table Card`
  * Everything is immutable, so the original Deck's state never changes

* Using this kind of modeling (with rich types) makes the code to be understood for non-programmers

  * So domain experts, non-programmers, can provide useful feedback into the code

* What's a shuffled deck? Expert: "it's a list of cards"

  *  Having a Deck, and now adding a "ShuffledDeck" what I'm doing is adding a new concept to the domain
  * But what I do not do is to add a flag to the original Deck saying is it shuffled or not
  * Because they are a different concept. They can't be used interchangeably

* How do you make a shuffled deck?

  * This is an action, a workflow

    ```F#
    type ShuffledDeck = Card list
    type Shuffle = Deck -> ShuffledDeck
    ```

* The design is domain-driven, not database-driven

  * There's nothing about implementation here, nothing about databases, foreign keys, etc.
  * Design is what Eric Evans calls persistent ignorance
  * Also, is not "object oriented" either. There's no base classes, no managers, no factories, no inheritance, there's no all that stuff

* The design is based in the real word, using its vocabulary: Suit, Rank, Card, Hand, Deck, ...

  * In your code, you also want to have the same vocabulary
  * The domain code should be in sync with real world vocabuñary
  * If we learn new things about the domain, the code should reflect that

* This is the right way to design our domain. What we shouldn't do the other way around, where the domain contsains programmer jargon: something what domain experts do not understand

  * The "domain" code should not use programmer jargon
  * Don't place in the domain things like PlayerManager, DeckBase, AbstractCardProxyFactoryBean, ... This things are not domain concepts, something that domain experts need to know
  * Of course the implementation gets some of this, and you might to have this kind of things. But is in the side of implementation things, and not in the domain modeling side of things

* It's not just about the result of doing things, but actually the process of getting everyone in the same page

  * Like Event Storming and Context Mapping; the process of getting everyone in the same room and talking about it. Is a critical part
  * You don't come out with the document say this is the answer
  * Getting everyone to agree, having debates and having interactions and stuff is a really critical process
  * It's not about the result. The process of building the shared mental model is critical

* Key DDD principle: Communicate design in the code

  * Functional domain modeling can communicate all these decisions: Which values are optional? What are the constraints? Which fields are linked? Any domain logic?

### Part II: Understanding FP type systems

* algebraic types, like "composable types"

* FP principle: types are not classes. The're more like sets `A ∩ B = {x : x ∈ A and x ∈ B}`

* A function is a thing which transforms inputs to outputs, like an algebraic function

* A type in FP is just a name for a set of things

  * e.g. "all the numbers", we call it type `integer`
  * e.g. "all the possible strings", we call it type `string`
  * e.g. "all the possible fruits", we call it `Fruit`
  * Also a type can be a set of functions `Fruit -> Fruit functions` Function inputs and outputs can be functions

* FP principle: Composition everywhere

  * All the pieces are designed to be connected

* In FP, the behaviour is separate from data

  * If you come from OOP, this is like anemic domain models, so is a bad thing in an OO context, but in FP is actually a good thing

* New types are built from smaller ones (composition). Two ways of doing it:

  * Composing with "`AND`"
    * All its fields are required. None of them are optional
  * Composing with "`OR`"

* Examples composing with `AND` are pairs, tuples, records, structs

  ```F#
  type FruitSalad = {
    Apple: AppleVariety
    Banana: BananaVariety
    Cherry: CherryVariety
  }
  ```

* Examples composing with `OR`: `Snack = Apple or Banana or Cherry`

  * Not available in C#/Java or most non-FP languages

  ```F#
  type Snack =
    | Apple of AppleVariety
    | Banana of BananaVariety
    | Cherry of CherryVariety
  ```

  * Then, a snack will be just one thing but an apple, or a banana or a cherry. Nothing more, nothing else. Also, if is an apple then what kind of apple is it (variety), if is a banana then what kind of, etc.
  * This are called choice types, also called discriminated unions, or also some types, or co-product types. Scott likes to use "choice types" because is how you think about it in domain modeling

* 

