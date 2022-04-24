# Pairing

https://www.martinfowler.com/articles/on-pair-programming.html

# Pairing styles

## Driver & Navigator

Allows two opposite perspectives on code at all times during development.

### Pros and Cons



### Driver

Manages the keyboard and focuses on syntax and immediate code
issues.

### Navigator

Focuses on the big picture. They're the one with the big picture
in mind at all times. They should be focused on the logic of your task absent
the little details.

### Flow

1. Define your task
2. Agree on a small goal defined by a pre-written commit message or unit test
3. Switch roles often
4. Save long term or medium term thoughts or ideas for once you perform the
   swap and discuss then. Protect the drivers flow.

## Ping Pong

TDD incarnate within pair programming. Suited more to conferences or fun
projects than serious work.

### Flow


1. A writes a failing test
2. B writes the implementation
3. A&B refactor together
4. Repeat

## Strong Style Pairing

https://llewellynfalco.blogspot.com/2014/06/llewellyns-strong-style-pairing.html

Designed to facilitate knowledge transfer from one individual to another. This
is ideal for onboarding new employees in a hands-on fashion and developing
skills in weak spots across the team.

### Flow

Focused on increasing communication, collaboration and knowledge transfer.

All code must be written by the driver. This is the only way for knowledge to
transfer practically.

### Driver

* Trust your navigator
  Ask questions when required about _how_ but leave the _what_ until the small
  piece of work is complete
* Become comfortable working with incomplete understanding
  Trust your navigator. If the navigator attempts to explain everything to you
  it may take way too long for the task to begin. Understanding will come as
  you work.
Communicate if you want to change

### Navigator

* Speak at the highest level of abstraction your driver understands

* Give instructions as soon as the driver is ready
  You need to already have planned what you will be working through as you
  begin to code. While you manage keeping track of these items, your driver
  should be focused _only_ on the task of writing code at all times to allow
  them to stay in flow.
  This style emphasises working with varying skill levels so you should be able to 
  change communication styles from "extract that method" to "press ctrl alt m"
  to "highlight lines 7-14 and press ctrl alt m" within the same session.  You
  must always keep the level of your pairing partner in your mind without
  becoming pedantic.
  If you make a mistake or have a bad decision correct it. Don't steam on how
  to avoid it. If someone has an idea have them swap roles so that you have a
  part in driving as well

You may tend to keep your hand on the mouse if you are the navigator especially
in code-focused work. This may assist in gesturing or dictating areas of code

## Pair Development

Handle all development activities as a pair. This is a multi-day process that
can involve everything from requirements gathering to implementation and
deployment

### Flow

#### Plan

1. Understand the problem. Ensure requirements are clear and there is an
   obvious completion point.
2. Come up with a solution. Either do this together or apart. Ideally a short
   design document will help keep you on track
3. Plan your approach. What steps are needed to achieve the goal. Write a short
   list of tasks to achieve your design document

#### Research

When developing an unknown feature or using an unkown tool/tech you will need
to split up. Research is ineffective in pairs.

1. Define a list of questions that need to be answered to create a solution
2. Split. Divide the questions between the two or each come up with your own answers
3. Get back together after timebox completion.

# Rotate pairs

Teams can rotate team members across pairs as long as an "anchor" is kept in
one team for the duration of a build.

Show and tells are important to ensure developers across the business
understand new learnings, designs and tech debt.

# Planning

If either of you have meetings or other obligations through the day ensure that
you agree to some core coding hours and treat those as sacred

If you have a crazy IDE setup, keyboard or other config, install both of your
preferred configs on the same machine so that you can code in your style.

# Avoid

* Phone use
* Emails
* Staring off into the void
* Micro managing the driver
* Immediately point out logic/typing errors
* Hogging either the driver or navigator role
* Pairing all day

# Why Pair?

* Actively fights knowledge silos
  Can help extract knowledge from some members
* Removes code review (!)
  As you are already working with a peer most teams will not need to have a
  formal code review process. This allows code to be integrated faster and less
  team friction to occur. If you need additional code review after pairing,
  you're doing it wrong.
* Ownership spread across team
* Reduces possible WIP
* High focus
* Helps stay on task
* Improve onboarding speed
* Team cohesion
* Force experts to explain themselves
* Higher quality
  Pairing forces you to speak aloud and rationalize your work as you go. This
  means others will have an easier time maintaining your work.
* Endure pain now, rather than later (XP)
  Code review, bugs and meetings can be pushed forward by practicing pairing
* Satisfaction.
  Frequently I find myself much more satisfied with a days work after pairing

# Challenges

* Exhaustion.
  Pairing requires taking real breaks. Never pair for more han half your
  workday.
* More easily interruptible
* Varying skill levels
* Pairing with new/unknown requirements/tech
  Consider doing a spike instead
* Finding time on your own
* High rotation rate leads to context switch

# Reasons to not pair

* Boring / Boilerplate tasks
* Low confidence developers need time to achieve a goal on their own

# Why to replace code review with pairing

| The advantage of pair programming is its gripping immediacy: it is impossible
| to ignore the reviewer when he or she is sitting right next to you.
| -- Jeff Atwood

1. Code reviews always end up sloppy
  * One side of the review process always relies/trusts the other too heavily
    without making that explicit.
  * The coder might defer refactoring subconsiously expecting issues to be
    caught in the review process.
  * The reviewer relies on the coders dilligence trusting their work and not
    digging too deeply to catch these issues.
  * *Sunk cost fallacy* We are usually reluctant to cause rework for something
   that has already been invested in
  * Code review reduces velocity and can frequently prevent trunk based development

# Misconceptions

* Agile requires pairing
  XP requires pairing. Agile does not.
* Pairing reduces the productivity of developers
  This is only true if the hardest part of development is typing.
  A pair is more productive than two seperate developers. This is due to all
  the points above
* Pairing for boring code is a waste.
  Pairing for boring code may illuminate better abstractions that save everyone time in the end.


# Common issues

## 1 person working and 1 person watching

If the navigator isn't paying attention or engaged then you
aren't even pairing. This is even worse in the strong style

## Fighting over the keyboard

Rude. You're both adults.

## Expert/Novice pairing

The strong style is ideal for this as the expert is almost
always in the navigator position. If the navigator wants
anything done they have to communicate it to the driver. The
driver then has an easier time of onboarding and becoming
proficient faster.

The expert gains by having to explain. (if you want to learn something, teach.).
The expert is also able to stay at a higher level of
abstraction, keeping the logic of the system in their mind
instead of 



# Misc

> Merging code into trunk on a daily basis; Having branches or forks with very
> short lifetimes (less than a day); Having fewer than three active branches."

*USAF guide to focused skill acquisition*
https://apps.dtic.mil/dtic/tr/fulltext/u2/a084551.pdf

*Patterns of effective teams*
https://www.youtube.com/watch?v=lvs7VEsQzKY

*Power of agile?*
https://www.youtube.com/watch?v=YL-6RCTywbc
