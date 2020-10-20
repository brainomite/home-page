---
layout: project
title: Begone Email (in progress)
date: 10 Oct 20
image:
  path: /assets/img/projects/begone-email happy.png
caption: Anonymous temporary ad hoc emails
description: >
  This is an ad-hoc temporary email service.
links:
  # - title: Live
  #   url: https://ahem.monster
  - title: GitHub Source Code
    url: https://github.com/brainomite/begone-email
featured: false
---

This is a work-in-progress.
Built using React on the front end and with Node.Js express server on the back
end. The database is built in MongoDB using Mongoose as an ORM

## Learning Goals for this project

1. Mongodb
2. Mongoose
3. Docker
4. Add patreon Level capabilities using their API - Post MVP

## MVP

* ~~Receive emails~~
* Prevent open relay by limiting emails received to specified domains
* Deployed to a virtual private server.
* Basic home/landing page
* Users can specify a mailbox to check
* Users can specify the domain of the mailbox
* Emails can be viewed for a specified mailbox-domain combo
* Read indicator
* Email can be viewed
* ~~Emails deleted at a specified interval~~
* ~~Can specify after how long an email is stale and ready for cleanup~~
* Create a production docker
* Create a development docker

