# Contributing to an Open Source Project on GitHub

by Kamran Ayub – Pluralsight

------

> Where do you begin to get involved in an open source project? In this course, Contributing to an Open Source Project on GitHub, you will learn foundational knowledge of how to be an effective open source contributor. First, you will explore how to find a project that suits your interests. Next, you will discover how to tackle common pull request scenarios in the real world by working with others. Finally, you will learn how to leverage GitHub's social features to keep updated on what you care about most. When you are finished with this course, you will have the skills and knowledge of working with open source projects needed to make high-quality contributions you will be proud of

**Available resources**

* [An example open source project for this course, for demo purposes](https://github.com/wired-brain/wired-espresso)

🏷️ Tags: `course`, `2019`, `pluralsight` `git`, `github`, `open-source`, `teamwork`

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

* Dealing with Conflict

  * Alongside that, diversity of ideas and backgrounds will emerge. Conflict

  * Conflict in an open source project is not always a bad thing. Types of conflict

    | Good conflict             | Bad conflict                      |
    | ------------------------- | --------------------------------- |
    | Healthy debate            | Personal attacks                  |
    | Disagreement with empathy | Harmful or disrespectful language |
    | Difference of opinion     | Violent behaviour                 |
    | Constructive criticism    | Conduct violations                |

  * Sample in an opened issue with some change proposal (related with adding a dependency)

    * The scenario is a great example because tooling changes can often generate debate amongst contributors
    * Conflict appears: "I don't think we need yet another dependency"
    * A classic example of a maintainer opposing a new change they believe might inhibit the project in some fashion

  * Let's cover some tactics you can use to work through this conflict

    * The first instinct, when seen a comment that opposes your idea, might be to get defensive
    * Try asking a question instead. The impediment was not reasoned, so we want to get at the why
    * For example, I might ask: "Is there some overhead you are specifically concerned about?"
    * Just ask and try to listen
      * We can't be sure of the motivation behind
    * (...) he/she follows not convinced, doesn't want to get your point, or you think is not taking you into account, etc.
      * Uh, okay, that's frustrating. So, I'm going to give her/him a piece of my mind! $*#%&@!
      * Hold on. Okay, Okay. Wait a minute. Hold on. Take your hands off the keyboard
      * Sometimes it's just best to take a break and cool down when you feel the heat rising, that's a sign
    * Let me share a trick with you
      * My wife works with kids all day at school as a social worker, and she tells them that when they feel angry about something, ask themselves whether it's a little deal or a big deal
      * Asking yourself this question in the moment can help immediately ground you
      * Let's face it, someone not wanting to use a tool I like on an open source project is probably a little deal. Just that simple mindset shift can make all the difference
    * Let's refocus back to the first (more visceral) comment you wanted to post
    * Now that we've had our moment, let's think about what they're trying to say
      * Their argument is that there's already a code style guide people can follow. So why introduce yet another tool now?
      * Rewrite your own comment in order to focus on the ideas (avoiding the visceral parts). Because it will contain the key points you could focus on
    * In a worst case scenario, let's say he/she left a comment like where attacks are personal character and claims that by pushing our opinions on that we must hate the project and all it stands
      * Let you imagine what else someone could say that would be considered disrespectful or harassment
    * In this situation, it might be best not to respond and instead consult the code of conduct to report a violation
    * If a project has a healthy community, they would certainly take action against this kind of (harmful) conduct

  * The topic of communicating effectively is larger than the scope of this course

* Working with Code Reviews

  * PR are the primary way to get feedback on your contributions, and there's several features you'll likely used during the code review process
  * We initially opened this PR up as a draft because I had some work in progress (...) I've pushed those changes (...) so we can now mark this PR as ready for review
  * To do that, I'll scroll down here and click the button "Ready for review"
  * This doesn't necessarily notify the team that your PR is actually ready
  * I'm not going to tag maintainers within a PR comment, though. Instead I'll go back to the original issue and leave a comment there
  * It's possible that anyone who is interested in the issue is watching it as shown here on the right side
    * "Hi @johndow, I just opener a PR to #9 to address this issue if you'd like to take a look. Thank you"
  * Reviewers can leave suggestions which you can incorporate without going back to your local repositories
  * We can add suggestions to batch and then commit those added ones
  * Wait until the conversation is over, before resolving it
  * Leave comments to provide context or reminders

* Undoing or Reverting Changes

  * Undo specific lines using interactive patching
    * An interactive checkout to the commit right before the one where is the not desired changes
    * Checkout to that commit using patching mode, `git checkout -p <commit>`
  * Undoing a commit with `git revert` command
  
* Rewriting History and fixing mistakes

  * Rebasing
  * Perform interactive git squash
  * We'll also go back and fix typos by amending last commit
  * Using a force push
  * What do you need to know? From a practical standpoint
    * @johndoe has asked us to rebase our commits to clean up our history
    * There's an important reason why it should be before pushing. It's because you should never rewrite public history

* Recovering from a Public Rebase

  * Demonstrate how rewriting public history might have affected another contributor
  * How we can recover from it

* Integrating Changes from Others

  * On a large and active open source project. There are several scenarios
  * Allowing edits from maintainers can be helpful during the pull request process
  * How to pull in changes from another users fork in case we need to base our work off of someone else's
  * How to pull down a pull request directly into our local repositories, which you may need to do if a user has deleted their fork continuing from where we left off

* Splitting a Pull Request Apart

  * Scenario where we need to split apart of pull request and bring specific commits into a new PR
  * There are three approaches will look at that are commonly used
    * How cherry pick command lets us pull specific commits into a new branch
    * How cherry-pick specific files from a commit
    * Alternative approach to split a large PR by creating and merging sequential branches

* Checking, Merging, and Closing Pull Requests

  * Merge a pull request if we have access
  * Clean up after ourselves by deleting stale branches
  * Take into account some etiquette to follow when closing a PR
  * It's common for teams to set up automated tasks like continuous integration, continuous delivery, linting code coverage or other checks that can prevent your pull request from being ready
  * @johndoe has approved our PR. It's time to merge
    * If you're an external contributor, you'll likely not need to worry about this as much because you'll see this message here about not having right access to the repositories
    * if you do have access, there're several ways of PR can be merged
  * Button that allows me to merge the PR
    * Squash and merge
    * Rebase and merge
    * Create a merge request

### Summary

* Keeping your work tidy and organized makes for an easier time in cases where you have to go back and split apart, work or fix mistakes
* Descriptive branch names, commit messages and tried to keep work scope to specific issues on pull requests
* Spending time on the details will help you save time and headache down the road as you work on more complex changes

## Staying Updated With Social Features

* Keeping updated on your interests
* Following a project by Stargazing and Watching
* Subscribing to issues and pull requests
* Following and unfollowing users
* Setting your activity status
