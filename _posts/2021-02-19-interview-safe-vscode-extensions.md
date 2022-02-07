---
layout: post
title: Interview Safe VS Code Extensions I Like
description: >
  I provide a list of my some of my favorite useful (interview safe) extensions
sitemap: true
comments: true
---

This is a collections of extensions that enhances or speeds up development but
not to the extent that we can't pass certain kinds of technical interviews.

Before installing any extension, at the minimum, you should **always** read the
overview of an extension to see:

1. The capabilities of the extension
2. How to use it. There may be settings to update.

The videos are demos/brief descriptions, on average 1.5 minutes long {:.note}

- this unordered seed list will be replaced by the toc {:toc}

## Extensions that make me more productive

### [Auto Rename Tags](https://marketplace.visualstudio.com/items?itemName=formulahendry.auto-rename-tag)

- Easily rename HTML/JSX/XML tags
- <iframe width="560" height="315" src="https://www.youtube.com/embed/t5sY2n9HuTk" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Bracket Pair Colorizer 2 - This addon has been depreciated - read below to setup built in equivalent

- This will make brackets be colorized so that you can easily visually find the
  matching closing bracket for every open bracket or find ones that are missing
  their closing bracket.
- Brackets are - [], (), {}
- <iframe width="560" height="315" src="https://www.youtube.com/embed/kqK2qoiPu0I" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

#### Setup

1. Open the command palette using one of the following keyboard shortcuts

- Windows: Ctrl+Shift+P)
- Mac: Shift + CMD + P

2. Type "open settings"
3. You are presented with options, find and select
   `Preferences: Open Settings (JSON)`
4. Add the following to the JSON file within the outer {}

```JSON
"editor.bracketPairColorization.enabled": true,
"editor.guides.bracketPairs":"active"
```

### [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)

- A basic spell checker that works with programming including, in camelCase or
  snake case variable names
- Make your comments & variable more professional due to the lack of spelling
  errors
- If you are like me, staring at code often makes me question the spelling of
  things, and having this gives me a sanity check
- <iframe width="560" height="315" src="https://www.youtube.com/embed/K9_9LQqAaUw" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### [Duplicate action](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-duplicate)

- Adds ability to duplicate files and directories in VS Code from the file
  explorer.
- <iframe width="560" height="315" src="https://www.youtube.com/embed/ILK4UoIV3_c" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### [indent-rainbow](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)

- This allows you to see indent levels via colors visually.
- <iframe width="560" height="315" src="https://www.youtube.com/embed/RtNrGoMLgg8" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)

- When you work on a simple website, this will cause the browser to auto-refresh
  every time you save your work. This way you can see your latest changes
  quicker
- <iframe width="560" height="315" src="https://www.youtube.com/embed/q7LxB6Ns7rk" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### [Live Share](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare)

- Share your coding session including the codebase, any locally running terminal
  you can, when configured, even access a running webserver
- Useful for when working with instructors, mentors, or even when pair-coding
  remotely
- Every participant gets to use their VS Code, with THEIR font size, font,
  colors etc. Way better than screen sharing for this reason
- <iframe width="560" height="315" src="https://www.youtube.com/embed/O1fLV7xsNFc" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### [Reload](https://marketplace.visualstudio.com/items?itemName=natqe.reload)

- I frequently see vscode or an extension spaz out. I used to quit and restart
  VS Code, but no more.
- Really simple addon that will add a reload button to your status bar on the
  bottom.
- <iframe width="560" height="315" src="https://www.youtube.com/embed/dizHpzddEDs" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### [TabOut](https://marketplace.visualstudio.com/items?itemName=albert.TabOut)

- As they put it, it'll allow you to "tab out of quotes, brackets, etc for
  Visual Studio Code."
- <iframe width="560" height="315" src="https://www.youtube.com/embed/hojvatbAuXU" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### [Todo Tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree)

- Make a to-do list, inside of VS Code based on your code comments!
- This will work with comments that contain the word `TODO` or `FIXME`
- Useful for your projects
- <iframe width="560" height="315" src="https://www.youtube.com/embed/pQ72cAFDsTA" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)

- When you work on a plain website, using VS Code, this will cause the browser
  to auto-refresh every time you save your work. This way you can see your
  latest changes quicker.

## Other Extensions

### Coding Time Trackers

Are you like me, someone who loves stats? Do you want to track how much coding
you are doing? Here are two options. I actually use both simultaneously

#### WakaTime

[WakaTime](https://wakatime.com/) will let you know exactly this and more!

You'll know how much time you spent coding overall and broken down by projects.
You'll also see a breakdown of time by language.

Please be aware they are collecting your data, and if you are concerned about
privacy, you can check out the FAQ on privacy [here](https://wakatime.com/faq).
{:.note}

The free account will show you your coding for the past 2 weeks.

Should you wish to use this, sign up and then follow
[these directions](https://wakatime.com/vs-code) to set it up with VS code.

#### [Code Time](https://marketplace.visualstudio.com/items?itemName=softwaredotcom.swdc-vscode)

There's this VS Code extension
[Code Time](https://marketplace.visualstudio.com/items?itemName=softwaredotcom.swdc-vscode)
and its complimentary [web service](https://www.software.com/code-time), that
will let you know exactly this and more!

You'll know how much time you spent coding overall and broken down by projects.
You'll also see a breakdown of time by language.

The free account will show you your coding for the past 90 days.
