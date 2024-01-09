# Alistair in the "Hexagone"

Alistair Cockburn - Meetup: DDD FR

------

**Available resources**

-  [Part 1](https://www.youtube.com/watch?v=th4AgBcrEHA), [part 2](https://www.youtube.com/watch?v=iALcE8BPs94), [part 3](https://www.youtube.com/watch?v=DAe0Bmcyt-4)
-  [Event meetup-page](https://www.meetup.com/DDD-Paris/events/240715351/)

🏷️ Tags: `talk`, `meetup`, `hexagonal`, `architecture`

------

## General notes

- The sketch of this talk contains four parts: 1. Why an hexagon. 2. ? (don’t understand) 3. Sample about a pizza shop. 4. Dip into use cases, talking about primary and secondary actors
- Design pattern in architecture called “[light on two sides of every room](https://santacruzarchitect.wordpress.com/2014/12/23/pattern-language-no-159-light-on-two-sides-of-every-room/)”
- There’s a dependency, so you want to be able to parametrise it. Dependency injection is the thing you do to achieve a configurable dependency
- When you look at the left side in the right side of the hexagon, we’re going to configure the dependencies differently, so the code on the left side is completely different than the code of the right side
- Hexagon has a configurable dependency on the left and also right sides
- About ports: what is it for; you get a verb in English which ends with *-ing*
  - Sample, this interface here (port) is for adding events. Its purpose in life
  - So a port is an intention of the dialog, it’s not the technology. You want to be absolutely technology neutral
  -  
-  

