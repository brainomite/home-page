---
layout: project
title: AHEM Monster
date: 12 Sep 20
image:
  path: /assets/img/projects/ahem.png
srcset:
  1840w: /assets/img/projects/ahem.png
  920w: /assets/img/projects/ahem@0,5x.png
  460w: /assets/img/projects/ahem@0,25x.png
caption: Anonymous temporary ad hoc emails
description: >
  This is an ad-hoc temporary email service. Built using Vue on the front end
  with Node.JS express server on the back end. It uses socket.io for new email
  alerts. The database is built in MongoDB
links:
  - title: Live
    url: https://ahem.monster
  - title: GitHub Source Code
    url: https://github.com/brainomite/Ad-Hoc-Email-Server/tree/production
featured: false
---

I took an open source project ([ahem.email](https://www.ahem.email/), please
note the site doesn't always function) and made it my own. Since this project
needs to receive communication via SMTP on port 25, I couldn't host it on
Heroku. This is hosted on an AWS EC2 Ubuntu virtual private server.

I made it my own project by changing the following:

1. I changed the styling and removed some content to suit my own taste.
2. I added a list of over 1,000 email mailbox usernames that are forbidden (for example admin, or administrator)
    1. The SMTP server will send an NDR (non-delivery receipt) if it receives a forbidden email mailbox username
    2. When a user tries to open a forbidden mailbox, they will get a tooltip warning and will be unable to proceed.
3. Submit won't work if the address is empty or contains just white space. Also if it contains a '@' it won't proceed to the mailbox. Tooltips will appear showing the reason why the user cannot proceed
4. Generate will now generate silly email addresses using this NPM package, [sillyname](https://www.npmjs.com/package/sillyname). It generates silly random mailbox usernames, vs the original pure-random mailbox
5. On both the home page and mailbox view, there is now a click-to-copy the email address feature.
6. The SMTP server now saves an email into the database using mailbox@domain.com, not just mailbox as originally designed
7. The home page when clicked, submit will go to the email list using the full email address. not just the mailbox alone as originally designed.
8. The home page now has a drop down showing all the possible domains configured with the server.
9. There is no longer a need to manually refresh the email list to check for new emails. Using socket.io, the server now alerts the client if a new email has arrived and will automatically update the list of emails.
