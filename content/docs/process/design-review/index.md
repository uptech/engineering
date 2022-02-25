+++
title = "Design Review"
description = "Overview of Design Reviews, what they are, when they are done, etc."
date = 2021-05-01T18:20:00+00:00
updated = 2021-05-01T18:20:00+00:00
draft = false
weight = 25
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

This document describes how and when a software design review should take place.

## What is it?

A design review is an opportunity to get feedback on your application design prior to spending a
lot of time working on the implementation.

Even if you love to do emergent design using TDD, when projects are large and complex, some
upfront design is necessary. It's hard to identify all the components that exist and what their
interactions are with existing systems without taking some time to think through it. It's also
really helpful so you can break down a larger project into smaller components.

As a general rule of thumb if a project is going to take **more than a week** to develop, then it
probably should have a design review. Smaller projects will have real quick meetings, whereas
larger projects will probably take some serious brain power.

## When does it take place?

A design review should happen after the requirements have been documented and delivered to the
implementation team, but before any code has been written.

Ideally the product owner will have a kickoff meeting with the team to go over this new feature
and how it should work. After that, the engineers can review the requirements in detail and start
coming up with an implementation plan.

Within a few days of the kickoff meeting, the engineers should have a design review meeting to go
over the plan.

## Who should be part of it?

The technical lead of the project is responsible for scheduling time to plan and document the
proposed design. They are also responsible for scheduling the review meeting and making sure they
have proper representation at the meeting.

A design review should have the following people:
* Technical Lead
* Backend Representative
* Front-end Representative
* Infrastructure Representative
* QA Representative (Ideally the person that will be testing this feature)

Including members from other teams helps to address any concerns they may have earlier in the
process as opposed to later when it's more expensive to change.

## How does the meeting run?

During the meeting, the technical lead on the project should review the feature being implemented
at a high level so everyone in the room understands what the business objectives are. After that,
they should walk through a document detailing the implementation plan.

The document should include the following:
* A high level description
* A diagram of the different systems/components
* A sequence diagram indicating interactions between components/systems
* Interface specifications (REST API, Message Formats, etc.)
* Callout for new Infrastructure components
* Rollout plan - call out any sequencing required

## Why should we do this?

Because we want to build awesome products with the highest standards in mind and avoid making
desicions that may bite us in the future.
