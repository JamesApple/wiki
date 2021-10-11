# Implemnenting Domain Driven Design by Vaughn Vernon

## Before I Begin (Meta)

### 1. Why are you reading this

Plenty of people have recommended the red DDD book as a more hands on version
of the blue DDD book.

After having seen the technical complexity that comes from building
applications without engineers having an understanding of the business domain
at small and large companies I want to see some practical use cases where
companies have moved towards DDD and how they worked with stakeholders, other
engineers and senior management to accomplish their objectives.

It is a strong belief of mine that code is communication much like creative
writing and we all need to learn to tell better and more concise stories with
our software.

### 2. What do you hope to gain

Having read the blue DDD book previously I understand the concepts but my
implementations are guided by guesswork and I'd like to see how other
professionals have approached the problems I have encountered.

### 3. What is DDD to you

DDD is a set of patterns and practices that help translate a domain like
psychology, construction or project management into software that reflects its
purpose in its source code as well as in the way teams communicate.

### 4. What do you not want to get out of this book

I do not want to adopt an overly verbose and heavy version of DDD
implementations that insist on creating incredibly complex class structures for
small applications that seem to come from anyone working in Java. Many of the
examples I have read have referenced patterns that smell way too much like Java
specific advice.

I firmly believe DDD strategic design is beneficial in vanilla JS
microservices, .NET monoliths and Rails apps even if no reference to "entity"
or "aggregate" is found in the code. A consistency boundary can be enforced
without a Java Bean.

# 1: Getting started with DDD

* Quality accrued from testing does not equate to a quality software model.
  Even 100% bug free software does not necessarily equate to a well designed  model.
* DDD is not primarily about discussion, listening understanding, discovery and business value to create a Ubiquitous Language
* DDD requires you to humble yourself and attempt to converse with those who rarely if ever speak technically.

## Why DDD

* Put domain experts and developers on a level playing field to educate the developers _and_ the domain experts.
* Nobody knows everything about tthe software
* Centralize knowledge to ensure that undestanding does not beome tribalistic.
* The design is the code and the code is the design.
* The objective is not to model the real world but to create a model that most appropriately serves the business. The useful and realistic models do not always overlap.
* DDD in technology is succesful when it is highly testable, less error prone, performs to SLA's, is scalable and allows distributed computing.
* Whiteboarded diagrams are not the design. The code is the design and the whiteboard diagram is an effective method to discuss problems.
* Bounded contexts are smaller than you expect
* The language is only ubiquitous in the scope of a single bounded context

## When not to use DDD
1. When the required solution is primarily a CRUD product where almost every
   operation is a simple database query.
1. You trust your users to insert the correct data
1. If your system requires 30 or fewer business operations

## When to use DDD
1. Are users asking for more complicated features early on in iteration?
1. You expect the features of the product to change over years to come
1. You don't understand the domain because it's new or nobody has done this before

## What is an anemic domain model

1. Do your models contain mostly public getters and setters and no business
   logic or almost none. Like attribute holders.
1. Does the majority of your application logic exist within the service/application tier instead of the model

## What to do if your domain model is anemic

Either:
1. Embrace it and accept usage of either Active Record or Transaction Script patterns instead.
1. Correct it immediately

## How did it get anemic

The majority of code is copy pasted and that code is primarily designed around
showcasing a particular API instead of displaying best practices or application
development.


## Describing business actions

1. No DDD
```js
patient.setShotType(ShotTypes.TYPE_FLU)
patient.setDose(dose)
patient.setNurse(nurse)
```

2. "We give flu shots to patients"
```js
patient.giveFluShot()
```

3. "Nurses administer flu vaccines to patients in standard doses"
```js
const vaccine = vaccines.standardAdultFluDose()
nurse.administerFluVaccine(patient, vaccine)
```

## How to capture the language

1. Draw pictures or diagrams of the concepts and actions that occur. Avoid
   heavy handed UML or any ceremony.
2. Create a glossary of terms with simple definitions
3. If a glossary is too heavy capture some documentation such as the images above

Understand that the above are temporary. The only enduring language is the one
that the team speaks and the one in the code.


## Questions

* When is DDD not appropriate
  * See When to use DDD
* How to start a project that needs DDD
* How to sell DDD to management
  1. The org gains a useful model of the domain
  1. A refined, precise definition and understanding of the business is developed
  1. Experts contribute to the design instead of being antagonists
  1. Bettuer user experience
  1. Clean boundaries are placed around models
  1. Architecture is better organized
  1. Agile, iterative, continuous modeling is used
* What common mistakes do people make when beginning DDD
  * Anemic domain models
  * Thinking that business/industry standard terms need to be forced on the tech team instead of building a language together.

# 2: Domains, Subdomains and Bounded Contexts

## Questions

* What is a Domain
* What is a Subdomain
* What is a Core Domain
* What is a Bounded Context
* What is a Ubiquitous Language
* What are the consequences of misapplying DDD practices


# 3: Context Maps


## Questions
* What is a Context Map
* What are some good examples of a context map


# 4: Architecture

## Questions
* How does DDD affect the architecture of an application

# 5: Entities

## Questions
* How are entities overused
* Why are they overused
* What is a well designed entity
* What is a Value Object

# 6: Value Objects

## Questions
* How do you identify the difference between a value object and an entity
* How do you identify the fields that belong on an entity and a vlue object
* How do you write domain driven tests

# 7: Services

## Questions
* What is a service in DDD

# 8: Domain Events

## Questions
* Where are Domain Events published from

# 9: Modules

## Questions
* How do you name and choose the right modules for your system

# 10: Aggregates

## Questions
* What is an aggregate
* Why is the consistency boundary important
* What are some good and bad aggregate boundaries

# 11: Factories

## Questions
* When to use factories

# 12: Repositories

## Questions
* Why should repos be built like collections and not DAO's

# 13: Integrating Bounded Contexts
## Questions

# 14: Application
## Questions

# A: Event Sourcing Aggregates A+ES


# Quotes

> It seems to me that Scrum and other agile techniques are being used as
> substitutes for careful modeling, where a product backlog is thrust at
> developers as if it serves as a set of designs.
> Most agile practicioners leave their daily standup without giving a second
> thought to how their backlog tasks will affect the underlying model of the
> business. Although I assume this is needles to say, I must assert that Scrum,
> for example, was never meant to stand in place of design.
- Preface XXVII


# Next books on the subjet

* Applying Domain-Driven Design and Patterns: With examples in C# and .NET by Jimmy Nilsson

