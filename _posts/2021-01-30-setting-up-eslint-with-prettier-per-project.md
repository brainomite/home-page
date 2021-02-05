---
layout: post
title: Setting Up ESLint with Prettier for Individual Projects
description: >
  I provide directions for configuring Prettier & ESLint in VS Code on a project
  basis
sitemap: true
comments: true
image:
  path: /assets/img/posts/2021-01-30-setting-up-eslint-with-prettier-per-project/cover.png
  srcset:
    1800w: /assets/img/posts/2021-01-30-setting-up-eslint-with-prettier-per-project/cover.png
    900w: /assets/img/posts/2021-01-30-setting-up-eslint-with-prettier-per-project/cover@0,5x.png
    450w: /assets/img/posts/2021-01-30-setting-up-eslint-with-prettier-per-project/cover@0,25x.png
---

This post is intended for students at
[Thinkful](https://www.thinkful.com/).


| Date | Change |
|-----------------|:-----------:|
| 2/1/2021 | Added a prerequisite step, #3, adding a setting to settings.json |
| 2/4/2021 | 1. Updated React GitHub gist, 2. Created edits table |
| 2/5/2021 | 1. Updated the copy buttons 2. Gave each script a file name|
{:.stretch-table .faded}
edits
{:.figcaption}


This post just shows you how to set up you class projects, in the future, I will
be creating a post so you can set up Prettier and ESLint to work together the
way you want for your own projects. Which, isn't actually that hard, or time
consuming. For now we want to focus on good practices.
{:.note}

1. this unordered seed list will be replaced by the toc
{:toc}

So there is this extension,
[ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint),
with over twelve million installs, is the most popular Javascript extension.
Also there is
[Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode),
which you should have installed at Thinkful’s suggestion, happens to be the
second most popular VS Code extension. These two are popular for a good reason!

ESLint will do what we call code
[linting](<https://en.wikipedia.org/wiki/Lint_(software)>) actively, as you
code, check your syntax, find common coding bugs, and additionally enforce
styles. All of the ESLint options are highly configurable.

It will:

- Auto fix well-known issues or style goofs on save. If configured, or when the
  `Format Document` command is run.
- Warn you with identified issues in two ways:
  1.  Using squiggly lines beneath the offending code
  2.  Update the Problems panel, which is in the same area as the debug panel

To use both together requires some setup. Below we will tell ESLint to use it’s
rules unless Prettier has an identical rule, in which case Prettier takes
precedence.

There are many pre-configured ESLint style configs that people use, if they
don't create their own. You can see some well known ESLint style rules
[here](https://github.com/dustinspecker/awesome-eslint#configs-by-well-known-companiesorganizations).
We will be using
[Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
{:.note}

## Prerequisites

1.  If not already added to your VS Code, install
    [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
    and/or
    [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
2.  Let us configure VS Code to use code formatters on save. go to Settings>Text
    Editor>Formatting and make sure that `Format On Save` has a check
3.  Let us configure VS Code to fix some identified eslint issues by adding the
    following to VS Code's `settings.json`

    ```json
    "editor.codeActionsOnSave": {
      // For ESLint
      "source.fixAll.eslint": true,
    }
    ```

4.  Ensure your project has a `package.json`

## Installation instructions

1. Choose the correct environment and copy the single chained command chain by
   clicking the button. Run the command from the in **_Git Bash_** or the Mac
   terminal while in the project's root
2. If VS Code is running, reload or restart VS Code, sometimes the changes won't
   take effect until you do.

`&&` will cause a command to wait on the prior command then execute. `\` will
allow the command to continue on the next line
{:.note}

In all 3 of the following variations of the command will:

- Install Prettier and ESLint npm packages along with required packages to make
  them work together and in the environment selected.
- Install the
  [AirBnB's Style](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb)
  npm package and related packages
- Download the appropriate `.eslintrc.js` file which will:
  - Configure ESLint to work with Prettier and AirBnB style guide
  - Turn on the rule that a variable can't be less than 2 characters
  - Make an exception for the `_` variable as certain packages like `lodash` and
    `underscore` have conventionally been assigned to the `_` variable.

At the end of the post I have [all 3 variations](#gist-from-github) of the
ESLint config files that we will be using which I suggest taking a look at.

### Pre-React

In addition to installing NPM packages, this command will auto download
`browser-pre-react.eslintrc.js`, which you can find below in the
[Gist From Github](#gist-from-github) section and saves it as `.eslintrc.js`

```bash
# file: "pre-react.sh"
npm i -D prettier eslint eslint-config-airbnb-base eslint-config-prettier \
         eslint-plugin-import \
&& curl \
"https://gist.githubusercontent.com/brainomite/d259b1d17b1d70959ab5c6f6ecd019a9/raw/d9e331cc63a943298132ab7ee4a150330d636a37/browser-pre-react.eslintrc.js" \
--output .eslintrc.js
```
{: #pre-react-code}

<button id="pre-react-btn" onClick="codeCopyHandler('pre-react-code', 'pre-react-btn')">Click
to Copy!</button>

### React

Having warnings during install of these packages are normal
{:.note}

In addition to installing NPM packages, this command will auto download
`react.eslintrc.js`, which you can find below in the
[Gist From Github](#gist-from-github) section and saves it as `.eslintrc.js`

```bash
# file: "react.sh"
npm i -D prettier eslint-plugin-react eslint-config-airbnb eslint \
         eslint-plugin-import eslint-plugin-jsx-a11y \
         eslint-plugin-react-hooks eslint-config-prettier \
&& curl \
"https://gist.githubusercontent.com/brainomite/d259b1d17b1d70959ab5c6f6ecd019a9/raw/d3426bfa31a78ec68f7df36018b0f2aedfe51709/react.eslintrc.js" \
--output .eslintrc.js
```
{: #react-code}

<button id="react-btn" onClick="codeCopyHandler('react-code', 'react-btn')">Click
to Copy!</button>

### Backend with Node

In addition to installing NPM packages, this command will auto download
`backend.eslintrc.js`, which you can find below in the
[Gist From Github](#gist-from-github) section and saves it as `.eslintrc.js`

```bash
# file: "backend.sh"
npm i -D prettier eslint-config-airbnb-base eslint eslint-plugin-import \
         eslint-config-prettier \
&& curl \
"https://gist.githubusercontent.com/brainomite/d259b1d17b1d70959ab5c6f6ecd019a9/raw/d9e331cc63a943298132ab7ee4a150330d636a37/backend.eslintrc.js" \
--output .eslintrc.js
```
{: #backend-code}

<button id="backend-btn" onClick="codeCopyHandler('backend-code', 'backend-btn')">Click
to Copy!</button>

## Gist from GitHub

<script src="https://gist.github.com/brainomite/d259b1d17b1d70959ab5c6f6ecd019a9.js"></script>


/* https://github.com/lonekorean/gist-syntax-themes */
<style>
@import url('https://cdn.rawgit.com/lonekorean/gist-syntax-themes/848d6580/stylesheets/monokai.css');
</style>
