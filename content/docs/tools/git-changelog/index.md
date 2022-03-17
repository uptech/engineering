+++
title = "Git Changelog"
description = "An overview of Git Changelog and what we use it for"
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

Over all our years of experience we have found that maintaining a CHANGELOG file is extremely valuable and definitely worth the effort. However, this was always a point of friction as doing so within a Git repository would result in conflicts with every change simply because of the changes to the CHANGELOG file. [Git Changelog][] is an open source Git extension that we built to specifically address this issue and it has become our standard way of managing CHANGELOG files in projects.

[Git Changelog]: https://github.com/uptech/git-cl 
