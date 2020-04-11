# Late to the Party, but Nailing It! A Journey into Pair Programming


Technique popularised as an Agile software development

* **Pairing is about**
   * Wanting d+ifferent approaches to one same task at hand
   * Learning from each other
   * Avoiding knowledge silos in how things are made (and why) under the hood
   * Agreeing with "two heads are better than one"
      * Where "better" goes in both directions: engineering and economic
* Pairing is an **equal partnership**. You both learn from each other
   * It is not an apprenticeship where one person is learning from their mentor
* Two programmers **on the same computer**. Roles:
   * Driver: the person typing. Stays focused on the current task
   * Navigator: the person telling the driver how to complete the task. Thinks ahead, ponders edge cases, spots bugs, suggests refactorings, asks good questions, stays zoomed out

## How to proceed, in a nutshell

1. Choose your partner, pick a task and block off time together
2. Before coding
   1. Outline how you are going to code the task, which coding style and standards, what tools to use, etc.
   2. Use a whiteboard (or paper) to make sure you both understand the task in the same way
   3. This is highly recommended. Talking it out before coding it out
3. Try ten-minute intervals of swapping driver and navigator
4. Work in this way until finish the task, or until the time blocked is over
5. Commit and push the last work pending to do it
6. Small retrospective after the pairing. Talk about: what worked, what didn't, and what could you do to improve it

**Driver role**

* Unless you’re sure your pair is keeping up, don’t manipulate code quite as fast as you’re able
* If you get the sense that your pair’s attention is drifting, stop and sync up
* Ask good questions: "*Which part of this is hardest to follow?*” vs bad questions: “*You understand this, right?*”
* If your navigator is making a suggestion, consider taking your hands off the keyboard

**Navigator role**

* Give your driver a chance to notice their own syntax errors and typos
* If you have a suggestion for the driver, communicate it at the highest level of abstraction they’ll understand
* If you find yourself dictating code (or worse, individual keystrokes), stop and see if you can communicate your idea at a higher level

## In-depth details, step by step

### Finding a Pair

* Is not easy to find people to work in this way. Is recommended working only with someone open to the idea of pair programming
* People that don't think "this is stupid" is the recommended one
* Working with someone who works differently than you and has a different skillset
* People that follow the same coding standards is very important. If not, you need make sure you have an agreement in the same standards and style before start programming
Making a Plan
* If you are in your first pair-programming session/s, find and start on the smaller tasks, those that may only take thirty minutes to complete. As simpler and smaller ones, as easier to start and get good feeling with pair programming
* Make sure you have a plan of action, before start coding. Have a basic idea of how you'll work together
* Brainstorm what your challenges might be in this pairing session, and work to fix them before start
   * Example: your pair uses a Spanish keyboard layout, and you a different one (e.g: German layout). Before you start, you need to setup the system to switch fast and easly between both layouts
* Pick a task. Outline your objectives for both the task at hand and pair programming, in general
* Make a mini-retrospective together, immediately after your session. Be open, and honest about what worked and what can be done differently the next time
   * Example: In our retrospective, we were honest with one another. I told my pair he/she is squirrely, and that can be hard to work with. He/she told me that shouting orders at him/her is not the best way to be a good driver.
      * We learned, adjusted, and grew as a team

### Retrospective

* Spend a few minutes after your session reflecting on the experience
* First, discuss what went well.
* Then, consider what would make the next session better
* Possible areas for improvement
   * Focus: did distractions sneak in?
   * Communication: were there long stretches of no talking?
   * Pacing: did the session feel like a grind at any point?
   * Division of responsibility: did you split the work up well?
   * Code quality: was your end-product high-quality?

### Further recommendations while pairing

Try pair programming several times. What’s the worst that could happen?

**Before start**

* Don’t check your phones (silence it as well), emails, slacks, or get distracted
* Disable notifications on the machine you’re using to pair
* Close any program and window you don't need
* Close the email and instant messaging
* Put your phones to the side, so you aren't temped to pick them up

**While programming together**

* Set a timer to know when to switch drivers (ten-minute interval recommended)
   * Be flexible sometimes. Ignore it for a while if the driver is on a roll
* Stay focused. Pay attention, especially when you aren't the driver
* Pairing should involve constant two-way communication
* When navigating: ask questions rather than making demands
* When driving: dictate what you’re doing and why
* Swap roles frequently

**After you finish**

* Team building. High five after the session finish (or from time to time)
* Have a retrospective
* Schedule another pairing session

### Advantages and challenges on pair programming

**Advantages**

* Usually, your pair and you can come up with two different solutions tot he same problem. This difference makes pair programming an adventure and a fantastic learning opportunity
* You both combine your knowledge (don't caring about who knows more than the other, is absolutely irrelevant), compare solutions and choose the one that works best
* Taking time to discuss your approaches to solve the task, allows you to identify lot of issues in the possible solution

**Challenges**

* Working in team requires a substantial amount of trust and understanding. When pairing, the level of trust needed to work together is significant
   * This is the most significant obstacle to overcome: trusting each other enough to work openly
   * Building the trust and relationship to be able to do takes time (sometimes, lot of it)
* You have to know (or you have to build, if is not done yet) how to communicate with one another, and felt safe to both express your feelings
   * This can be worked on, making a retrospective once you finish the session
   * You need to be honest with each other on how it feels, what it worked and what didn't, in order to find solutions and establish actions to do in the next pairing session
* The pairing relationship requires some give and take; both members of the team must be willing to compromise and adapt.
   * Example of "give and take": Every time John and Mary pair together, John moves the cursor constantly; he does when he's thinking. But for Mary is distracting, so it's a challenge. In the retro this issue is exposed, Mary explain herself and ask him to stop moving the cursor so many times, so she can focus better. John stops doing it the next time.
* The basic setup of both computers, and each work environment is just for one person. This faces some challenges:
   * When code is pushed, only one name is attached to it. This lack of attribution becomes an issue if something comes back from code review that two people have to adjust
   * When one of the pairs is out of the office, the code only lives on one person’s computer
* Nowadays is too common in almost every company (and/or team) treat the pair programming approach with too much disbelief and shaking of heads

## Final insights

* Try new things! And do it anyway, in several times and different moments
* The worst that could happen is it doesn’t work, but then at least you’ll know you were right
* The best thing that could happen is you learn a new skill, your code improves, and you get closer with your teammates. These goodies worth each attempt, each investment in your team
* Working side by side with your teammates builds a level of trust that surpasses the pairing session. It brings you closer and makes your team stronger

## References

* Schrader J. *Late to the Party, but Nailing It! A Journey into Pair Programming*. php[architect] Magazine. Feb. 2020, pages 3-7
* [Pair Programming Cheatsheet](https://github.com/charliemc/pair-programming-cheatsheet/blob/master/README.md)

## Resources in this topic

* 📚 [Pair Programming Illuminated](https://www.goodreads.com/book/show/1762375.Pair_Programming_Illuminated), by Laurie Williams, Robert Kessler
* 📚 [Pair Programming, video-course](https://app.pluralsight.com/library/courses/pair-programming/table-of-contents)
* 🕸 [Tuple’s Pair Programming Guide](https://tuple.app/pair-programming-guide/)
* 📹 [Videos doing pair programming and TDD](https://www.artofunittesting.com/pairing-videos)
