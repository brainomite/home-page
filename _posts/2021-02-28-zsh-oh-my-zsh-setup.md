---
layout: post
title: Setting ZSH Oh-my-zsh and optionally a theme
description: >
  We are going go through the process of setting up ZSH and Oh My Zsh on Mac and
  WSL2's Ubuntu 20.04

sitemap: true
comments: true
image:
  path:    https://camo.githubusercontent.com/3ec75cb1c3278cce3c661d3bcf72a4eca75db241a6ace648ea014b02f3f44458/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6f686d797a73682f6f682d6d792d7a73682d6c6f676f2e706e67
  # srcset:
  #   1800w: /assets/img/posts/2021-02-24-wsl2-node-set-up/cover.png
  #   900w:  /assets/img/posts/2021-02-24-wsl2-node-set-up/cover@0,5x.png
  #   450w:  /assets/img/posts/2021-02-24-wsl2-node-set-up/cover@0,25x.png
---


## Why use ZSH?

ZSH is a replacement shell for Linux and Mac terminals. ZSH can do everything
that BASH can do plus more. In fact the latest version of MacOS (Big Sur)
actually defaults to the ZSH shell.

There are many features but the following are just four of my favorites.

- [Automatic cd](https://github.com/hmml/awesome-zsh#auto-change-directories):
  Just type the name of the directory relative to current folder or the full path
- [Recursive path expansion](https://github.com/hmml/awesome-zsh#expand-paths):
  For example “/u/lo/b” expands to “/usr/local/bin” when you hit the tab key
- Plugins to auto complete common tools such as git, npm and loads more
- Amazing [history navigation](https://github.com/hmml/awesome-zsh#history-navigation):
  what this means, is that in addition to just pressing up for your most recent
  command, you can start typing a command and when you press the up key it'll
  only show the history that starts with those characters

## Oh-My-ZSH?

[oh-my-zsh](https://ohmyz.sh/) is a plugin for zsh that has lots of convenient
features including loads of useful built-in command aliases (that can be found
on this [cheat sheet](https://github.com/ohmyzsh/ohmyzsh/wiki/Cheatsheet)) but
my favorite thing? Loads of themes to customize your terminal both included (see
examples of the built-in themes
[here](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes)) as well as third-party
themes.


## Directions

1. Install ZSH if necessary. First to check, run from a Mac/Ubuntu terminal,
   `zsh --version`, If you get a version number it is installed, Skip to the
   next major step.
  2. Install ZSH
      - __WSL2 Ubuntu__:
        1. Run from the ubuntu shell, entering your password when prompted `sudo apt install zsh -y && chsh -s $(which zsh)`
        3. Close and re-open the shell
        4. A tool will launch to config ZSH, press `q` to 'Quit and do nothing'
        5. Update the zsh config file, `~/.zshrc` for nvm by running:
      - __MacOS prior to Version 11 (Big Sur)__
        - directions to come at a later point in time, sorry
2. Install prerequisite third party terminals so we can see the cool new fonts
   and better range of colors.
  - __Windows__: Download & Install [Windows Terminal](https://aka.ms/terminal)
  - __Mac__: Download & Install [Iterm2](https://iterm2.com/)
3. Install oh-my-zsh
    ```bash
    # file: "install-oh-my-zsh.sh"
    sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    ```
  1.
4. Check if nvm still works, by running `nvm --version` run the following command if you get an error:
    ```bash
    # file: "fix nvm.sh"
    echo '\n# nvm setup from .bashrc\nexport NVM_DIR="$HOME/.nvm"\n[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm' >> .zshrc
    ```
5. Optional but suggested, pick a theme! I an NOT a fan of the default theme.
   There are 2 main ways to get a theme, choose an included one, or download an
   external theme. I recommend trying one of the included ones, option 1, before trying
   my preferred external theme, option 2.
    1. __Using an included theme__
        1. Visit the theme page, and find a theme you like. Note that a few have
            instructions beneath the image.
        2. Open the zsh config file in VS Code: `code ~/.zshrc`
        3. Find the line that starts with `ZSH_THEME` and change the theme name
            to the one you've selected
    2. __Use an external theme__ most can be on this list of [external themes](https://github.com/ohmyzsh/ohmyzsh/wiki/External-themes)
          - These require additional configuration, I will *not* be explaining how to
            set up any of them them.
          - The theme I used and is source of the video below, is
            [powerlevel10k](https://github.com/ohmyzsh/ohmyzsh/wiki/External-themes#powerlevel10k)
            It is very configurable and geeky in my opinion.

<div class="youtube-container">
    <iframe
        src="https://www.youtube.com/embed/XTri3QiYrWc"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
        class="youtube-video"
    >
    </iframe>
</div>

<style>
    .youtube-container {
        position: relative;
        width: 100%;
        height: 0;
        padding-bottom: 56.25%;
    }
    .youtube-video {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
    }
</style>
