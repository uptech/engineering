+++
title = "The DSL Phenomenon"
description = "An exploration of the DSL Phenomenon and how we might get there."
date = 2021-02-22T01:32:01-08:00
updated = 2021-02-22T01:32:01-08:00
draft = true
template = "blog/page.html"
+++

# The DSL Phenomenon

There exists this phenomenon in software development where we can tackle solving a problem and write a specific solution and it takes lets say 1,000 lines of code. However, we can also solve this same problem by building a DSL (Domain Specific Language) to solve that particular class/type of problem. Writing this DSL takes significantly less code than the solution itself. For sake of discussion lets say 250 lines of code. Then with that DSL you can solve the specific problem in say 10-20 lines of code. Overall this saves you 730 lines of code.

This is a phenomenon that has existed for ages in software. We can see examples of it at various levels. Arguably assembly languages vs machine code are an example of this. Higher level programming languages in relation to Assembly languages are an example of this. All the way up to things like web frameworks or even languages for more specific domains.

## Levels of DSLs/Frameworks

It is important to understand that these DSLs/Framworks can exist at various different levels. We can see this in the Framework/DSL -> Higher Level Programming Language -> Assembly Language -> Machine Code. And if we looked closer at things we would realize that coding is literally layer upon layer of domains. Some of which have been formalized into DSLs and other which haven’t.

So in theory it could be DSLs all the way down the stack.

## Values of these DSL/Frameworks

We know the values these DSL/Frameworks provide because we see at least the solution side of them on a regular basis every day that we use them.

- reduce the amount of code that has to be written for a solution
	- some at the framework/dsl level
	- some at the solution level
- we end up with better factored code both at the framework/dsl level as well as at the solution level making the code in general easier to reason about
- all this also makes the code easier to maintain
- ability to reuse your DSL to solve similar problems in the future more quickly

## General Recommendation Contrast

Given all this value we have to ask ourselves, “Why is the common recomendation to people that they implement a solution 3 or 4 times before they ever start worrying or thinking about building a framework or DSL to solve the problem?”.  This is clearly counter to the values that we see in code reduction, ease of reasoning, and long term maintenance gains. There must be a reason!

I believe the primary reason that people give this general recommendation is because people find it hard to figure out what exactly a good DSL/Framework would be fore a speciifc domain. Not to mention the fact that people who have tried to build a DSL/Framework often invest huge amounts of time trying to figure out an ideal DSL and then run into issues when trying to actually apply their DSL to the specific solution they needed it for.

Interestingly these problems sound like the same problems that we run into when doing **Inside Out** or **Middle Out** deveolpment. So maybe we could solve this by simply following the **Outside-In** development process?

## So we are good right?

Great, so if we just follow **Outside-In** we will end up with a well factored DSL/Framework not only for our solution layer but also all the layers in between, right?

Sadly that doesn’t seem to be true. However, that shouldn’t detract us from using **Outside-In** because I do believe it definitely aids in preventing us from making the classic **Inside-Out** or **Middle-Out** mistakes that people generally make and it is crucial we don’t make those.

## The Other Part(s)

So if we follow **Outside-In** but it isn’t enough, what are the other characteristics that would help unlock/evolve/expose a DSL at these various layers. In my experienece the two key characteristics that I think have aided in doing this for me are making sure things **focused & granular** while also making them **composable**.

Being **focused & granular** means that the building blocks used in developing a solution need to be hyper focused and broken down into small granualar components that do one thing very clearly & well. In addition these building blocks need to be compossable so that they can easily be put together to solve more complex problems.

## The Fallout

Turns out when you follow these principles a DSLs & Framwerok at the various domains involved in your solution start to expose them selves. These exposed focused, granular, composable building blocks are in their nature a DSL. This may not be the most ideal DSL you can fathem. But, it is an unbeleivable starting point for that DSL and it enables you to easily identify the components and their strattifications which then facilitate you easily factoring them into even more ideal DSLs.

## In Practice?

This sounds great and all in theory. However in practice what do we have to help us actually accomplish this?

In my experience Functional Programming has been the one and only paradigmn that has actually resulted in this DSL exposure for me. It is one of the primary reason I feel that Functional Programming is so valuable.

## Example

Maybe the GitDiff Parser

