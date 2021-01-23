---
layout: post
title: Styling VS Code
description: >
  I provide fonts and themes for adjusting the look of VS Code
sitemap: true
comments: true
---

1. this unordered seed list will be replaced by the toc
{:toc}

## Fonts

## Ligatures

Student,

There is this concept of coding fonts with two major benefits.
They are mono-spaced, every character is the same width.
But the main draw is that they support coding [ligatures](https://en.wikipedia.org/wiki/Orthographic_ligature#Programming_languages).


Here is a comparison of two fonts with and without ligatures

<iframe frameborder="0" class="juxtapose" width="100%" height="440.00000000000006" src="https://cdn.knightlab.com/libs/juxtapose/latest/embed/index.html?uid=bf61a5c6-5db9-11eb-83c8-ebb5d6f907df"></iframe>


Before installing an extension, at the minimum, you should always read the
overview of an extension to see:
1. The capabilities of the extension
2. How to use it. There may be settings to update.

You don't need to install the extensions individually, if you like the idea of
all of them, read the following section and I have an extension pack that
will install all of these extensions in one shot!
{:.note}


TL;DR - Download all the suggested extensions [here](#download-of-all-the-suggested-extensions)

Thinkful has a module on installing Visual Studio Code. In that module, they
recommend [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode), a
fantastic extension that I use daily.

## Extensions that I Find Useful

- [Bracket Pair Colorizer 2](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer-2)
   - This will make brackets be colorized so that you can easily visually find
   the matching closing bracket for every open bracket or find ones that are
   missing their closing bracket.
   - Brackets are - [], (), {}
- [indent-rainbow](https://marketplace.visualstudio.com/items?itemName=oderwat.indent-rainbow)
   - This allows you to see indent levels via colors visually.
- [javascript console utils](https://marketplace.visualstudio.com/items?itemName=whtouche.vscode-js-console-utils)
   - An easy way to add/remove of the console.logs with labels for the variables
- [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)
   - A basic spell checker that works with programming
   - Make your comments & variable more professional due to the lack of spelling
   errors
   - If you are like me, staring at code often makes me question the spelling of
   things, and having this gives me peace of mind
- [Todo Tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree)
   - Make a todo list, inside of VS Code based on your code comments!
      - This will work with comments that start with `// TODO ` or `// FIXME `
   - Useful for your assignments, there are more rubrics to complete as you
  saw in the 'Build your portfolio web page'
- [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer)
   - When you work on a plain website, using VS Code, this will cause the
   browser to auto-refresh every time you save your work. This way you can see
   your latest changes quicker.

## Download of all the Suggested Extensions

Using this extension pack, we can install all of the above extensions in one
step.
You can find the repo for the extension pack [here](https://github.com/brainomite/suggested-thinkful-extension-pack).

### Directions

1. Download the extension pack [here](/assets/suggested-thinkful-extension-pack.vsix)
2. Open VS Code (if not already open)
3. Press the `F1` key or in the menu go to view>Command Palette
4. Type `vsix` and select `Extensions: Install from VSIX`
5. Navigate to the file you downloaded and select it.
6. Pay attention to the popup on bottom right of VS Code, it will tell you when
   all of the extensions have been installed.

## Coding Time Tracker

Are you like me, someone who loves stats?
Do you want to track how much coding you are doing?

There's this tool [WakaTime](https://wakatime.com/), that will let you know
exactly this and more!

You'll know how much time you spent coding overall and broken down by projects.
You'll also see a breakdown of time by language.

Please be aware they are collecting your data, and if you are concerned about privacy,
you can check out the FAQ on privacy [here](https://wakatime.com/faq).
{:.note}

The free account will show you your coding for the past 2 weeks.

Should you wish to use this, sign up and then follow [these directions](https://wakatime.com/vs-code)
to set it up with VS code.

Hit me up in the comments!

