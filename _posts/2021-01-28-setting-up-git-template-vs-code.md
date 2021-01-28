---
layout: post
title: Configuring Git to Better our lives
description: >
  I give directions for setting up a git commit template & using VS Code for
  commit messages
sitemap: true
comments: true
---

Watch this video to see what we are about to setup

<iframe
  width="560"
  height="315"
  src="https://www.youtube.com/embed/fyUrvveAhs4"
  frameborder="0"
  allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
  style="display: block; margin: 0 auto;"
  allowfullscreen>
</iframe>

Like what you saw?

Only follow this post if you have Visual Studio Code

The following commands will:

1. Download this
   [git template](https://gist.github.com/adeekshith/cd4c95a064977cdc6c50) to
   your `~` (root) folder
2. Configure git to use the template downloaded
3. Configure git to use VS Code as the default commit message editor

Copy the following into your terminal or git bash in one shot, its all one
command chain and press enter to run it.

This presumes you are able to start VS Code in a terminal with the command
`code`.
{:.note}

```bash
curl "https://gist.githubusercontent.com/adeekshith/cd4c95a064977cdc6c50/raw/83bfb3821fa98c723bec3799a07b387ddd47a369/.git-commit-template.txt" --output ~/.git-commit-template.txt \
&& git config --global commit.template ~/.git-commit-template.txt \
&& git config --global core.editor "code --wait"
```
