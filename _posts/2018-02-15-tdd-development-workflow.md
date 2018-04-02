---
layout: post
title:  Applying Test-Driven Development to Agile
date:   2018-02-15 00:00:00 +0000
categories: jekyll update
---
In the agile software development community, we value the delivery of working software through an incremental and iterative approach. Teams are encouraged to maintain close collaboration with project stakeholders in order to elicit feedback on their progress. This feedback is then incorporated into future iterations and, ideally, will increase the likelihood of producing the most valuable features.
However, as projects evolve, the code typically becomes increasingly complicated and requires additional amounts of cognitive reasoning. This mental overhead can affect the team's ability to adapt to new requirements and can ultimately hinder the agile process. Complexity is an agile killer and should be mitigated before too much technical debt is accumulated.
As a point of fact, great results can be obtained simply through the use of core engineering practices and test-driven development (TDD).

# Combating the Effects of Complexity

To combat the spiraling effects of an ever-expanding set of requested features, teams can adopt various engineering practices that help maintain a reasonable level of complexity while ensuring the necessary flexibility with regards to changing business needs. These practices include the use of modularization, design patterns, pair programming, continuous integration, and test-driven development.
Continuous integration can help strengthen the stability and confidence of a system that relies heavily on the presence of automated tests. Essentially, these tests provide constant verification of new and existing behavior that enables the team to experiment with alternative functional or technical implementations. Unfortunately, most teams struggle with adding a competent level of tests that voids many of the benefits gained from continuous integration.
In test-driven development, incremental and iterative programming concepts are married with the discipline of writing automated tests. If you look at any definition for TDD, you will find references to Kent Beck's Test-Driven Development. [1] In this influential piece of work, he describes the practice of driving development with automated tests—with the ultimate goal of producing clean code that works—with two simple rules: “Write new code only if an automated test has failed,” and “Eliminate duplication.”

# Starting the TDD Flow

In the TDD process, start with a particular story chosen from a prioritized list of backlog items that has been carefully crafted with appropriate information based on business value. With value in mind, the story should consist of a vertical slice of functionality, meaning that all components of the system are incorporated into the implementation. For a web application, this will typically include user interface elements and the associated server side functionality.
Let’s look at an example story about password strength introduced by a security expert stakeholder who needs a feature that encourages users to choose an appropriately complex password in order to help mitigate security breaches in the system (figure 1).

![Initial password strength story card](/assets/images/example-story.png)

Figure 1: Initial password strength story card

With additional conversations with stakeholders, the team has established a set of criteria to help complete the story forming the basis of the automated tests (figure 2).

![Completed password strength acceptance criteria](/assets/images/example-acceptance-criteria.png)

Figure 2: Completed password strength acceptance criteria

In this example story’s acceptance criteria, the use of imprecise language is an attempt to vaguely document what is expected to validate. This is intentional and will become evident as an important concept when applying TDD to the agile process.

# Test-Driven Functional Tests

The first bullet point on the story card in figure 2 describes an expected behavior from a user interface perspective and represents a good starting point for our overall story. This will be used to begin the test-driven approach.
In true test-driven fashion, a JavaScript test is written before implementing any production code, and because the scenario implies user interactions, an automation tool will be used to control a browser and to assert the expected outcome from the app’s user interface (figure 3).

~~~javascript
describe( "password strength messages", function() {

  it( "display a warning, when entering a weak password", function( done ) {

      browser.url( "http://localhost:8080/new-user-registration.html" )
        .setValue( "#password", "weak-password" )
        .getText( "#strength-indicator-message", function( err, message ) {
          assert.equal( "Your password is too weak!", message )
        } )
        .call( done );
  } );

} );
~~~

Figure 3: User interface test written up front

This test specifically opens a browser window, navigates to the appropriate section, enters a value for the password, and verifies that a particular message is displayed on the screen. Because it incorporates multiple parts of the overall story (a vertical slice), it is a functional (or end-to-end) test that is the primary driving force for the rest of our test code. Some people may recognize this approach as reminiscent of acceptance test-driven development.
The test will initially fail due to the fact that none of the functionality in question has been implemented, so our next step is to identify the minimal amount of code necessary to make the failing test pass to refactor or eliminate any code duplication. At this point, we could simply implement a shell of the overall functionality in order to get the test to pass. TDD encourages the simplest implementation, in contrast to adding code that may not be needed.
The second item of acceptance criteria in figure 2 is very similar to the first test case, but with a different expected outcome, verifying that the message output would have different colorized font.

# Test-Driven Unit Tests

To finalize the test case development for this sample story, the third criterion needs to be treated differently. While the test case in figure 3 was primarily focused on high-level interactions of the UI, isolated unit testing could be used to validate the third acceptance criterion. For example, figure 4 shows a test case that uses an algorithm that verifies password strength, starting with the simplest test that describes an expected outcome and then drives the implementation from there.

```javascript
describe( "password strength", function() {

  it( "less than 8 characters are not strong", function( done ) {
    var result = Passwords.isStrong( "short" )
    assert.equal( result, false )
  } );

} );
```

Figure 4: Test case that verifies password strength

This test uncovers the details of our initial implementation, such as establishing an appropriate naming convention for the function along with its inputs and expected outputs. It may seem trivial, but it's an intricate part of the test-driven mindset and critical for the overall flow of TDD.
Once the minimal code necessary has been implemented to make this test pass, move on to another scenario. Figure 5 shows testing for a negative condition and should be easy to add to the original test case.

~~~javascript
describe( "password strength", function() {

  describe( "passwords with 8 characters", function() {

    it( "letters, numbers and no specials are NOT strong", function( done ) {
      var result = Passwords.isStrong( "1234567a" )
      assert.equal( result, false )
    } );

  } );

} );
~~~

Figure 5: Test case enhanced to validate negative conditions

Adding test coverage to an established base line to reject weak passwords has the benefit to evolve the test code in an incremental fashion. This approach can be pivotal in a developer's adoption of TDD, as it takes time to learn what sequence of tests is needed to drive an implementation. Figure 6 verifies the positive outcome of providing a password that is approved by the strength algorithm and should help drive the bulk of the implementation logic. When implementing the minimal amount of test code, existing test cases should be run to provide feedback on the effects of any new code.

~~~javascript 
describe( "password strength", function() {

  describe( "passwords with 8 characters", function() {

    it( "letters, numbers and specials are strong", function( done ) {
      var result = Passwords.isStrong( "123456a!" )
      assert.equal( result, true )
    } );

  } );

} );
~~~

Figure 6: Enhancing test case

With practice, an engineer can start moving between the test and implementation code with ease, and quite possibly fall into a natural cadence.

# Conclusion

TDD is a disciplined technique for validating implementations, but it is also useful to incrementally discover the best implementations of a proposed system. This has the following benefits:

* Encourages radical simplification of the code
* Allows the design to progressively evolve as business requirements change
* Encourages developers to remain focused on implementing what is needed to make a test pass
* Provides a significant level of test coverage to help maintain a healthy confidence in the system
Incorporating TDD into your software development process is a mechanism for increasing developer confidence around code changes, and therefore it can encourage experimentation even in the wake of a growing code base. TDD is not a silver-bullet solution to the problems introduced by complexity and change, but with practice and commitment, it can be a significant tool in a team's arsenal.
