+++
title = "Infrastructure"
description = "Our preferred infrastructure tech stacks"
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

Infrastructure is **not** our primary focus. We consider ourselves to be **infrastructure light**. Meaning we do infrastructure as a means to an end and it isn't something we specialize in beyond those needs. We have no interest in contracting to just do infrastructure work for anyone.

If a client has an existing team with DevOps people that manage the infrastructure we will happily work with them. If the client doesn't have such people then we put on our **infrastructure** hats and evaluate the needs and decide between two approaches.

- [Heroku][] - a Cloud Application Hosting platform
- [AWS][] - a Cloud infrastructure provider

If [Heroku][] is fitting for the project then we will default to that. If the project requires more control over the infrastructure for whatever reason then we use [AWS][] managed via [Terraform][] and [Kubernetes][].

[Heroku]: https://www.heroku.com
[AWS]: https://aws.amazon.com
[Terraform]: https://www.terraform.io
[Kubernetes]: https://kubernetes.io
