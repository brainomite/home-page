---
layout: post
title: Setting Up ESLint with Prettier
description: >
  I provide directions for configuring Prettier & ESLint in VS Code
sitemap: true
comments: true
image:
  path: /assets/img/posts/2021-01-27-setting-up-eslint-with-prettier/cover.png
  srcset:
    1800w: /assets/img/posts/2021-01-27-setting-up-eslint-with-prettier/cover.png
    900w: /assets/img/posts/2021-01-27-setting-up-eslint-with-prettier/cover@0,5x.png
    450w: /assets/img/posts/2021-01-27-setting-up-eslint-with-prettier/cover@0,25x.png
---

This post is being recreated.

<strike>

This post is intended primarily for students at
[Thinkful](https://www.thinkful.com/).

So there is this extension,
[ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint),
with over twelve million installs, is the most popular Javascript extension.
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
default rules unless Prettier has an identical rule, in which case Prettier
takes precedence.

Here are the setup instructions with my suggested configuration:

1.  If not already added to your VS Code, install
    [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
    and/or
    [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
2.  run this command to install the dependencies globally on your machine: 
    `npm i -g eslint eslint-config-prettier eslint-plugin-prettier prettier`
3.  Create and open for editing a global ESLint config file, which will only be
    used if ESLint isn't configured for a specific project. Run:
    `touch ~/.eslintrc && code ~/.eslintrc`

        By the way, `&&` lets us run a second (or more) command(s) when the
        prior one completes
        {:.note}

4.  Add the following to the .eslintrc file which will tell ESLint to

    - Use the Prettier config to prefer Prettier's rules for rules that match
    - Register the Prettier plug-in we installed
    - Show Prettier errors as errors

    ```json
    {
      "extends": ["plugin:prettier/recommended"],
      "plugins": ["prettier"],
      "rules": {
        "prettier/prettier": "error"
      }
    }
    ```

5.  Let us configure VS Code to use code formatters on save. go to Settings>Text
    Editor>Formatting and make sure that `Format On Save` has a check
6.  Restart VS Code.

Many open-source projects on GitHub, and even on the job have specialized ESLint
settings, but that is all right. Both ESLint and Prettier can be configured on a
project-basis by putting a config file in the project’s root, which will
override the global settings. Lots of preexisting projects provide the
`.eslintrc` file in their repositories.

There are many pre-configured ESLint style configs that people use, if they
don't create their own You can see some well known ESLint style rules
[here](https://github.com/dustinspecker/awesome-eslint#configs-by-well-known-companiesorganizations).
{:.note}

</strike>
