# Contributing to an Open Source Project on GitHub

by Kamran Ayub



* [An example open source project for this course, for demo purposes](https://github.com/wired-brain/wired-espresso)

------

## Course overview

* Practical skills to become an effective contributor
* Threw a simulated project where we'll tackle common scenarios you're
* Major topics that will cover
  * Exploring projects that interest you
  * Identify and good first issues to work on communicating effectively and responsibly handling common pull request scenarios and following the user's issues and pull requests

## Getting Involved in an Open Source Project

* Contributing to open source is great for personal and professional growth
* Contributing doesn't just mean coding. Takes many forms
* Where to contribute?
  * Own projects, but not necesary if you don't have a specific one in mind
  * Learn how to find projects that interest you
* What to look for when on boarding to a new project
* How to perform a quick pulse check that will give you a sense of whether the project will be a good fit for you
* Benefits of Contributing to Open Source
  * Nearly all companies run on open source somewhere in their business
  * Personal Growth
    * Contributed. Open Source can Help You Grow Personally
    * One of the biggest benefits is the opportunity to give back to the community
    * Contribute to open source projects that have a real impact on people / help others
    * It gives you a space to experiment and grow your skills, learn new technologies and build your talent
    * It helps to find and meet other developers. Many projects have more than one maintainer
    * Popular projects have hundreds of regular contributors
      * As you contribute more and more to a project, inevitably, you'll start to develop relationships with maintainers and contributors alike
      * These kinds of relationships at riel value to your life and could open new doors you previously didn't think were possible
    * Move from a consumer role, to a creator one
    * Discover something you enjoy working on. What are your hobbies? Skydiving, video games, board games, rock climbing, hiking? There are open source projects related to all
  * Professional Growth
    * Finally, contributing to open source can be financially rewarding when a project get sponsorship crowdfunding and makes a real impact on the broader community
    * Make your work experience portable. Which means that anyone could see your code, the way you interact with others and your experience
    * Open alternative paths to getting hired
    * Learn how to work effectively on a team
    * Develop interpersonal communication skills
  * In short, open source is an incredibly rewarding way to invest your time
* Understanding the Different Types of Contributions
  * Contributing is more than writing code
  * Three main contribution categories: Planning, social and development
  * Planning contributions
    * Deal mainly with helping to assist an issue management or helping plan new features
    * Helping fill gaps in the issues (missing information, provide reproduction cases, etc)
    * Providing feedback on usage and features
      * Some projects put out requests for comments (RFC). Common to need members of the community to weigh in on important new features or major changes
      * This is a great opportunity if you use a project heavily to provide input on its direction and influence development
    * Participating in community standups or planning sessions
  * Social contributions
    * Focus on the community, aspects of a project and marketing. Yes, even open source needs marketing
    * Open source software is still a product, and products need marketing so people know about and want to use them
    * Answer questions and help others
    * Write documentation or tutorials
    * Host events or help marketing efforts
  * Development contributions
    * The primary way to contribute to development is by implementing features or fixing bugs
    * Participate in code reviews, once you're comfortable contributing to the project. Helping to review code style, provide feedback or offer implementation advice, etc
    * Write process or workflows that can be automated. Helping to increase the productivity of everyone who contributes
    * Offer financial support, and I'm categorizing this as contributing to development because this helps support the developers and keep the project funded
  * This course is mainly gonna focus on contributing, and both the planning and development areas as well be looking at scenarios when working on issues and submitting pull requests
* Finding a Project to Contribute To
  * One option is the [Explorer feature](https://github.com/explore)
  * Browsing projects [by topic](https://github.com/topics)
  * Discovber projects [by collections](https://github.com/collections)
  * Find [trending projects](https://github.com/trending)
* Onboarding Yourself to a New Project
  * GitHub has help standardize a few key pieces of information across projects
    * Repositories could have a `.github` folder with all this kind of information (samples: [ionic](https://github.com/ionic-team/ionic/tree/master/.github), [symfony](https://github.com/symfony/symfony/tree/master/.github))
  * The most common ones are: the contributing guides, a code of conduct and licensing agreements
  * Contributing and Setup Guides (samples: [ionic](https://github.com/ionic-team/ionic/blob/master/.github/CONTRIBUTING.md), [symfony](https://symfony.com/doc/current/contributing/code/index.html))
  * Code of Conduct (samples: [ionic](https://github.com/ionic-team/ionic/blob/master/CODE_OF_CONDUCT.md), [symfony](https://symfony.com/coc))
  * Contributor License Agrrements (CLA) (samples: [ionic](https://github.com/ionic-team/ionic/blob/master/LICENSE), [symfony](https://github.com/symfony/symfony/blob/master/LICENSE))
    * [Software Licenses in Plain English](https://tldrlegal.com/)
    * An agreement between you and the company that owns the project that defines the terms of your contribution, such as assigning rights and ownership
    * It's really important to mention that if you are doing work on behalf of your company or even while at work, you may need to have your company's legal counsel look over the document and review it
  * Checking the Pulse of a Project
    * Before contributing to a project it's good to make sure the community around is healthy and supportive of new contributors
      * Is an important step if you're looking to get more involved on a project over time
    * What to whatch out for
      * Lack of contributing documentation. Make it harder to how to contribute, what's expected
      * No easily identificable issues for first-time contributors. Samples: use of "help wanted", "good first issue" or labels designed like that
      * Stale or outdated pull requests
    * Poor attitude or conduct amongst maintainers

## Preparing to Make a Contribution

* Demo: Working with a Fork Using [GitHub Desktop](https://desktop.github.com/)

* Demo: Working with a Fork Using the Command-Line

  * Clone a forked repo
  * Adding an upstream remote
  * Syncting upstream changes

  ```shell
  $ git clone https://github.com/ebarbeito/wired-espresso.git && cd $_
  $ git remote # origin
  $ git remote add upstream https://github.com/wired-brain/wired-espresso.git
  $ git fetch upstream master
  $ git log HEAD..upstream/master --oneline
  $ git pull upstream master
  $ git push origin master
  ```

* Shortcut command to keep all your git remotes up to date

  ```shell
  $ git remote update
  ```

  It will fetch both the origin and the upstream remotes

* Finding Work and Engaging the Team
  * Looking through the issues list to find some issues to work on and walk through some situations when you interest by a specific issue
    * Looking at an issue labeled witg "help-wanted". A maintainer usually wrote the issue requesting for changes (features, bugfixing, etc.)
      * Explain the context and/or the things are needed to be done
      * Should also give a proposed solution, how to make the change (if exists a clear path) or what to do
    * The same as before, but you wrote the issue with a change request
      * Maintainers can not be interested and close it
      * Or can be interested, they should give some guidance how is needed to make it happen, request a PR, etc
    * In the issue someone else said that he would like to work on this but it was several months ago
      * Leave a comment asking the other person: "Hi @johndoe, saw your comment and wondering ig you are still working on this?"
  * Before I go and start writing code, it's best to signal my intent to work on this issue. How to do it?
    * Add a comment: "Going to take this" → Avoid telling it in that way, cause maybe someone else is already working on it
    * Add a comment: "Is it available to work on" → Better
  * Drafting a Great Pull Request
    * Open a draft pull request anytime your changes aren't yet ready to be reviewed
    * A PR is the primary way you'll be getting feedback and engaging with the team on a new set of changes
    * Provide as much information as you can with a description and bulleted list of major changes
    * Make sure the link to the related issue if there is one
    * Optional: Include a checklist to track my progress
    * Ask any question or mention specific feedback needed. Include reviewers you have questions for reviewers
    * If you're changes aren't yet ready for review, be sure to open a draft pull request
      * Use the button "Draft pull request"
    * Read and follow the PR template if it's provided

## Collaborating Effectively on Pull Requests

.

## Staying Updated With Social Features

.