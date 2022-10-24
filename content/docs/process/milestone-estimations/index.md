+++
title = "Milestone Estimations"
description = "Overview of our Milestone Estimation process"
date = 2022-10-24T18:20:00+00:00
updated = 2021-10-24T18:20:00+00:00
draft = false
weight = 50
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

Despite our dislike for estimates due to their inaccurate nature we still do
them in some form because the reality of the situation is that we need to know
relatively early in the process if we are going to be able to hit a timeline or
not. Obviously we can't know this 100% because as we have stated estimates are
very inaccurate. However, they can gives us a pulse to know if we
think we are significantly going to miss a timeline. This is crucial as that
pulse is what instigates pruning of non-crucial features and setting
expectations appropriately with the client.

To facilitate this we of course start with the initial estimates from the
Discovery & Definition phase. However, things have likely changed since that
time and the developers haven't thought about the complexities of each of those
milestones.

To facilitate this when a Journey is reviewed by product with the associated
dev team. The dev team goes back through the milestones of that Journey and
puts together estimates based on the following thinking.

### Origination of Thinking

Historically we have focused on the concepts of **Complexity** and
**Effort** while putting together estimates and it was only our
Milestone Planning that thought about unknowns and risk.

However, relatively recently Theo dropped a video where he broke down what he
has thought of for a system for estimates. It is very much in alignment with
our current approach as it takes **Complexity** and **Effort** into
consideration. However, beyond that it takes into account **Risk** and goes
through the reasons for estimating in a more analytical fashion. Therefore, we
have effectively adopted his approach with some small tweaks.

We also differ from his approach in that after thinking through these
characteristics we then estimate how long things will take (in developer days)
using these characteristics guidence. This is critical for us to understand how
we are making progress in terms of our estimates so that we can make
adjustments and manage client expectations appropriately.

[Theo - Why Are Estimations So Hard??? PLANNING TASKS AS AN ENGINEER](https://www.youtube.com/watch?v=XhUAIVJ62dQ)

## Why we estimate?

We do estimates at the epic level for the following reasons.

- Identify who should do it
- Identify when it should be done
- Identify low performers
- Bring risk to the forefront so that it can be addressed/mitigated
- Prioritize based one ^
- Identify how long it will take (this will be wrong, but hopefully taking the following system into account will help make it less wrong)

## The Breakdown

Our estimation system is broken down into 3 categories, Complexity,
Effort, and Risk.

- **Complexity** (1-10) - How "hard" is this thing to do?
- **Effort** (1-10) - How much work does it take?
- **Risk** (1-10) - What are the chances that Complexity or Effort is off?

## Getting Started

When first getting a team started thinking in this way we have found that it is
useful to often go down to the story level and document the **Complexity**,
**Effort**, and **Risk** as it ends up being a forcing function to think about
the complexities and risk. Then go back up to the milestone and produce the
milestones **Complexity**, **Effort**, and **Risk**. Then you can look at the
stories overall and validate that your estmiate for the milestone makes sense.

Once the devs start to get the feeling for thinking about **Complexity**,
**Effort**, and **Risk** this process goes really quickly.

## Breaking out Research Tasks

As we go through this process we are often running into **Risks** and most of
the time you need to do some further research to understand and mitigate the
risk. This is the perfect time to break those tasks out.

You can also see by doing so how breaking the research task out helps you to
think about and adjust the characteristics for the story and it's associated
research task.

## Example

The following are a couple examples from the video referenced above with the addition of the developer days estimates.

Display voice levels in pre-call view

- **Complexity**: 4/10
- **Effort**: 2/10
- **Risk**: 6/10
- **Estimate**: 4 dev days

To reduce the risk of this task we could create a research task to figure out
one of the bigger pieces of risk. For example.

Identify how to get voice levels from 3p SDK

- **Complexity**: 2/10
- **Effort**: 2/10
- **Risk**: 4/10
- **Estimate**: 1 dev day

