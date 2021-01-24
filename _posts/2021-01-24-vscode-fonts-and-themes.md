---
layout: post
title: Styling VS Code
description: >
  I provide fonts, icons, and themes for adjusting the look of VS Code
sitemap: true
comments: true
image:
  path:    /assets/img/posts/2021-01-24-vscode-fonts-and-themes/cover.png
  srcset:
    1800w: /assets/img/posts/2021-01-24-vscode-fonts-and-themes/cover.png
    900w:  /assets/img/posts/2021-01-24-vscode-fonts-and-themes/cover@0,5x.png
    450w:  /assets/img/posts/2021-01-24-vscode-fonts-and-themes/cover@0,25x.png
---

I'm going to give some options for styling your vscode. We use VS Code daily,
all day long. Why not set the icons, fonts, and theme to a style you like best?
Your eyes will thank you!

The cover image actually uses a combination of two fonts, [JetBrains Mono](https://www.jetbrains.com/lp/mono/)
& [Script12 BT](https://www.dafontfree.net/freefonts-script12-bt-f141942.htm)! This is
done using some trickery but I'll be sharing how in a how-to coming later, stay
tuned!
{:.note}

Everything in this post is geared towards dark mode as that is my preference.
{:.note}

1. this unordered seed list will be replaced by the toc
{:toc}

If you like any of these options, tell me about it in the comments below.
Have a theme you like that I've not demoed? Again tell me about it in the
comments below.

## Icon Themes

These will set icons for files and optionally folders.

### Changing Selected Icon Theme

1. Optionally download a new icon theme
2. Go to Settings
3. Search for `icon theme`
4. Find the 'Workbench: Icon Theme' section
5. Select the desired icon theme

### Themes


#### [Material Icon Theme](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)

This is my preferred icon theme.
{:.note}

![default seti icons](/assets/img/posts/2021-01-24-vscode-fonts-and-themes/icons-material-theme.png)

#### [vscode-icons](https://marketplace.visualstudio.com/items?itemName=vscode-icons-team.vscode-icons)

![default seti icons](/assets/img/posts/2021-01-24-vscode-fonts-and-themes/icons-vscode-icons.png)

#### Default Icons - Seti

![default seti icons](/assets/img/posts/2021-01-24-vscode-fonts-and-themes/icons-default-seti.png)

This one doesn't have icons for folders
{:.figcaption}

## Fonts

The two fonts used in this post are [JetBrains Mono](https://www.jetbrains.com/lp/mono/)
and [Fira Code](https://github.com/tonsky/FiraCode). I recently switched to
Jetbrains Mono.

### Ligatures

There is this concept of coding fonts with two major benefits.
They are mono-spaced, meaning every character is the same width.
But the main draw is that they support coding [ligatures](https://en.wikipedia.org/wiki/Orthographic_ligature#Programming_languages).

<div class="juxtapose">
    <img src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/fira-code-no-ligatures.png" data-label="No Ligatures"/>
    <img src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/fira-code.png" data-label="Ligatures"/>
</div>

Comparison of Fira Code with and without ligatures enabled.
{:.figcaption}

JetBrains Mono doesn't have as many ligatures as Fira Code
{:.note}

<div class="juxtapose">
    <img src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/jetbrains-mono-no-ligatures.png" data-label="No Ligatures"/>
    <img src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/jetbrains-mono.png" data-label="Ligatures"/>
</div>

Comparison of JetBrains Mono with and without ligatures
{:.figcaption}

<div class="juxtapose">
    <img src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/jetbrains-mono.png" data-label="JetBrains Mono"/>
    <img src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/fira-code.png" data-label="Fira Code"/>
</div>

Comparison of Ligatures
{:.figcaption}

### Font Installation Directions

1.  Download the font you prefer

    - [JetBrains Mono](https://www.jetbrains.com/lp/mono/)
    - [Fira Code](https://github.com/tonsky/FiraCode#download--install)

2.  Unzip the downloaded archive
3.  Install the font
    - Windows:
      1. Select all font files in the folder
      2. Right-click any of them
      3. Pick “Install” from the menu.
    - Mac:
      1.  Select all font files in the folder and double-click.
      2.  Click open at the prompt
      3.  Click Install Font
    - Linux:
      1. Unpack fonts to `~/.local/share/fonts` (or `/usr/share/fonts`, to
         install fonts system-wide)
      2. `fc-cache -f -v`
3.  Go to settings
4.  To set desired font
    1. In the settings panel, on the left, Navigate to Text Editor>Font
    2. Set 'Font Family' to your preferred font, for example `JetBrains Mono`
       or `Fira Code`
5.  To enable ligatures
    1. In the settings panel, on the left, Navigate to Text Editor>Font
    2. Scroll down to 'Font Ligatures'
    3. click 'edit in settings.json'
    4. set 'editor.fontLigatures' to `true` if not already set

## Themes

My preferred theme is Lukin Theme.

Some themes come with its own Icons and will override your selected icons. You
can change it back after setting the theme if you want.
{:.note}

Not all themes use italics for certain things i.e parameter declarations or HTML
attribute names.
{:.note}

### Update your theme

To change your theme:
1. If needed download your theme
2. Go to Settings
3. Navigate to workbench>appearance
4. Change the 'Color Theme' setting to your preferred theme
5. If needed, follow the change the selected icon theme [instructions](#changing-selected-icon-theme)

### VS Code Themes I've Used in the Past

#### [Lukin Theme](https://marketplace.visualstudio.com/items?itemName=lukinco.lukin-vscode-theme)

This my preferred theme

<div class="juxtapose">
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/lukin-jetbrains-mono.png"
    data-label="JetBrains Mono"
  />
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/lukin-fira-code.png"
    data-label="Fira Code"
  />
</div>

#### [SynthWave '84](https://marketplace.visualstudio.com/items?itemName=RobbOwen.synthwave-vscode) with Glow Enabled

My screenshots from here on in for sharing code will use JetBrains Mono with the
glow enabled.

The glow effect requires additional configurations
{:.note}

<div class="juxtapose">
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/synthwave-84-glow-jetbrains-mono.png"
    data-label="JetBrains Mono"
  />
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/synthwave-84-glow-fira-code.png"
    data-label="Fira Code"
  />
</div>

#### [SynthWave '84](https://marketplace.visualstudio.com/items?itemName=RobbOwen.synthwave-vscode) without Glow Enabled

<div class="juxtapose">
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/synthwave-84-jetbrains-mono.png"
    data-label="JetBrains Mono"
  />
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/synthwave-84-fira-code.png"
    data-label="Fira Code"
  />
</div>

#### [Snazzy Operator](https://marketplace.visualstudio.com/items?itemName=aaronthomas.vscode-snazzy-operator)

<div class="juxtapose">
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/snazzy-operator-jetbrains-mono.png"
    data-label="JetBrains Mono"
  />
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/snazzy-operator-fira-code.png"
    data-label="Fira Code"
  />
</div>

#### [Material Theme Darker High Contrast](https://marketplace.visualstudio.com/items?itemName=equinusocio.vsc-material-theme)

<div class="juxtapose">
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/material-theme-darker-high-contrast-jetbrains-mono.png"
    data-label="JetBrains Mono"
  />
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/material-theme-darker-high-contrast-fira-code.png"
    data-label="Fira Code"
  />
</div>

#### [Material Theme Ocean High Contrast](https://marketplace.visualstudio.com/items?itemName=equinusocio.vsc-material-theme)

<div class="juxtapose">
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/material-theme-ocean-high-contrast-jetbrains-mono.png"
    data-label="JetBrains Mono"
  />
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/material-theme-ocean-high-contrast-fira-code.png"
    data-label="Fira Code"
  />
</div>

#### [One Dark Pro](https://marketplace.visualstudio.com/items?itemName=azemoh.one-monokai)

<div class="juxtapose">
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/one-dark-pro-jetbrains-mono.png"
    data-label="JetBrains Mono"
  />
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/one-dark-pro-fira-code.png"
    data-label="Fira Code"
  />
</div>

#### [One Monokai](https://marketplace.visualstudio.com/items?itemName=azemoh.one-monokai)

<div class="juxtapose">
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/one-monokai-jetbrains-mono.png"
    data-label="JetBrains Mono"
  />
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/one-monokai-fira-code.png"
    data-label="Fira Code"
  />
</div>

#### Monokai - included with VS Code

<div class="juxtapose">
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/monokai-jetbrains-mono.png"
    data-label="JetBrains Mono"
  />
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/monokai-fira-code.png"
    data-label="Fira Code"
  />
</div>

#### Monokai Dimmed - included with VS Code

<div class="juxtapose">
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/monokai-dimmed-jetbrains-mono.png"
    data-label="JetBrains Mono"
  />
  <img
    src="/assets/img/posts/2021-01-24-vscode-fonts-and-themes/monokai-dimmed-fira-code.png"
    data-label="Fira Code"
  />
</div>

<style>
  img[alt*="icons"]{
    display: block;
    margin: 0 auto;
  }
  .juxtapose {
    margin-bottom: 10px;
  }
</style>

<!-- This is for Juxtapose
     https://github.com/NUKnightLab/juxtapose
 -->
<link rel="stylesheet" href="/assets/css/juxtapose.css">
<script src="/assets/js/juxtapose.min.js"></script>
