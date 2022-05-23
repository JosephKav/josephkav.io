---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Argus"
subtitle: ""
summary: "A lightweight monitor of software releases that I built in Go/React"
authors:
  - admin
tags: []
categories: []
date: 2022-05-03T22:37:00Z
lastmod: 2022-05-03T22:37:00Z
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects:
  - argus-app
  - argus-website
---

Argus, which I named after the many-eyed giant in Greek mythology, is a lightweight monitor of software releases. I developed this to keep up-to-date with releases and easily view the changelogs. A big factor in starting this project was a desire to do more [Go](https://go.dev). This started as a simple CLI-only app named [Release-Notifier](https://github.com/JosephKav/Release-Notifier) and eventually expanded to having a web UI to visually track what it's doing as well as how up-to-date my deployed softwares are.


### Docs

Documentation for Argus can be found [here](https://release-argus.io) ([GitHub](https://github.com/release-argus/Website)).


### Demo

A live demo of Argus can be found [here](https://release-argus.io/demo) ([GitHub](https://github.com/release-argus/Argus)).


### Positives

One nice thing I encountered whilst in the early stages of this project was the release of Go 1.18+. This release introduced [Generics](https://go.dev/doc/tutorial/generics) which greatly reduced the amount of utils-style code. So instead of needing say a `GetFirstNonNilStringPtr`, `GetFirstNonNilUIntPtr`, `GetFirstNonNilIntPtr`, and a `GetFirstNonNilBoolPtr` function, all I'd need is `GetFirstNonNilPtr` that takes Generics and so can handle pretty much all pointer types. This helped cut down on a lot of duplicate code!

It was also appreciated that once I had the web UI working, someone (larsl-net) found the project and started using it. He helped by submitting a few bug/feature requests as well as helping with some of the documentation. It felt really good that someone else had found my project and cared enough to help try and improve it. This helped me better understand the appeal of working on FOSS. Thanks larsl-net :)
