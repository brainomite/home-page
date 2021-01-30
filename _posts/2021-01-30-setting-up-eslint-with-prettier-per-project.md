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

This post is intended primarily for students at
[Thinkful](https://www.thinkful.com/).

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
rules unless Prettier has an identical rule, in which case Prettier
takes precedence.

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
2.  Optionally, let us configure VS Code to use code formatters on save. go to
    Settings>Text Editor>Formatting and make sure that `Format On Save` has a
    check
3.  Ensure your project has a `package.json`

## Installation instructions

In all 3 of the following variations of the directions we will:

1. Install Prettier and ESLint npm packages along with required packages to make
   them work together and in the environment selected.
2. Install the
   [AirBnB's Style](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb)
   npm package and related packages
3. Download the appropriate `.eslintrc.js` file which will:
   1. Configure ESLint to work with Prettier and AirBnB style guide
   2. Turn on the rule that a variable can't be less than 2 characters
   3. Make an exception for the `_` variable as certain packages like `lodash`
      and `underscore` have conventionally been assigned to the  `_`  variable.
4. Reload or restart VS Code, sometimes the changes don't take effect untill you
   do.

At the end of the post I have [all 3 variations](#gist-from-github) of the
ESLint config files that we will be using which I suggest taking a look at.

Choose the correct environment and copy single command chain (copy and paste it
all) and run it in git bash or the terminal

`&&` will cause a command to wait on the prior command then execute. `\` will
allow the command to continue on the next line
{:.note}

### Pre-React

Downloads
[browser-pre-react.eslintrc.js](https://gist.github.com/brainomite/d259b1d17b1d70959ab5c6f6ecd019a9#file-browser-pre-react-eslint-js)
and saves it as `.eslintrc.js`

<button id="pre-react-btn" onClick="codeCopyHandler('pre-react-code', 'pre-react-btn')">Copy pre-react command</button>
```bash
npm i -D prettier eslint eslint-config-airbnb-base eslint-config-prettier \
         eslint-plugin-import \
&& curl \
"https://gist.githubusercontent.com/brainomite/d259b1d17b1d70959ab5c6f6ecd019a9/raw/d9e331cc63a943298132ab7ee4a150330d636a37/browser-pre-react.eslintrc.js" \
--output .eslintrc.js
```
{: #pre-react-code}

### React

Having warnings during install of these packages are normal
{:.note}

Downloads
[react.eslintrc.js](https://gist.github.com/brainomite/d259b1d17b1d70959ab5c6f6ecd019a9#file-react-eslintrc-js)
and saves it as `.eslintrc.js`

<button id="react-btn" onClick="codeCopyHandler('react-code', 'react-btn')">Copy pre-react command</button>

```bash
npm i -D prettier eslint-plugin-react eslint-config-airbnb eslint \
         eslint-plugin-import eslint-plugin-jsx-a11y \
         eslint-plugin-react-hooks eslint-config-prettier \
&& curl \
"https://gist.githubusercontent.com/brainomite/d259b1d17b1d70959ab5c6f6ecd019a9/raw/d9e331cc63a943298132ab7ee4a150330d636a37/react.eslintrc.js" \
--output .eslintrc.js
```
{: #react-code}

### Backend with Node

Downloads
[backend.eslintrc.js](https://gist.github.com/brainomite/d259b1d17b1d70959ab5c6f6ecd019a9#file-backend-eslintrc-js)
and saves it as `.eslintrc.js`

<button id="backend-btn" onClick="codeCopyHandler('backend-code', 'backend-btn')">Copy pre-react command</button>

```bash
npm i -D prettier eslint-config-airbnb-base eslint eslint-plugin-import \
         eslint-config-prettier \
&& curl \
"https://gist.githubusercontent.com/brainomite/d259b1d17b1d70959ab5c6f6ecd019a9/raw/d9e331cc63a943298132ab7ee4a150330d636a37/backend.eslintrc.js" \
--output .eslintrc.js
```
{: #backend-code}


## Gist from GitHub
<script src="https://gist.github.com/brainomite/d259b1d17b1d70959ab5c6f6ecd019a9.js"></script>


<script>
function codeCopyHandler(codeId, buttonId) {
  const code = document.querySelector(`#${codeId} code`).innerText;
  navigator.clipboard.writeText(code);
  const btn = document.querySelector("#" + buttonId);
  const oldText = btn.innerText;
  btn.innerText = "copied!";
  setTimeout(() => (btn.innerText = oldText), 1500);
}

</script>

<style>
/*!
* Gist DarkCode ver 0.2.0
* Copyright (c) 2017 KillerCodes.in
* License: Free to use with this file header ;)
* https://gist.github.com/adimancv/eb2f4b46d3c95e6b8fe4dd52375236b2
*/
.gist{font-size: 18px}.gist-meta, .gist-file, .octotree_toggle, ul.comparison-list > li.title,button.button, a.button, span.button, button.minibutton, a.minibutton,span.minibutton, .clone-url-button > .clone-url-link{background: linear-gradient(#202020, #181818) !important;border-color: #383838 !important;border-radius: 0 0 3px 3px !important;text-shadow: none !important;color: #b5b5b5 !important}.markdown-format pre, .markdown-body pre, .markdown-format .highlight pre,.markdown-body .highlight pre, body.blog pre, #facebox pre, .blob-expanded,.terminal, .copyable-terminal, #notebook .input_area, .blob-code-context,.markdown-format code, body.blog pre > code, .api pre, .api code,.CodeMirror,.highlight{background-color: #1D1F21!important;color: #C5C8C6!important}.gist .blob-code{padding: 1px 10px !important;text-align: left;background: #000;border: 0}::selection{background: #24890d;color: #fff;text-shadow: none}::-moz-selection{background: #24890d;color: #fff;text-shadow: none}.blob-num{padding: 10px 8px 9px;text-align: right;color: #6B6B6B!important;border: 0}.blob-code,.blob-code-inner{color: #C5C8C6!important}.pl-c,.pl-c span{color: #969896!important;font-style: italic!important}.pl-c1{color: #DE935F!important}.pl-cce{color: #DE935F!important}.pl-cn{color: #DE935F!important}.pl-coc{color: #DE935F!important}.pl-cos{color: #B5BD68!important}.pl-e{color: #F0C674!important}.pl-ef{color: #F0C674!important}.pl-en{color: #F0C674!important}.pl-enc{color: #DE935F!important}.pl-enf{color: #F0C674!important}.pl-enm{color: #F0C674!important}.pl-ens{color: #DE935F!important}.pl-ent{color: #B294BB!important}.pl-entc{color: #F0C674!important}.pl-enti{color: #F0C674!important;font-weight: 700!important}.pl-entm{color: #C66!important}.pl-eoa{color: #B294BB!important}.pl-eoac{color: #C66!important}.pl-eoac .pl-pde{color: #C66!important}.pl-eoai{color: #B294BB!important}.pl-eoai .pl-pde{color: #B294BB!important}.pl-eoi{color: #F0C674!important}.pl-k{color: #B294BB!important}.pl-ko{color: #B294BB!important}.pl-kolp{color: #B294BB!important}.pl-kos{color: #DE935F!important}.pl-kou{color: #DE935F!important}.pl-mai .pl-sf{color: #C66!important}.pl-mb{color: #B5BD68!important;font-weight: 700!important}.pl-mc{color: #B294BB!important}.pl-mh .pl-pdh{color: #DE935F!important}.pl-mi{color: #B294BB!important;font-style: italic!important}.pl-ml{color: #B5BD68!important}.pl-mm{color: #C66!important}.pl-mp{color: #81A2BE!important}.pl-mp1 .pl-sf{color: #81A2BE!important}.pl-mq{color: #DE935F!important}.pl-mr{color: #B294BB!important}.pl-ms{color: #B294BB!important}.pl-pdb{color: #B5BD68!important;font-weight: 700!important}.pl-pdc{color: #969896!important;font-style: italic!important}.pl-pdc1{color: #DE935F!important}.pl-pde{color: #DE935F!important}.pl-pdi{color: #B294BB!important;font-style: italic!important}.pl-pds{color: #B5BD68!important}.pl-pdv{color: #C66!important}.pl-pse{color: #DE935F!important}.pl-pse .pl-s2{color: #DE935F!important}.pl-s{color: #B294BB!important}.pl-s1{color: #B5BD68!important}.pl-s2{color: #c5c8c6!important}.pl-mp .pl-s3{color: #B294BB!important}.pl-s3{color: #81a2be!important}.pl-sc{color: #c5c8c6!important}.pl-scp{color: #DE935F!important}.pl-sf{color: #DAD085!important}.pl-smc{color: #F0C674!important}.pl-smi{color: #c5c8c6!important}.pl-smp{color: #c5c8c6!important}.pl-sok{color: #B294BB!important}.pl-sol{color: #B5BD68!important}.pl-som{color: #C66!important}.pl-sr{color: #C66!important}.pl-sra{color: #B294BB!important}.pl-src{color: #B294BB!important}.pl-sre{color: #B294BB!important}.pl-st{color: #B294BB!important}.pl-stj{color: #c5c8c6!important}.pl-stp{color: #DE935F!important}.pl-sv{color: #DE935F!important}.pl-v{color: #DE935F!important}.pl-vi{color: #DE935F!important}.pl-vo{color: #C66!important}.pl-vpf{color: #DE935F!important}.pl-mi1{color: #8F9D6A!important;background: rgba(0,64,0,.5)!important}.pl-mdht{color: #8F9D6A!important;background: rgba(0,64,0,.5)!important}.pl-md{color: #C66!important;background: rgba(64,0,0,.5)!important}.pl-mdhf{color: #C66!important;background: rgba(64,0,0,.5)!important}.pl-mdr{color: #DE935F!important;font-weight: 400!important}.pl-mdh{color: #C66!important;font-weight: 400!important}.pl-mdi{color: #C66!important;font-weight: 400!important}.pl-ib{background-color: #C66!important}.pl-id{background-color: #C66!important;color: #fff!important}.pl-ii{background-color: #C66!important;color: #fff!important}.pl-iu{background-color: #C66!important}.pl-mo{color: #c5c8c6!important}.pl-mri{color: #DE935F!important}.pl-ms1{background-color: #c5c8c6!important}.pl-va{color: #DE935F!important}.pl-vpu{color: #DE935F!important}.pl-entl{color: #c5c8c6!important}.CodeMirror-gutters{background: #222!important;border-right: 1px solid #484848!important}.CodeMirror-guttermarker{color: #fff!important}.CodeMirror-guttermarker-subtle{color: #aaa!important}.CodeMirror-linenumber{color: #aaa!important}.CodeMirror-cursor{border-left: 1px solid #fff!important}.CodeMirror-activeline-background{background: #27282E!important}.CodeMirror-matchingbracket{outline: 1px solid grey!important;color: #fff!important}.cm-keyword{color: #f9ee98!important}.cm-atom{color: #FC0!important}.cm-number{color: #ca7841!important}.cm-def{color: #8DA6CE!important}.cm-variable-2,span.cm-tag{color: #607392!important}.cm-variable-3,span.cm-def{color: #607392!important}.cm-operator{color: #cda869!important}.cm-comment{color: #777!important;font-style: italic!important;font-weight: 400!important}.cm-string{color: #8f9d6a!important}.cm-string-2{color: #bd6b18!important}.cm-meta{background-color: #141414!important;color: #f7f7f7!important}.cm-builtin{color: #cda869!important}.cm-tag{color: #997643!important}.cm-attribute{color: #d6bb6d!important}.cm-header{color: #FF6400!important}.cm-hr{color: #AEAEAE!important}.cm-link{color: #ad9361!important;font-style: italic!important;text-decoration: none!important}.cm-error{border-bottom: 1px solid red!important}#notebook .highlight table{background: #1d1f21!important;color: #c5c8c6!important}.highlight .hll{background-color: #373b41!important}.highlight .c{color: #969896!important}.highlight .err{color: #c66!important}.highlight .k{color: #b294bb!important}.highlight .l{color: #de935f!important}.highlight .h,.highlight .n{color: #c5c8c6!important}.highlight .o{color: #8abeb7!important}.highlight .p{color: #c5c8c6!important}.highlight .cm{color: #969896!important}.highlight .cp{color: #969896!important}.highlight .c1{color: #969896!important}.highlight .cs{color: #969896!important}.highlight .gd{color: #c66!important}.highlight .ge{font-style: italic!important}.highlight .gh{color: #c5c8c6!important;font-weight: 700!important}.highlight .gi{color: #b5bd68!important}.highlight .gp{color: #969896!important;font-weight: 700!important}.highlight .gs{font-weight: 700!important}.highlight .gu{color: #8abeb7!important;font-weight: 700!important}.highlight .kc{color: #b294bb!important}.highlight .kd{color: #b294bb!important}.highlight .kn{color: #8abeb7!important}.highlight .kp{color: #b294bb!important}.highlight .kr{color: #b294bb!important}.highlight .kt{color: #f0c674!important}.highlight .ld{color: #b5bd68!important}.highlight .m{color: #de935f!important}.highlight .s{color: #b5bd68!important}.highlight .na{color: #81a2be!important}.highlight .nb{color: #c5c8c6!important}.highlight .nc{color: #f0c674!important}.highlight .no{color: #c66!important}.highlight .nd{color: #8abeb7!important}.highlight .ni{color: #c5c8c6!important}.highlight .ne{color: #c66!important}.highlight .nf{color: #81a2be!important}.highlight .nl{color: #c5c8c6!important}.highlight .nn{color: #f0c674!important}.highlight .nx{color: #81a2be!important}.highlight .py{color: #c5c8c6!important}.highlight .nt{color: #8abeb7!important}.highlight .nv{color: #c66!important}.highlight .ow{color: #8abeb7!important}.highlight .w{color: #c5c8c6!important}.highlight .mf{color: #de935f!important}.highlight .mh{color: #de935f!important}.highlight .mi{color: #de935f!important}.highlight .mo{color: #de935f!important}.highlight .sb{color: #b5bd68!important}.highlight .sc{color: #c5c8c6!important}.highlight .sd{color: #969896!important}.highlight .s2{color: #b5bd68!important}.highlight .se{color: #de935f!important}.highlight .sh{color: #b5bd68!important}.highlight .si{color: #de935f!important}.highlight .sx{color: #b5bd68!important}.highlight .sr{color: #b5bd68!important}.highlight .s1{color: #b5bd68!important}.highlight .ss{color: #b5bd68!important}.highlight .bp{color: #c5c8c6!important}.highlight .vc{color: #c66!important}.highlight .vg{color: #c66!important}.highlight .vi{color: #c66!important}.highlight .il{color: #de935f!important}
</style>