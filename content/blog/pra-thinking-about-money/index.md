+++
title = "Pra: Thinking about Money"
date = 2018-12-19T09:32:01-08:00
updated = 2018-12-19T09:32:01-08:00
draft = false
template = "blog/page.html"

[extra]
authors = ["drewdeponte"]
+++

While I am waiting to collect information from the [survey](/blog/pull-request-notifications-an-anti-pattern/), I started thinking about how I could make money with Pra. I thought about it before at a high level. But now I have started thinking about it in more depth.

## Expectations

It is important to understand that my expectations aren't that this product be some sort of unicorn. I share the mindset of [Paul Jarvis](https://pjrvs.com) — I want to build a successful, sustainable business rather than shoot for explosive growth — and we largely operate our current boutique consulting/contracting business that way. Paul just launched a book titled [Company of One](https://ofone.co) where he talks about this mindset. If I boil it down, my expectations for Pra are to be something that:
- actually helps people
- I can be proud of
- isn't overly complex or requires massive amount of effort to develop
- I can sell and make a relatively small amount of money from
- I can use to provide content to start building an audience

## How Do I Make Money?

After talking in depth about this with my co-workers, Ryan and Adam, it seems like I want to have a free tier that provides "real" value. Ideally I want people to find enough value in the free version that they use it over GitHub (or any of the other providers) directly and are willing to tell other people about it.

Then, to make money, I need to find compelling additional features that at least some people would be willing to pay for.

The natural organizational entities are _accounts_, _organizations_, and _repositories_.

### Stances

There are two main stances I have thought of related to this product that I could take:
- **Core Value Paywalled.** - In this stance, I would provide a free version of the app that wouldn’t do any aggregation. For this to work, I think it would have to be an app that works similar to GitHub, where you get access to the repos belonging to a single account and the ability to click on one and see the pull request for that specific repository. I believe it would also need to have some additional (free) features that make it more useful than just using Github itself. Then, I would have a paywall gating access to the aggregation feature. I would then have to provide some sort of trial experience to let the user experience the value of aggregation. The concern with this stance is that it would reduce the number of people willing to try the product out because they are hit with a paywall/trial right off the bat.
- **Core Value Free** - In the _core value free_ stance, I would provide the aggregation for free within a single account. Then the stuff behind the paywall would be _N_ accounts, as well as the set of tertiary features that make the overall experience better. The high-level thinking about this strategy is that if it is valuable enough, I may be able to get a larger number of people using the free version (since they get the core value for free). I would then have a channel through paywalled tertiary features that I would communicate on a personal level about who I am, what our mission is, what they get, and talk about it in terms of supporting our mission. The thinking is I could get a much wider net of free users to build our following for communicating other maker tools, etc. in the future, and some subset of people will pay for a Pro version. The concern with this approach is that if I provide the core value for free, there's less additional value (that people would pay for) in the Pro version.

My current stance is that I am not 100% sure which route to take. My instinct is that I should take the _Core Value Free_ stance as it facilitates growing a potentially larger following for other products in the future, and I can envision how to make it happen. Also, in the _Core Value Paywalled_ stance I would potentially have to build out a larger more complicated version of the app prior to the paywall being a thing, as it wouldn’t make much sense for people to use it if it was a worse tool than what GitHub already provides.

The above leads us to the following questions:

#### Do I charge by the repository and limit the free version to 1 repo, or maybe some fixed number of repositories like 3?

After hashing it out I think the answer to this question is **no**. I don’t think it provides/communicates/shows the value of aggregation when it is limited to simply 3 repositories.

#### Do I charge by the organization?

Well, I could. However, the largest use case of people are probably people that have one primary Organization (the org of the company they work for), and their personal organization. The question then becomes is there enough value of aggregation seen for them to then purchase a subscription just to get their personal org added in, or would they just deal with the annoyance? After talking about it with Adam and Ryan I think most people would just deal with the annoyance. So, the answer is no.

I also need to keep in mind that I want the largest possible group of people using and finding the free version valuable so that I can convince them to pay for even more great value.

#### Do I charge for multiple accounts?

Initially the thinking around this was that I charge for multiple accounts, as it is probably a niche subset of the market (consultants/contractors). However, I know personal developers who have their personal repos in BitBucket and their company repos in GitHub. So, I am not sure where I net on this at the moment. It still needs some consideration.

### So Where am I?

If I don’t charge based on core organizing principles what do I charge for? Right now the thinking is that I would charge for features. For instance, provide some of the Quick Filters for free and some of them would require you to be a subscriber. I could also make the search functionality, keyboard shortcuts, enriched PRs, etc. paid features.

### Principles of Purchase

One thing I don’t like is the complexity around purchases and nickel and diming people. So I think it should basically be two tiers:
- Free – Roughly feature parity with seeing PRs in GitHub, with the added value of the aggregated views and some other nice to have features
- Pro (probably a monthly subscription) - All The Features. I think that getting people to pay is as much about access to the features as it is finding supporters around our mission of helping makers, and who we are. So they aren’t just buying _a thing,_ they are supporting a small boutique group of real people they can identify with.

One thing I have talked about as a possible 3rd tier would be Enterprise, which would potentially get you integrations with GitHub Enterprise, etc. The thinking around this is some individuals might pay for it because they have become dependent on Pra and love it. But I think the real money with that is in group licensing to companies that use Enterprise GitHub, etc.

## Conclusion

Although I don't have all the answers, my instinct is still to take the **Core Value Free** stance. I do believe thinking through some of this prior to getting the results from the survey back is a good thing. It helps me look at the data I get back in ways I may not have looked at it originally. Hopefully, the results of the survey will help clarify answers to some of these questions.

