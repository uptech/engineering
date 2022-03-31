+++
title = "Project Planning"
description = "Description of our Project Planning"
date = 2021-05-01T18:20:00+00:00
updated = 2021-05-01T18:20:00+00:00
draft = false
weight = 20
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

The **Project Planning** phase is where take the knowledge we gained around the project scope and start planning out the high level technical details around the work. The goal of this phase is to do an initial relatively high level of planning to start to breaking down the technical problem space and identify unknowns, concerns, and blockers. This is generally achieved with the following artifacts of this process which are the responsibility of the Tech Lead to produce.

* Systems Architecture
* Project Detailed Spec
* Defining Unknowns

## Systems Architecture

The first artifact for this phase is a Systems Level Architecture diagram that the Tech Lead is responsible for putting together. The goal of this is to simply start the conversation and discussion around all the entities in play at a system level and how they are going to interact with eachother. Over time this continues to be updated and used as a conversation piece to discuss and review systems level interactions.

## Project Detailed Spec

The Project Detailed Spec is a document that the Tech Lead is again responsible for. This document defines the project scope and goals for review as well as the Systems Architecture to provide context. Beyond that it is a vehicle to explore the next tier down of technical problem solving. This could be how the various components would be interacting with each other. Is it done via GraphQL, REST, etc? What are some rough ideas of what the key functional data is needed to be passed between these system components. These types of questions and thinking. It isn't to formalize 100% exactly what the payloads will look like. It is just to get an idea and an understanding to gain more confidence.

The process of the Tech Lead walking through, planning this out and documenting it helps them start to think through all of the components and how they will interact to accomplish the defined goals. This also helps identify up front major unknowns, concerns, and blockers very early on in the project so that those things can be focused on from the get go.

## Project Design Review

Once the Project Detailed Spec is produced the Tech Lead then schedules a **Project Design Review** internal to Uptech Studio. This meeting enables a round of review and feedback on the architecture and design that is documented in the Project Detailed Spec. This generally should have a representative from Product, Design, and Engineering. The people at this point aren't from your Squad.  As a Tech Lead you can choose to do Squad internal rounds of feedback with the engineers prior to this meeting.

## Project Kick-off

Once the Tech Lead has held the Project Design Review, gotten feedback, and integrated that feedback into their Systems Level Architecture diagram and Project Detailed Spec they then schedule a **Project Kick-off** meeting with their Squad. This involves all members of the Squad and provides in opportunity to go over everything covered in the Detailed Spec from the project Goals down to how the various components will be interacting and with what data.

The purpose of this meeting is for all the members to get comfortable with all of this and voice any concerns, suggestions, etc.

## Upfront Planning?

I thought Uptech Studio did Agile. We do! Nothing in Agile says that you shouldn't do up front planning. Agile is all about iterating and getting feedback throughout the process and adjusting to that feedback. But nothing in it says you shouldn't think about how you would build or implement something. Our experience over the years has informed our opinion that doing a small amount of up front planning like this is often a game changer in terms of the success of a project.
