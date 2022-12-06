+++
title = "Testing"
description = "Our standards around testing"
date = 2021-05-01T18:20:00+00:00
updated = 2021-05-01T18:20:00+00:00
draft = false
weight = 420
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

We have always been big believers in automated testing and Test Driven Development ([TDD][]). Back in the day we were even big believers in Behavior Driven Development ([BDD][]).

Over the many years we have learned that [Cucumber][] and [Gherkin][] were nice ideas but we never really saw huge value and it required additional adoption from Product which was always a hurdle. 

The concept of **emergent design**, the process of evolving application architecture from mocked tests, sounds great in theory. But in practice we didn't see any real added value once you have written the non-mocked tests. This is because the mocked tests become brittle noise that isn't worth maintaing and in fact interfere with the process of refactoring functionality. Even if you take the stance of removing these mocked tests, the development team has then taken on the burden of identifying which tests to remove and when to remove them. This is simply more overhead that doesn't provide any real payout.

Now there is no reason to throw the baby out with the bath water. There were ideas in [BDD][] that we found to be extremely valuable. For example the concept of applying Outside-In Development as a way of making sure that what is being developed throughout the application architecture is actually necessary. We found this practice aids immensely with not only making sure the right software is built but also with the process of identifying architectural boundaries and doing application architecture.

Therefore our current position on testing is a hybrid of the various methodologies we have used over the years. The principles we follow are listed below.

* Outside-In
* Test First
* Small Tests
* Red, Green, Refactor
* State over Behavior
* Unit & Integration
* Setup in Test
* Full-Stack Integration within Reason

## Outside-In

As described above we value Outside-In Development from [BDD][] as a way of making sure that what is being developed throughout the application architecture is actually necessary. Additionaly we value it as a tool for aiding with architecting an application as well as identifying application architecture boundaries. Which are both extremely useful when writing software in a fashion supporting collaboration. You can see more on Outside-In and how it relates to some of our other processes in our [Journy to Small Pull Requests][] and [How we should be using Git][] posts.

## Test First

We also strongly abide by the concept of "Test First", the practice of writing a failing test against a nonexistent idealized API and then following the Red, Green, Refactor process to iterate the test & implementation to its final state.

The primary reason we value "Test First" so much is because the practice of thinking about what the ideal API would look like and using the first iteration of writing the test as a tool for this has huge positive impacts in terms of the application architecture and its maintainability over time.

Beyond that it also benefits in helping to make sure that the tests fully cover the implemented code. If instead you were to for example to write a function first and then go back and write the test after the fact. You would discover over time that it is very easy to miss cases and end up with a test suite that doesn't fully cover all the cases.

## Small Tests

As a general rule of thumb we stick to one conceptual assertion per test. What this means is that we effectively want to test and verify one thing. If we are testing say a function that returns an object that should have multiple attributes on it in a new state. Then we would have multiple assertions to verify those. But conceptually we are still verifying the resultant state of that one action.

There are cases, when doing higher level integration tests especially, where it may make sense to actually do multiple conceptual assertions. However, this is the exception not the rule.

Sticking to one conceptual assertion keeps our tests focused in such a manner that when one fails it is focused enough that we don't have to do a ton of debugging to identify what the problem is. Generally the failing test immedietly tells us exactly what the problem is. This of course does not hold true for higher level integration tests, especially the ones that have multiple conceptual assertions.

## Red, Green, Refactor

Red, Green, Refactor is really just a name for a process that occurs naturally when doing "Test First" [TDD][]. It simply communicates the enforcement of the sequence of the testing process.

1. write a failing test - a.k.a. Red
2. write code that makes the test pass - a.k.a. Green
3. clean up the the implementation that makes the test pass by refactoring it - a.k.a. Refactor

Over the years this naming has been overloaded by being utilized at a micro level to explain the iterative process of doing "Test First" [TDD][]. In this context is more along the lines of the following.

1. write a failing test (in initial state, not final state) - a.k.a. Red
2. write code that makes the test pass - a.k.a. Green
3. evolve the test to test more of what it should, leaving you with a now failing test - a.k.a. Refactor, Red
4. write code that makes the test pass - a.k.a. Green
5. ...
6. ...
7. evolve the test to test to its final state, leaving you with a now failing test - a.k.a. Refactor, Red
8. write code that makes the test pass - a.k.a. Green
9. clean up the the implementation that makes the test pass by refactoring it - a.k.a. Refactor

I don't think this overloaded adaption to this micro usecase is necessarily the cleanest mapping. But it is what a decent portion of the community has starting using it to refer two. So we might as well at least be clear about it.

As I stated before these are basically just the naturaly processes that occur when doing "Test First" [TDD][] so we naturally abide by them.

## State over Behavior

The concept of **State over Behavior** means that we ideally want to do **State** based testing whenever possible and only use **Behavior** testing when explicitly necessary.

We hold this stance because **State** based testing aids with application architecture design and is naturally long living as well as interface abiding. This means that once we have written a **State** based test it is useful when we go to refector the implementation of a function/method. **Behavior** tests are **not** useful and have to be completely reworked when refactoring implementation of a function/method.

This position does have implications. For example to support isolation (via stubs/test doubles) in our tests we need to modify how we write our code to make sure we are using Dependency Injection. This is an implication we are more than happy to take on as we have found that classic Dependency Injection has aided massively in our application architectures over the years.

## Unit & Integration

There are two main categories of tests **Unit** and **Integration**. We are proponents of both of them.

**Unit** tests are focused around validating a particular unit of code, generally a function/method. Therefore, they isolate away collaborator objects. An example might look something like the following.

```
stub_shopping_cart = ShoppingCartTestDouble()
add_item(item, stub_shopping_cart)
assert(shopping_cart.items.include(item))
```

In the above example we replaced the shopping cart by giving the `add_item` function the `stub_shopping_cart`. This isolates away any functionality that a real shopping cart whould have had and allows us to strictly test the functionality of just the `add_item` function.

**Integration** tests are focused around validation of many units of code working together in an integrated fashion. An example might look as follows:

```
data_store = DataStore()
real_shopping_cart = ShoppingCart(data_store)
add_item(item, real_shopping_cart)
assert(shopping_cart.items.include(item))
```

You can see in the above that it is using real objects rather than Test Doubles or Stub objects. The benefit of this is that it can be used to verify functionality of all those different units of code working together which is also important. Especially to make sure that integrated results are what we expect.

Therefore we write both **Unit** and **Integration** tests.

## Setup in Test

**Setup in Test** is the concept of making sure that the **setup** portion of each test is included directly in the test itself and not in some `before each` or `describe` block or some parenting test framework concept that provides initial state setup across multiple tests.

You might be thinking to yourself well this concept goes against the DRY principle which we follow in implementation code. Shouldn't we be following it in our tests as well? Technically it doesn't go againts the DRY principle overall. It just goes against DRYing test setup up using framework concepts like `before each` and `describe`.

The reason that **Setup in Test** exists as a concept is because extracting test setup into `before each` or `describe` blocks or other test framework constructs have massive negative implications when you are coming back to a test later on. They make it very difficult to understand what the exact test setup is for a particular test. You often have to go hunt to try and figure out what test setups are done earlier in the suite and even then there are likely test setup steps that aren't needed in your particular test but exist earlier in the suite.

To get the best of both worlds, **clarity of test setup** and **DRYing up test setup** we follow the common strategy of extracting test setup into utility functions in the test suite. This enables us to have clear, concise, well named utility functions that we can use in each of the tests that clearly communicate the setup. Even in cases where the naming don't clearly communicate the setup, it is easy enough for us to dive into any setup functions and see what is going. This is superior as there is a formal relationship and we don't have to guess if setup is used or not.

## Full-Stack Integration within Reason

Full-Stack integration testing is the same concept as **integration** testing from above. The difference is that it is generally at the highest level of the application and integrates through all the various layers of the application. We are big believers in this for the same reason we are **integration** testing.

However, when you are doing Full-Stack integration testing you often have to deal with things like a database or 3rd party APIs, etc. This is why we have the caveat of "within Reason". Doing true Full-Stack integration testing is very difficult and not practical/pragmatic in most cases. It often makes more sense to stub out 3rd party integrations and do some sort of datbase resetting/transaction management, or maybe an in memory database substitution. In some cases maybe it even makes sense to start a layer lower in the architecture.

The point is that given the impracticality of doing true Full-Stack Integration Testing we instead figure out for each of the different projects we work on what we can do "within reason" to get as close to Full-Stack Integration as possible.

[Uptech Studio]: https://uptechstudio.com
[BDD]: https://en.wikipedia.org/wiki/Behavior-driven_development
[TDD]: https://en.wikipedia.org/wiki/Test-driven_development
[Cucumber]: https://cucumber.io
[Gherkin]: https://cucumber.io/docs/gherkin/
[Journy to Small Pull Requests]: /blog/journey-to-small-pull-requests/
[How we should be using Git]: /blog/how-we-should-be-using-git/
