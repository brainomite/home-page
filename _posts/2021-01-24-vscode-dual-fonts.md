---
layout: post
title: Using Two Fonts in VS Code
description: >
  I give directions on using two fonts simultaneously in VS Code
sitemap: true
comments: true
image:
  path: /assets/img/posts/2021-01-24-vscode-fonts-and-themes/cover.png
  srcset:
    1800w: /assets/img/posts/2021-01-24-vscode-fonts-and-themes/cover.png
    900w: /assets/img/posts/2021-01-24-vscode-fonts-and-themes/cover@0,5x.png
    450w: /assets/img/posts/2021-01-24-vscode-fonts-and-themes/cover@0,25x.png
---

Like the fonts in the cover image? Want to use any two fonts, with one being
italic? I've got you covered

The theme used (that has an option for the glow effect, which I enabled) is
[SynthWave '84](https://marketplace.visualstudio.com/items?itemName=RobbOwen.synthwave-vscode)
{:.note}

> Please be aware, any time you update the CSS or VS Code is updated to a new version,
  follow steps 8-10.
{:.lead}

1. If not already installed, download and install desired fonts. I'm using
   [JetBrains Mono](https://www.jetbrains.com/lp/mono/)for my main font and [Victor Mono](https://rubjo.github.io/victor-mono/)
   but feel free to use whatever you'd like. A lot of people like [Fira Code](https://github.com/tonsky/FiraCode#download--install)
   instead of JetBrains Mono
   1. Unzip the downloaded archive
   2. Install the font
      - Windows:
        1. Select all font files in the folder
        2. Right-click any of them
        3. Pick “Install” from the menu.
      - Mac:
        1. Select all font files in the folder and double-click.
        2. Click open at the prompt
        3. Click Install Font
      - Linux:
        1. Unpack fonts to `~/.local/share/fonts` (or `/usr/share/fonts`, to
           install fonts system-wide)
        2. `fc-cache -f -v`
   3. Go to settings
   4. To set the desired main font
      1. In the settings panel, on the left, Navigate to Text Editor>Font
      2. Set 'Font Family' to your preferred font, for example, `JetBrains Mono`
         or `Fira Code`
   5. To enable ligatures
      1. In the settings panel, on the left, Navigate to Text Editor>Font
      2. Scroll down to 'Font Ligatures'
      3. Click 'edit in settings.json'
      4. Set 'editor.fontLigatures' to `true` if not already set
2. Install VSCode Extension: [Custom CSS and JS Loader](https://marketplace.visualstudio.com/items?itemName=be5invis.vscode-custom-css)
3. Download this [style.css](/assets/misc/2021-01-24-vscode-dual-fonts/styles.css)
   (right-click the link and chose 'save link as') to your desktop or a
   preferred location; know the path to the file.
   - You can modify this CSS file to suit your needs to change the font weight,
     for example. If you want to, I suggest using the VS Code development tools
     by going to Help>Toggle Developer Tools
4. Main Font: If you are using a font other than JetBrains Mono
   1. Edit the styles.css
   2. Change `JetBrains Mono` to your preferred main font
5. Cursive Font: If you are using a font other than JetBrains Mono
   1. Edit the styles.css
   2. Change `Script12 BT` to your preferred cursive font
6. Update settings.json
   1. Add the following within the settings.json, without the comments & open or
      close brackets
   ```json
   { // open of main json object
     "vscode_custom_css.imports": ["path-to-file"],
     "vscode_custom_css.policy": true
   } // close of main json object
   ```
      2. replace `path-to-file` with the correct path
         - Windows:
            - Example: `file:///C:/Users/MyUserName/Desktop/styles.css`
         - Mac or Linux
            - Example "file:///Users/MyUserName/Desktop/style.css"
7. Install the extension, [Fix VSCode Checksums](https://marketplace.visualstudio.com/items?itemName=lehni.vscode-fix-checksums) this will remove the error, "Your code installation appears to be corrupt"
8. In the command palette (view>command pallet), run `Reload Custom CSS and JS`
9. In the command palette, run `Fix Checksums: Apply`
10. Restart VS Code by exiting, not reloading, VS Code and, start it back up.

I took some content from [An alternative to Operator Mono font](https://medium.com/@docodemore/an-alternative-to-operator-mono-font-6e5d040e1c7e)
and [Multiple Fonts: Alternative to Operator Mono in VSCode](https://medium.com/@zamamohammed/multiple-fonts-alternative-to-operator-mono-in-vscode-7745b52120a0)
then modified and added some steps.
{:.faded}
