+++
title = "Milestone Planning"
description = "Overview of our Milestone Planning process"
date = 2021-05-01T18:20:00+00:00
updated = 2021-05-01T18:20:00+00:00
draft = false
weight = 40
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

The **Milestone Planning** process is basically the same process that we do in terms of **Project Planning**. The main difference is that we are planning within the scope of a defined **Milestone**. This process is again driven by the Tech Lead. It can be thought of in the following sections.

## Scoped Systems Architecture Diagram

When doing Milestone Planning you are scoping your thought process and in turn your created artifacts to a particular milestone. Therefore, you are creating a System Architecture Diagram that only focuses on the needs for this particular milestone. This is useful again to provide context but also useful to remove the noise of everything outside of the scope of this Milestone.

## Application Architecture

The Application Architecture is where you start to visually layout how your team is going to build an application that will meet the needs of the goals. This again is about components, how data flows through them, and what components handle different types of processing.

## Scoped Protocol Specifications

These protocol specifications again aren't inteded to be 100% accurate in terms of every piece of data. It is intended to be a tool to make the Tech Lead think through the core data that needs to be exchanged via this different channels and what those protocols would look like.

## Infrastructure Diagram (depends on need)

Infrastructucture Diagrams aren't always necessary. Basically if there is something complex and abnormal necessary in the infrastructure then we should create an infrastructure diagram. The other use case might be that a client is explicitly requesting or pushing for an Infrastructure Diagram.

## Sequence Diagram (depends on need)

A Sequence Diagram is again dependent. If you are in a situation where you need to clearly define and communicate about complex interactions between system level or software level components a Sequence Diagram is the tool for you.

## Flow Chart (depends on need)

Flow Charts again aren't required but are super useful when you have some complex logic that needs to be planned out and thought out.

## Detailed Design Spec

The Tech Lead then puts together a Detailed Design Spec similar to the concept and structure of the Project Design Spec from before except scoped down to the concerns of this one milestone.

## Unknowns

Part of this process is identifying unknowns. We use these unknowns to guide us by effectively being blockers that need to be figured out to complete this milestone. We need to try and answer these unknowns as much as possible prior to the design review. However, some unknowns might be dependent on getting feedback from 3rd party vendors, etc. We should start those conversations with product and the 3rd party vendor ASAP, prior to the design review.

This list of unknowns should be a working list of blockers that should get formalized as tickets and prioritized generally to be figured out ASAP. There are sometimes cases where you might want to not prioritize resolving one of these unknowns until later. This should be vetted with the project's principal engineer.

## Design Review

Once completed the Tech Lead schedules a design review (same process as before) except this time it happens within the Squad. If the Squad feels they need external imput from people outside the Squad they are welcome to invite other people.

## Kick-off Meeting

Once the Design Review process is complete. The Tech Lead schedules a kick-off meeting for the Squad. This has the same purpose as before. It is to get everyone comfortable with everything and provide a venue for people to raise concerns, ask questions, etc.

## Task Breakout

One thing that different Squads tend to do differently is task breakout. Some Squads have an extra step here after the kick-off meeting where they schedule a meeting with Squad Product representative and the Engineers on the Squad and go through the tickets in the Milestone and breakout sub-tasks for each of the stories.

Other Squads don't do this meeting and instead leave the task breakout to the Engineer that grabs the story. We don't have strong opinions on this. It is really whatever the Squad is comfortable with. The key is that our Squads are constantly self evaluating so if there is a problem in this area they can quickly adjust.
