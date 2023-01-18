+++
title = "Feature Flag Resources"
description = "A brief listing of useful feature flag resources"
date = 2023-01-18T20:33:00+00:00
updated = 2023-01-18T20:33:00+00:00
draft = false
weight = 420
sort_by = "weight"
template = "docs/page.html"

[extra]
toc = true
top = false
+++

* "Growthbook" - [link](https://docs.growthbook.io/)

### Canary Testing with Growthbook

Our [Flutter Growthbook Wrapper](https://github.com/uptech/uptech-growthbook-sdk-flutter) makes it easy to do canary testing and version baselining with Growthbook

In the Growthbook GUI:

* Add wanted attributes (for the canary testing example, use id and set it as identifier) [link](https://docs.growthbook.io/app/features#targeting-attributes)

* Add an override rule:
`if id is in the list` (add desired tester ids)
[link](https://docs.growthbook.io/app/features#override-rules)

**Using the Uptech Growthbook Wrapper to set attributes**

In Flutter: [link](https://github.com/uptech/uptech-growthbook-sdk-flutter#set-attributes)

In Typescript: [link](https://github.com/uptech/uptech-growthbook-sdk-typescript#set-attributes)