+++
title = "Codebase Organization"
description = "A breakdown of how we think about Codebase Organization"
date = 2021-05-01T18:20:00+00:00
updated = 2021-05-01T18:20:00+00:00
draft = false
weight = 400
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

## TLDR

The following is the TLDR of what we do in terms of Codebase Organization here at UpTech Works.

* organize code according to architectue **not** types of architecture element types
* if in framework that does organize by architecture elment types
	* follow that convention up until you reach natural boundary (e.g. `lib` folder)
	* then switch to organizing by architecture

## Major Strategies

There are basically two major strategies people use to organize their codebases.

* around types of architecture components (e.g. `models`, `views`, `controllers`)
* around actual application architecture

### Around Types of Architecture Components

Frameworks like Rails and NestJS are examples that follow this strategy where they have a folder for `models`, `controllers`, etc.

Organizing your codebase around these types of architecture components has the following advantages.

* you don't have to think very hard when you are trying to figure out where to put something. Controllers go in the `controllers` folder, `models` in the models folder, etc.

This benefit however is not always true because there is generally a boundary where this falls apart as the framework doesn't provide direction. The `lib` folder is often one of these boundaries. The framework provides guidence in terms of organization right up until that point and then all of the sudden `lib` is just a dumpster full of stuff.

Organizing your codebase around thes types of architecture components also doesn't help people understand anything from the codebase other than that there are things called `controllers`, `models`, and `views`. But it doesn't give you any knowledge in terms of how certain controllers, models, or views relate to one another. You have no idea from the organization of the codebase how to find a particular part of the application.

### Around Application Architecture

An alternative approach that people take is to instead organize their codebase around the actual application architecture.

When you have put that thought in, people coming into the code base can **quickly and easily understand the application architecture**. They can also **quickly navigate** to appropriate areas of the code they want to focus on.

This strategy also helps with creating **logical commits**, which we are big fans of, and **reducing conflicts** in source control systems.

People often perceive the fact that you have to think about where to put folders and files in your codebase as a negative thing.

We believe this effort is one of the more valuable activities actually. It is effectively a forcing function driving you to make sure you have a sound application architecture and understand where the piece you are working on should fit within it.

Using techniques like modularization and concepts like the The Onion architecture can help you figure out and evolve your application archicture and your codebase organization.

## But with Frameworks

We know from experience fighting against a frameworks inherent design is generally not a great strategy. So how do we organize our codebase around the Application Architecture when we working with a framework like Rails or NestJS?

Simple, we follow the convention of the framework up until we reach that natural boundary where it stops providing direction, e.g. the `lib` folder. Everything inside that `lib` folder we would organize around application architecture and **not** by category of architecture component.

This at least gives developers the knowledge around that area of the code. Ideally it would be great if we could get those benefits at the framework level of things as well. However, it just isn't worth fighting against the framework to make it happen.
