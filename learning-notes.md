# How this works

**tl;dr**: I open an issue for every resource to study ([issues](ebarbeito/learning-notes/issues) represents the backlog / to-do). I push a git-branch and a PR for every resource in-progress ([pull requests](ebarbeito/learning-notes/pulls) represents the current learnings). I use some kind of _git-flow_ with `article/`,  `book/`,  `talk/`, etc. branch-prefixes;  `master` branch only provides stable notes. And all this is reflected within a [project board](https://github.com/ebarbeito/learning-notes/projects/1).

## First of all, why

While reading or attending some learning resource, I tend to make some active work around. This is usually by highlighting, taking side notes, googling around, typing the examples provided, or trying to modify them just to know how it works.

Sometimes I am learning from one single source, but others I am on several ones at the same time. Sometimes I am using different computers, or reading from an ebook device, or using physical mediums like paper books or living conferences where I try to take notes and thoughts on paper to, then, transcribe it to digital format.

Lot of resources, several available mediums and different ways to collect what I am on, they make messy how I try to study. This is why I started to do it in this more explicit way, to improve the methodical workflow by itself, in order to learn more actively, to loss less focus, to force me to type and reread again what I've learnt, to keep track of each resource that worths the effort, etc.

## How I do it

I changed my previous way to collect my learning notes, into this thing. I am using not only a git repository, but also the Github tools like tags, issues, pull requests and a board. At first glance, all of this sounds like too much, but to me it makes sense, is a quite simple workflow and, foremost, is being a good way to keep track of all the things.

The overall process is usually as follows:

* If I find some resource enough valuable (and long) to make the effort,
  * ... I save it in the backlog (the issues) only if I am not going to start with it.
  * ... I create a new git-branch with an empty file for this resource and push it, opening a new pull request
  * ... I use tags to group every issue and pull request
* If I 
* With the above, I keep track of what I am studying and what I am (or was) interested in study next
* Every pull request is dedicated to a single resource (article, book, talk, etc.) and they have chunks of information (highlights, notes, images, etc.) that I put in several commits, until I finish
  * These commits are usually made in different days
  * Taking notes, rereading and typing highlights, etc. slow me down and I need a lot of much time to complete a reading, a talk or whatever
  * I try to focus only in one PR, but this is the harder part to me and is just not possible the most of the cases. Usually, I need weeks or months for books, days or weeks for articles, some days for talks, etc. because I only invest a very few hours in a row per day (or I stay for days or weeks without keep studying)
* Once the PR is done, I squash the commits, merge to `master` and close its PR.

### Git branches

As said before, a dedicated branch is created (from `master`) for each resource added to the repo. The branch names follows the convention "`prefix/title-or-something-about-it.md`". Prefixes are descriptive:

* `article/`
* `book/`
* `course/`
* `talk/`
* `topic/`

### The process

This is a manual process, so even if I try to be as methodical as possible, it doesn't care if I end up not following some of the above conventions in the strictest way. There are no fixed rules, indeed.

#### Reading a book

> 📚 See the the [open pull requests](https://github.com/ebarbeito/learning-notes/pulls?q=is%3Apr+is%3Aopen+label%3Abook) for the current books I am currently studying

It doesn't matter what format I am consuming; can be electronic (epub, pdf, etc.) or paper, but I always try to highlight those chunks of information which consider they teach me or remind me valuable knowledge. Additionally, I like to take sidenotes, going back or googling around while reading.

Right before starting a book, I `pull` the `master` branch and `push` a skeleton for the future notes I will collect here. The steps I follow are:

```
# Start a new branch for the new book
git checkout master \
    && git pull \
    && git checkout -b book/title-of-the-new-one

# Add the new file writing some skeleton inside
touch books/title-of-the-new-one.md

# Commit and push the new work-in-progress book
git add . \
    && git commit -m'Add book: Title of the new one' \
    && git push origin book/title-of-the-new-one

# Open a new pull request
```

The steps I follow opening the pull request are:

* Add the preffix `[wip]` in the title. This is to know which PRs are in progress, and which are pending to merge (once I finish a book and its notes, I update its PR removing the wip-preffix, but before merge I like to read the result to remind it)
* Add the tag `book`, plus a set of other ones related with the content
* Fill the description with the title of the book, author, some abstract and link(s) to get more info
* Attach the PR to the [kanban project](https://github.com/ebarbeito/learning-notes/projects/1) (this action moves it to the *In progress* column)

If the book was first added to the [issue list](https://github.com/ebarbeito/learning-notes/issues) (*the backlog*), then I go and delete its related issue to keep the upcoming backlog clean.

