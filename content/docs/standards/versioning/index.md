+++
title = "Versioning"
description = "Our Library & API Versioning Standards"
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

## Versioning Libraries

We use [Semantic Versioning](https://semver.org) for our standard of choice for versioning libraries.

## Versioning APIs

There are few options.

* URL based versioning, e.g. `/api/v1/route`
* HTTP Header based versioning
	* use [Media types](https://docs.github.com/en/rest/overview/media-types), e.g. `application/vnd.github[.version].param[+json]` or `application/vnd.github.v3+json`
* HTTP Custom Headers versioning (any header beginning with `X-`), e.g. `X-GitHub-Media-Type`

We prefer and will use **HTTP Header based versioning with Media Types** as it is the most robust solution that generally also has support built into libraries or frameworks.

The **HTTP Custom Headers versioning** is technically more robus as there are no standards around it. However it then requires more custom work to do be done for clients interacting with it.

The **URL based versioning** is **NOT** robust because it requires either duplicating code, or implementing some sort proxying, etc. to deal with situations where you want to create another version of all the routes. It also makes it harder to mix them as well in terms of code management.
