---
layout: project
title: Status for Promise
date: 1 Oct 20
image:
  path: /assets/img/projects/status-for-promise.jpg
srcset:
  1840w: /assets/img/projects/status-for-promise.jpg
  920w: /assets/img/projects/status-for-promise@0,5x.jpg
  460w: /assets/img/projects/status-for-promise@0,25x.jpg
caption: NPM package adding functionality to promises
description: >
  NPM package that will add statuses and values to promises or promise-like-objects
links:
  - title: Live
    url: https://www.npmjs.com/package/status-for-promise
  - title: GitHub Source Code
    url: https://github.com/brainomite/status-for-promise
featured: false
---

Sometimes we need to know whether or not a promise has resolved without waiting for the resolution.
This package will allow you to know the status without having to create some sort of external state tracker to keep track of whether a promise has resolved or not.
This package mutates a promise or a promise-like object adding status properties as well as the values or error when the promise is resolved.
