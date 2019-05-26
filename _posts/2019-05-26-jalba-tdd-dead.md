---
layout: post
title: "JAlba 2019 Session Report - Is TDD Dead?"
date: 2019-05-26
---

Inspired by [this podcast series](https://martinfowler.com/articles/is-tdd-dead/) the purpose of this session was to discuss experiences with TDD and testing. 
The format of this post is mostly notes around the conversation, apologies if I have missed anything.
Happy to take a PR for any corrections/additions or contact me direct. 
Please feel free to add any thoughts on [Twitter #jalbaTDD](https://twitter.com/hashtag/jalbaTDD?lang=en). 

#### Code Coverage

Code coverage surfaced early on in the conversation and the impact it has on confidence.
It was suggested that some attendees avoid code coverage metrics and assume things are alright based on the code reviews of tests. 
Code coverage is a useful, but also a potentially dangerous metric. 
Code coverage alone does not measure the quality of the test or whether that test makes any useful assertions.
Several of the developers in the session had seen situations where teams had “chased” 100% coverage without practicing TDD, often introducing brittle or unnecessary tests to the system. 

#### Styles of TDD

Another point that surfaced near the beginning of the conversation based on my recent experience is the different styles of TDD. 
This is referred to as the [Mockist vs the Classicist](https://medium.com/@adrianbooth/test-driven-development-wars-detroit-vs-london-classicist-vs-mockist-9956c78ae95f) approach to TDD, we also discussed how the outside in approach (or London TDD) favours mockist.
In the room there was a mixture of approaches and generally a caution around tooling used for mocking.

#### Using Tools

Developers sometimes use tools without understanding why and follow patterns that may lead to not actually testing anything useful.
The point around understanding collaborators and expected behaviours through mocks brought about a differing of opinion, though I think everyone agreed that one of the biggest benefits of TDD is the design of code. 

It was noted that the use of PowerMock may highlight a code smell, and should be used with caution.
That said when testing legacy code PowerMock can enable tests on what otherwise could be really tricky. 
Many of the developers have used mocking and PowerMock when testing legacy code. 

#### How Many Tests are Enough?

Another topic of conversation that came up is the extremely high number of tests that some teams have ended up with.
In projects where there are a high number of tests the point was raised about this raising the amount of time take for the feedback loop to complete.
Are all the tests actually necessary? Do you end up having overlapping tests and is that a problem in itself?
As is often the case in technology the answer was "It Depends", but it certainly left me thinking about some of the value of our integration vs e2e tests.

#### Mutation Testing

Mutation testing is an interesting mechanism to test the tests to verify that the tests are indeed adding value. 
Some people have found value using mutation testing to validate the value of the test suite. 
Not to be confused with automatic property testing, which was mainly my misunderstanding! 

> Mutation testing (or mutation analysis or program mutation) is used to design new software tests and evaluate the quality of existing software tests. 
Mutation testing involves modifying a program in small ways.
Each mutated version is called a mutant and tests detect and reject mutants by causing the behavior of the original version to differ from the mutant. 
This is called killing the mutant. Test suites are measured by the percentage of mutants that they kill. 
> [Mutation Testing (wikipedia)](https://en.wikipedia.org/wiki/Mutation_testing)

#### Testing for Confidence

Tests are a way of gaining confidence in the codebase and in the changes applied to the product. 
Often writing good tests does take more time, but it is a pay it forward model.
Over time defect rates tend to be lower and it is easier to bring in new developers and allow them to be quite free with the codebase and applying changes.
One point was that it would be nice to visualise the tests executed across all the different types (unit, integration, e2e) etc and see areas of code with decent combined coverage.
It felt like that would take some instrumentation and metrics, but could be an interesting project.

An interesting discussion that evolved was centred around "is having some tests better than having no tests?" 
The conversation came full circle almost back to the discussion about code coverage.
A test that is not quality can actually be quite dangerous, luring developers into a false sense of security that things are working fine because the tests pass. 
One such example was developers encountering situations where a function simply asserts true. 

#### A more Extreme Approach

A more extreme form of testing is the Gitmo tool, that automatically deletes code that does not have a test. 
This is an interesting mechanism of policing the codebase for quality and ensuring nothing gets through without a test.
[Crap4j](http://www.crap4j.org) was another tool that was mentioned to figure out the quality of the codebase using the cyclic complexity metric.

#### Testing for Open Source projects

When it comes to running open source projects it was discussed that often PRs come through that are not tested.
This brought about an interesting point about learning TDD and indeed testing. 
If you are new to a language, framework and even testing then you end up having a bigger number of moving parts to figure out.
Spike development is a way to counter this, where you hack to find out how something works (or indeed to fix something) then throw it away.
It turns out that what is probably happening is the Open Source projects are often receiving spikes rather than quality code.

#### Summary

* BDD wasn’t discussed in the session, it would have been interesting to include some thoughts around this
* The TDD session was combined with other sessions about testing, the point was made that TDD is much more about the design of code and tests are almost a side benefit.
* It’s important if taking a mockist approach to refactor the tests and not leave any unnecessary scaffolding in place.
* Tests help us to gain confidence, but it’s a difficult balance as to when we have too many tests.
* Tools are awesome, but should be used with caution and understanding patterns and approaches to fakes, doubles and stubs etc is still quite relevant. 