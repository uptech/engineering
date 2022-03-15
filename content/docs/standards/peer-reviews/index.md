+++
title = "Peer Reviews"
description = "Review of how and why we do peer reviews"
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

The intent of this document is to provide a reference document for the Peer
Review Process that we follow.

## Goals

The following are the main intentions of this process.

- improve code maintainability
- reduce defects in production
- developer growth

## Participants

All developers, no matter skill level, should participate in the peer
review process. If only a subset of the team does the peer reviews, this bogs
down the more senior people with a disproportionate amount of work. The senior
people usually are already spending time helping others, so we don't want to put
even more work on them. Additionally junior people can benefit from looking at
senior developer's work. They can see what the code should be like and learn
tips/tricks.

## Mindset*

Please be open to suggestions from your peer reviewers and don't take things
personally. On the flip side, please be aware that someone has put in a lot of
effort into their code and sometimes there can be multiple viewpoints. In order
to have a better collaborative experience for all people involved and still
achieve the stated goals, please consider the following suggestions:

- **Ask questions:** How does this method work? If this requirement changes,
  what else would have to change? How could we make this more maintainable?
- **Compliment / reinforce good practices:** One of the most important parts of
  the code review is to reward developers for growth and effort. Few things feel
  better than getting praise from a peer. Try to offer as many positive
  comments as possible.
- **Discuss directly for more detailed points:** On occasion, a recommended
  architectural change might be large enough that it’s easier to discuss it in
  person rather than in the comments. Similarly, if you are discussing a point
  and it goes back and forth, just pick it up in person and finish out the
  discussion.
- **Explain reasoning:** It’s best both to ask if there’s a better alternative
  and justify why you think it’s worth fixing. Sometimes it can feel like the
  changes suggested can seem nit-picky without context or explanation.
- **Make it about the code:** It’s easy to take notes from code reviews
  personally, especially if we take pride in our work. It’s best to make
  discussions about the code than about the developer. It lowers resistance and
  it’s not about the developer anyway, it’s about improving the quality of the
  code.
- **Suggest importance of fixes:** Not all suggestions made need to be acted
  upon. Clarifying if an item is important to fix before it can be considered
  done is useful both for the reviewer and the reviewee. It makes the results of
  a review clear and actionable.

## Process

### Developer Responsibilities

- Developer makes a patch
- Developer runs all automated test suites and makes sure it's green, or
  relies on CI server to do so and awaits green response.
- Developer manually tests the feature to see if acceptance, functional, ux,
  and ui criteria are met.
- Deveolper Requests Review of their patch
	- *Note:* at this point the pull request should meet the [Pull Request Standards](/docs/standards/pull-request)
    - If a patch is not complete, but you want feedback on it earlier in the
      process, you can Request Review of it, but indicate clearly in the title
      and description that the patch is not complete and not ready to be
      merged and what you're looking for feedback on.

### Reviewer Responsibilities

- Peer Reviewer assigns themselves to the pull request.
- Peer Reviewer checks out the code
- Peer Reviewer runs automated tests, if they fail, deny the pull request.
- Peer Reviewer does manual testing
    - Does it meet the acceptance criteria identified in the ticket?
    - Are there any performance issues?
- Peer Reviewer reads through the diff and comments on it. (see What to look
  for when reviewing below)
- Peer Reviewer approves/rejects the PR depending on their findings.

### More Developer Responsibilities

- Once approved, the Developer should then publish the patch to close the
  pull request and integrate the code. The peer reviewer should **NOT** merge
  the code as the developer knows best if it should be integrated into mainline
  or not.
- Developer should cleanup their remote branch associated with their PR.

## What to look for when reviewing*

### High Level

- **Don't Repeat Yourself (DRY):** If you see a block of code that is
  duplicated, wrap it in a function and re-use it.
- **Code left in a better state than found:** If you are changing an area of the
  code that’s messy, it’s tempting to add in a few lines and leave. Go one step
  further and leave the code nicer than it was found.

### Style

- **Commented code:** Good idea to remove any commented out lines.
- **Function length:** Rule of thumb is that a function should be somewhere
  around 1-10 lines in length. If you see a method above 10 lines, it’s likely
  needs to be broken down into smaller pieces
 
### Testing

- **Test coverage:** All new features and bug fixes should have tests. Are the
  tests thoughtful? Do they cover the failure conditions? Are they easy to read?
  How fragile are they? How big are the tests? Are they slow?
- **Number of Mocks:** Mocking is great. Mocking everything is not great. Use a
  rule of thumb where if there’s more than 3 mocks in a test, it should be
  revisited. Either the test is testing too broadly or the function is too
  large. Maybe it doesn’t need to be tested at a unit test level and would
  suffice as an integration test. Either way, it’s something to discuss.
- **Meets requirements:** Take a look at the requirements of the story, task, or
  bug which the work was filed against. If it doesn’t meet one of the criteria,
  it’s better to bounce it back before it goes to QA.

\* some items taken from: [Code Review BestPractices](http://kevinlondon.com/2015/05/05/code-review-best-practices.html)

