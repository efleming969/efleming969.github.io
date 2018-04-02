---
layout: post
title: Story Driven Features
date: 2018-01-12
summary: A systematic approach that combines the lightweight aspects of stories with the comprehensive nature of specifications.
categories: Agile Testing
---

The User Story practice is one of the most misunderstood concepts in Agile development.  At its core is the fundamental idea of capturing what stakeholders need through _concise_ summaries that identify the who, what, and why of some functionality.

While it sounds simple, many teams struggle to identify what level of detail to collect at the outset.  In reality, details should be captured but not until the "last responsible moment" and without adding unnecessary overhead to the development process.

I suggest a more systematic approach that combines the lightweight aspects of stories with the comprehensive nature of specifications.  I'm calling it Story Driven Features.

## Starting with Stories

Stories are a mechanism for discovery and planning.  They are intended to be succinct but clear on intentions and typically evolve in an organic way.

We start with a basic "thinking" template that helps capture the desired functionality and potential value but lacks implementation details.

    As a User,
    I want a password strength indicator,
    so that I can ensure an appropriate level of security.

Additionally, many teams drop the _user_ indication, allowing for more flexibility in the template.

    When creating a new account,
    I want to see how "strong" my password is,
    so that I can ensure an appropriate level of security.

In this example, the _user_ is implied and explicitly stating it adds no real value.  This subtle change is sometimes referred to as [Job Stories](https://medium.com/the-job-to-be-done/replacing-the-user-story-with-the-job-story-af7cdee10c27) and can make a difference when trying to capture the true motive or intent.

Ultimately, teams should experiment with different patterns to find something that fits their style as long as they focus on concise depictions of potential functionality.

After compiling a list of stories, teams should have enough information to start estimating and prioritizing a backlog.  Then they decide to work on a set of stories and group them into a unifying goal.

## Working towards Features

When determining _how_ to implement a set of stories, the team will transition them towards _features_ that represent the **actual** capability of a system as opposed to stories that represent the **potential** capability.  This difference is the foundation for the story driven approach.

Features are communicated in a [domain specific language](http://martinfowler.com/books/dsl.html) (DSL) which is designed to help teams capture the specifications of a system.

~~~gherkin
Feature: Password strength indicator

  When creating a new account,
  I want to see how "strong" my password is,
  so that I can ensure an appropriate level of security

Scenario: Strength indicator message for weak passwords

  Given I'm creating a new account
    And providing an initial password of “jsmith”
   Then I should see a message of "your password is weak"

Scenario: Strength indicator message for mild passwords

  Given I'm creating a new account
    And providing an initial password of “P@ss0rd”
   Then I should see a message of "your password is mild”
~~~

One alternative style in this DSL would be to use [Scenario Outlines](https://github.com/cucumber/cucumber/wiki/Scenario-Outlines) that allow a more concise example with placeholders.

~~~gherkin
Feature: Password strength indicator

  When creating a new account,
  I want to see how "strong" my password is,
  so that I can ensure an appropriate level of security

Scenario Outline: Strength indicator message

  Given I'm creating a new account
    And providing an initial password of <password>
   Then I should see a message of "your password is <phase>"

Examples:
  | password        | phrase  |
  | jsmith          | Weak    |
  | P@ssw0rd        | Mild    |
  | Cowabunga! D00d | Strong  |
~~~

Specifications in this format provide a conduit between business and development teams.  They help flesh out the details with scenarios or examples in the domain's [Ubiquitous Language](http://martinfowler.com/bliki/UbiquitousLanguage.html).  The resulting collaboration enables teams to build "right" features.

In addition to bridging the communication gap, features can be linked to an automated acceptance suite that establishes regression tests as well as formal documentation.  There are several tools[1] that can provide this functionality, but they are beyond the scope of this post.

## Conclusion

Stories where never intended to be the final piece of the puzzle when it comes to Agile development.  They lack the necessary details for implementation and quickly become obsolete.  If instead they are used to drive _features_ in the form of executable specifications, then their purpose can be more clear.

As with anything in Agile, your mileage may vary so inspect and adapt.
