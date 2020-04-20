# Outside-In Test-Driven Development

by Mark Seemann

------

> This course teaches how to build an application from the outside in - starting with tests targeting actual features or use cases of an application, but gradually working towards a more and more detailed specification of the components of an application. The focus is on the technical side of TDD, not the business side. Approximately half of the content is a series of C# demos, building a small RESTful service from scratch.

* [Course in Pluralsight](https://www.pluralsight.com/courses/outside-in-tdd)
* [My demo implementation](https://github.com/ebarbeito/running-journal-api)

------

## Module I. Walking skeleton

* In this module you'll learn how to build a walking skeleton by applying Outside-In TDD
* Concepts involved, including the concept of a walking skeleton → BDD, agile testing quadrants, the test pyramid,
* Also a comparison between Outside-In TDD against other types of software testing

### Purpose of Outside-In TDD

* It helps you to align what we're doing as software developers with what the product owner wants the software to actually do
* We do that by starting testing at the boundaries of the system (the UI, or at service layer like a REST or SOAP service), because that's most closely aligned with what the product owner typically wants
* The POs want a new feature, and they can express that in terms of using UI or in terms of service features that the service endpoint should have. And this helps us apply a principle called YAGNI
* By starting to build from the outside and going inwards, it helps to only write the needed code that meets the requirements that we've stated for ourselves by writing the test at the boundaries of the system
* And this helps us to bring better business value

### Outside-In TDD at a glance

* Just like with normal TDD, the first thing you have to do is write a failing test
* Again, just like with normal TDD, now that we have a failing test, we have to produce the real system, in order to make the test pass. This is what we call the system under test (SUT)
* Now, what normally happens with this first test, when doing Outside-In TDD, is that this prompts you to write quite a bit of infrastructure code (illustrated by making the SUT box slightly larger)
* But still, we're trying to write just enough code to make the test pass, and nothing more
* … and each time we do that, we slightly expand the SUT by writing new code to meet the requirements that each of those tests encapsulate

![Expanding SUT](.assets/outside-in-tdd-pluralsight.md/expanding_sut.png)

### Isn't this simply BDD?

* BDD is a testing discipline where you work tightly with the PO to specify a set of executable specifications that you can then use to validate whether the software that you're building is actually meeting those requirements or not
* And those executable specifications are usually executed against the boundaries of the system. So that sounds a lot like outside-in TDD. But are not the same, and can be used at the same time. Both  are overlapping and not mutually exclusive concepts

### The agile testing quadrants

* There are many ways to think about software testing
* This diagram, originally introduced by Brian Marick, talks about the many different ways that we can think about testing

### The test pyramid

* Concept developed by Mike Cohn. It describes how automated tests should be distributed among different types
* At the UI level, you should only have a few automated tests. They are very hard to develop and maintain, also typically run rather slowly
* At the service level, you can have more automated tests here. They tend to be more robust, but still quite slow
* Finally, you should have most of your automated tests at the unit testing level. They are a robust way of doing automated testing
* In relationship to outside-in TDD, is that you start at the top of the pyramid here. But then you work your way down. Which means that you write a few automated tests at the service boundaries of your system. But then, to flesh out the behavior of the system, you work your ways into the bottom of the pyramid here. And you should end up having most of your automated tests at the unit testing level
* You can have a couple of tests that test the boundaries of the system. But then you can have tons of more tests that tests the internals of the system at the unit testing level

### Outside-In or Bottom-Up

* There are two different approaches to TDD: Outside-in and bottom-up
  * Outside-in → “London School of TDD”, “Behaviour Verification”, “Mockist”
  * Bottom-up → “Classic TDD”, “State-Based Verification”, “Triangulation”
* London School of TDD vs Classic TDD, behavior-verification vs state-based verification, the mockist vs triangulation styles
* The important point here is that these aren't mutually exclusive approaches
* Even though we work from an overall approach of outside-in TDD, we can still do triangulation or state-based verification
* To learn more about the difference and similarities of outside-in vs bottom-up, read the article "[Classic TDD or London School](http://codemanship.co.uk/parlezuml/blog/?postid=987)" from Jason Gorman

### Walking Skeleton

* The first thing you should do with outside-in TDD is to build a walking skeleton
  * “*an implementation of the thinnest possible slice of real functionality that we can automatically build, deploy and test end-to-end*” (from the book “*Growing Object-Oriented Software, guided by tests*”)
* Walking Skeleton: the outcome of writing the first test and making it pass. The reason why we are talking about a skeleton is that at this point this system may not actually do anything particularly interesting. It just sits there and makes happy noises
* This, also, can be used to drive: Infrastructure (and technology decisions), Process
* Technical constraints
  * A typical process that we would like to arrive at is to automate as much as possible, not only the testing, but also the deployment
  * We want to be able to assume that there's no previous environment setup required, that you don't have to create special directories, neither set up special environment variables, registry keys, etc
  * This is very important in a team environment or when you have a CI system, because you have multiple environments and you don't want to have to maintain and synchronize the settings of all those environments
  * So the only things we can really assume is that we would have general tools, such as the IDE or a text editor and a terminal, for example. And that's the only thing we can really assume from the process
  * And everything else should be automatable

### Demo: introduction

* Now that you understand the principles and theory behind outside-in TDD and a walking skeleton, it's time to see how to do that in practice
* In this demo, I will show you how to write a walking skeleton, using Visual Studio and C#
* The scenario will be a REST API that captures information about a user's exercise runs.
  * The user can go for a run, and then they can upload a journal entry that captures how far they went when they did that and how fast they ran
  * And then later on they can go to the same REST service and review the journal entries that they already made
* The first thing to build a walking skeleton is simply to write a test and make sure that there's something responding on the other end
* So this is the first test that I'm going to write, and it's going to be very, very simple and a little bit boring, because it only tests that something sits on the other end of the service boundary -- in this case, a network address -- and actually responds to the requests that we're giving it

### Demo: Getting a response from a Walking Skeleton

* Building a walking skeleton means starting from scratch. The only thing that I have so far is just a solution called RunningJournalAPI and a single project for my service boundary test, those coarse-grained tests that I'm going to write at the boundary of the system
* The first thing to do is to pick a unit testing framework, or an automated testing framework (xunit)
* Now it's time to write the first automated tests. And I'll do that by adding a class called HomeJsonTests
  * And the reason why I call it that is because, if you think about the route resource of a RESTful API as the equivalent of the home page for a -- for a web-based application
  * And the reason why I called it json is because I'm going to use json as the serialization format going back and forth
* The first test I want to write is just a simple test that tests whether the response returns a correct status code
* So remember, at this point I'm just writing a test that pretty much verifies that there's a system at the other end and it's actually listening to the requests that we are sending
  * In order to invoke the external service boundary of the service, I need a client (he uses the new HTTP client class from .NET 4)
  * And we need some more infrastructure in place to actually make this work (an http server)
* So this prompts us to actually create an http server that's listening at localhost
* Here is where we might be tempted to set up Apache/nginx/whatever and create a service that's listening on localhost
* But that would go against a constraint that I should be able to take the source code for this whole system and pull it from the source control from some -- some other machine and just have the system running
* So I can't have that specific environment setup that requires me to have Apache/nginx/whatever configured in a certain way
* Luckily, the ASP .NET Web API, which is a new framework for building RESTful services, allows me to do in-process hosting of the service that I want to ride
  - In PHP the most similar option is to use the built-in web server functionality, and infrastructure code to handle HTTP routing (plain or framework code, but something in between)
  - The idea is to host the service from within the specific test case, and don't have to run anything outside the project

- From the client I can get a response, which is what I really care about

- So I'll just get the route resource here and block until the result comes back
- And the only thing I really care about at this point is just to see whether the response was some sort of success status code

- The test, as it currently looks, represents the essentials of what it is that I currently want to test
- And what I can do now is that I can create a new server. And I'll say that's going to be a new HTTP self-host server and just pull in that one
- We need to have something happening in between those two lines of code that configure the route. And I prefer that to be the system itself that has that responsibility (routing). So we could imagine that the system itself has this bootstrap class and it has a configure method. And we can pass in that config instance into the config -- into the configure method. And once that's done, we can start serving requests by just patching into specific controllers
- Now, this obviously doesn't exist yet, so we'll have to create the project for the system under test
- Now I'll go and add to the new project. And this can just be a normal library project, and we'll call that RunningJournalAPI
- In order to configure the system, we can add a route to this config instance here. And basically, what we say is that if the controller segment of the URI is empty, we will default to a controller called the JournalController. So let's add a class called the JournalController
- And in order to be a controller for the Web API, it needs to implement an interface called IHTPController. But we can also do that. The easiest way to do that is just by deriving from a class called APIController
- (...) implementation continues in .NET stuff (...)
- So, now I can actually run the test again. And you will see nothing really popped up. But I have one Passed and zero Failed or zero Skipped down in the corner. Just manually show the output here and we'll see that everything is actually passing at that moment

#### Demo recap

- So let's recap what happened. This is a walking skeleton that I'm building. So I just want the simplest possible test to get something up and running
- In this case I'm doing a get request against the root resource of my system. And the only thing that I'm really getting back is just 200 OK
- There's no content in the HTTP response at all, but that's okay for now
- However, if we think about the definition about a walking skeleton, the idea about building a possible slice of real functionality is probably a little bit of a stretch at the moment, because we just have something that responds
- So maybe we should think about doing just a little bit more before we can call it a day
- The next thing we can think about is to post a journal entry, because that would later on allow us to read back from that first resource and see whether we could read what we posted

### Demo: Posting an entry

- It's important to treat your test code as a first-class citizen, because this is something that you're checking into your source control system and you have to maintain it. So the same principles for maintainable code apply in test code.
- What I need to do now is just to make a post against a service, pretty similar to making a GET against the service. In the first instance I just want to test whether the response is still a success status code. We need to change the name of the method here. So we can just call it PostReturnsResponseWithCorrectStatusCode.
- First of all, we need something to post, and I want to post JSON as the running journal entry. And one way to model json is to just create an anonymous type. So you can see json here is just the type quote a, which hasn't been defined yet.
- The journal entry should include a time (DateTimeOffset.Now). We'll also need to log a distance (int, in meters) and a duration (TimeSpan)
- Now we can look at the response when we ask the client to do a post. So we'll do a POST as json async and post the json. And then we're just going to block and wait until the result comes back. Let's run the test and see what happens.

#### Demo recap

- At this point, you may be thinking that I'm cheating a little bit, because I haven't really added any functionality to the service at all. I'm just checking out the boundaries of the system and seeing whether they actually respond or not.

#### Demo: Posting and reading back an entry

- If we look at the journal controller, there's something missing here. We do a post, but we don't really save the value that's being posted at all
- We need to write one or more tests that will prompt us to add the desired functionality. Use a name of the test method to better capture the intent of this particular test case.

- I can post the json and just wait for the call to return
- And then I can get the response from a subsequent get request to that same resource
- That gives me the response back. And in order to make an assertion, I need to read the actual json content out of the response
- The thing that should contain the expected thing is actual entries

- We don't expect it to succeed at the moment.
- ...

#### Demo recap

- In this demo, I still tested the external boundaries of the system, first by making a post request, posting some json that represented a journal entry into the system, and just got a 200 OK response back
- Then subsequently, I made a get request against the same resource and got back a 200 OK response, with an Entries array, including the entry that I just posted.

#### Did I cheat?

- I cheated a little bit, because instead of using a real persistence engine, like a database or a file, I just used a read-only static list of journal entry models
- This is not the final state that I wish to arrive at. So in the next module, I'll show you how to spike a solution that uses real persistence technology, such as a database.

### Summary

- In this module you learned that Outside-In TDD is one among many testing techniques
- It favors testing of external system boundaries over testing the internal functionality of the system, at least at the beginning
- But still, keep Mike Cohn's test pyramid in mind. So while we start at the top of the pyramid by testing the external boundaries of the system, once we expand on the system, we'll write more and more unit tests to flesh out the behavior of the system
- The demo that I showed in this module demonstrated how to create a walking skeleton with the ASP .NET Web API

## Spiking

### Introduction

- This module talks about the concepts of spiking,FIRST (an acronym that describes some attractive how your unit test should have when you do TDD) and fixture, setup and teardown (which are part of a pattern called Four Phase Test)
- All of those things are important when we look at Outside-In TDD and this concept of spiking
- We also see how to apply these principles



### Application perspective

- With Outside-In TDD you don't need to have a detailed upfront plan for your applications architecture
- Initially, it often helps to just think about the applications architecture as a box, and then see what happens when you implement the application in response to the automated test that you write against the external boundaries of the system
- This is called Emergent Design, and helps you to adhere to this [YAGNI principle](https://www.google.com/url?q=https://martinfowler.com/bliki/Yagni.html&sa=D&ust=1587293165661000) (*you aren't going to need it*) → only write the code that you actually need to write in order to meet the requirements of the product owner
- The external boundaries describe features of the application (...) But the application of stakeholders also have other concerns for the application than just what can be observed over the external boundary of the system. One of these is the ability to store data in a persistent way
- This means that the application also has some internal boundaries. This is where it communicates with its persistent way
- So you can think about that as going all the way through the external boundary and into the internal boundary to see whether that data was actually stored → We call that spiking because it looks like we're driving a spike all the way through the application. We're not building the entire application, but we're going through the entire application for a very thin slice of functionality and making sure that we're going all the way through.

### FIRST

- Once we introduce persistence technologies to our automated tests, there are some things that we need to be aware of, that up until now hasn't really been an issue
- To understand what those are, we have to take a detour around this acronym called FIRST. Was first described by Tim Uttinger
- The FIRST acronym says that tests:

- Should be Fast. They should run as quickly as possible
- Should be Isolated so that each test can run independently on any other test
- Should be Repeatable, so you can repeat the test over and over again and get the same result every time
- Should be Self-validating, meaning that they will be able to tell by themselves whether they've passed or failed.
- Should be Timely, meaning, that they should be written at the correct time in the process, which means before you actually write the production code

- When we talk about persistence technologies isolated and repeatable and specifically the characteristic of being isolated is very important, because when we talk about the persistence technologies, what happens is that we leave behind stayed on the system, and that might actually influence the tests that run after the test that left behind stayed on the system. So we have to clean up after ourselves.

### Four-Phase Test

- When in each test do we do that? The Arrange-Act-Assert pattern for writing unit tests that says that each test should be divided into three phases

- First phase: Arrange. Where I set up all the things that I need to actually invoke the system
- Second phase: Act. Where I act on the system on the test that I have, in this case
- Third phase: Assert. Where I prove that the response that I got back was actually as it was supposed to be

- You might notice that there's an implicit fourth phase in a test, and happens when its scope ends
- It happens when the using scope ends and this is the sort of cleanup that happens
- How does this fourth phase fit into the arrange act assert pattern? And what do we call it? And it turns out that there's a different way to think about this arrange act assert pattern, is actually a superset of that pattern and that pattern is called Four Phase Test
- Instead of calling it arrange-act-assert, we call those phases “fixture setup, exercise SUT verification and fixture teardown”
- You see that we have an explicit fixture teardown where we clean up everything that we created so far
- This is the part of the test where we need to go and clean up any persistent data that each test case left behind

### Setting up and tearing down a database

- To introduce a persistence technology such as a database into your automated test, you need to make sure that any data left behind on that database is deleted again
- The safest way that you can guarantee that is by deleting the database after each test run
- Using the patent language are the Four Phase Test, here's how that might look like

- First, in the fixture setup phase, you create the database for every test case
- Then after you set the database you can exercise the system on the test, getting your result verification
- And then as the test completes, you can tear down the fixture again, which means simply deleting the database

- You should be aware that even though this runs pretty fast on modern computers, compared to a unit test where everything happens in memory, this is a rather slow process
- So we don't want to have too many of our automated tests working in this way. And if you recall, the test pyramid by Mike Cohn, this also fits pretty well with the concept of the pyramid, that we don't want to have too many tests at the top of the pyramid
- But the unit test that we can have, that's the base of the pyramid, shouldn't be using the database in this way. So we can minimize the number of automated tests that we have that actually touch the database, because setting up and tearing down this database for each and every test case will have a performance impact on how fast our test runs.

### Demo: database setup and teardown

- In this demo I will show you how to set up and tear down the database for each test case
- ...

