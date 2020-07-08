---
layout: post
title: The case for system backups in addition to Git or another VCS
# description: >
# Howdy! This is an example blog post that shows several types of HTML content supported in this theme.
sitemap: true
---

As a former IT professional, the mantra of backups has been drilled into me. I feel that my fellow software engineers can fall into a false sense of security because we are trained to use a version control system (VCS) like git as part of our development workflow.

Backups are needed in addition to source code control because there are many things that are not in our source code. Here are a few things that are important enough to ensure that we do backups, this is by no means an exhaustive list.

- SSH Keys
- System Aliases
- Environment variables
- Code secrets (For example, API keys. These are often stored in a config file outside of Git control, or even within a system environment variable)
- Software configurations (like your text editor)
- Local git repositories that are not synced
- Small scratch projects we do when learning a new api. We often refer to these but don’t put them under source control
- Internet bookmarks
- Linter setup

We developers are often doing rapid work day in day out. This may mean that on some days we make no system changes, on other days we make a lot of changes as we put new tools/techniques into place. I highly recommend backups following the [3–2–1](https://www.backblaze.com/blog/the-3-2-1-backup-strategy/) rule. Click the link for more details, but here is a quick run-down:

- 3 copies of your data (including the original)
- 2 different mediums/devices containing your data
- 1 copy off-site (what if you have a physical disaster?)

This is my setup, and my justification for my setup.

To achieve my 3–2–1 goal, I’ve enlisted two tools. For my local system backup, I’m using Apple OS X’s built-in backup tool, Time Machine. I have my Time Machine configured to backup wirelessly to a networked drive. I took this approach because I didn’t wanted to be rooted in location within my home while backing up. Additionally, I don’t trust myself to regularly plug in my external hard drive. Time Machine gives me the benefits of a complete system restore along with file versioning. With this setup alone I have achieved the 2 different mediums rule, but I am still failing the 1 copy off site & 3 copies of your data.

To achieve the complete rules, I enlisted a cloud backup solution, [Backblaze](https://www.backblaze.com/cloud-backup.html) (if you want a free month, [click here](https://secure.backblaze.com/r/01nxgp), we'll both get a free month). I chose Backblaze for several reasons. First is their flat rate of \$50 a year per computer regardless of the data size. Backups can be set to be continuous rather than on a schedule. This means that rather than a set frequency (like time machine of every hour at its default) it will attempt to backup a file every time it changes, as soon as it changes, as long as you’ve access to the backblaze website. This is fantastic, I can have more confidence that my data is being backed up all-the-time, as long as I have internet access. With that, I’ve achieved my goal of 3–2–1. Additionally, backblaze will give me 30 days of file changes, or versions. This is great, should I need it.

The negative of Backblaze? It is not a system-level backup, I cannot do a full restore, but it is better than nothing. The initial backup can take a day or two. Applications will not be backed up, but you can install it again.

I would like to point out that file syncing tools such as dropbox and google drive are not backups. If you make a change or mess up a file, the changes will be synced across all your devices. This is important to note because of issues like ransomware. You want backups to be read-only unless you are explicitly recovering a file.

I hope you found this helpful in your day-to-day coding.

This post has been cross-posted on [Medium](https://medium.com/@brainomite/the-case-for-system-backups-in-addition-to-git-or-another-vcs-1f3a54dc5ec5)
