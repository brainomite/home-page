---
layout: post
title: Setting up Dev environment with WSL2 and VS Code
description: >
  We are going go through the process of setting up Ubuntu on windows using the
  "Windows Subsystem for Linux 2" to work with Node, VS Code and Git.
sitemap: true
comments: true
image:
  path:    /assets/img/posts/2021-02-24-wsl2-node-set-up/cover.png
  srcset:
    1800w: /assets/img/posts/2021-02-24-wsl2-node-set-up/cover.png
    900w:  /assets/img/posts/2021-02-24-wsl2-node-set-up/cover@0,5x.png
    450w:  /assets/img/posts/2021-02-24-wsl2-node-set-up/cover@0,25x.png
---

Cover image credit to [ars Technica](https://arstechnica.com/information-technology/2020/03/the-windows-subsystem-for-linux-conference-was-a-virtual-success/)
{:.figcaption}

I was able to complete all of the steps from this post in ten minutes. That is
with a 300 Megabit internet connection. Depending on the speed of your computer
and Internet connection it should take between 10 & 25 minutes to do the steps.
{:.note}


1. this unordered seed list will be replaced by the toc
{:toc}

## What we set up

- Windows Subsystem For Linux 2 (WSL2)
- The Linux distribution, Ubuntu 20.04 LTS
    - We will update & upgrade Ubuntu's packages
    - We will set git defaults
        - Git default name will be set to GitHub's Name
        - Git default name will be set to Github default email
        - Git default starting branch will be changed to `main`
        - Git's default editor will be set to VS Code
    - We will install Node Version Manager (nvm)
    - We will use NVM to download the current LTS version of Node
    - We will use NVM to update Node Package Manager (npm) to current version
    - We will install the VS Code WSL Server
- Install and initially configure Windows Terminal
    - We will override the terrible home folder default for Ubuntu 20.04, and
       change it to the (~) path as is default for an Ubuntu session
    - Optionally set Windows Terminal to use Ubuntu 20.04 as it's default
       Terminal session


## Caveats

There are four main caveats to be aware of when using WSL2

1. It is __strongly advised__ to use VS Code with WSL2 using one of the following
   methods
    1. Using the commands
        - `code .` to open the current folder as the workspace
        - `code path_to_folder` to open the folder as the workspace
        - `code path_to_file` to open an individual file
    2. Using `File>Open Recent` within VS code if you have opened the
       folder/file recently
2. For now, graphic based tools are not supported. However that is going to
   [change soon](https://www.techrepublic.com/article/windows-subsystem-for-linux-2-the-gui-features-developers-have-been-asking-for/#ftag=RSS56d97e7)
3. The file system of the Linux distribution, in this case Ubuntu 20.04, is
   separate from the windows file system. For any projects you wish to work on
   within Ubuntu, you should
    1. Commit changes an push it up to GitHub from windows
    2. Within Ubuntu, git clone it to the folder of your liking.
4. Tools within Ubuntu, like `create-react-app`'s `npm start` script will not
   open your browser automatically like before. Simple ctrl-click the URL that
   these tools typically display, and it'll open in the browser of your choice!
   This method is confirmed to work in:
    - VS Code
    - Windows Terminal

## Video for steps

<div class="youtube-container">
    <iframe
        src="https://www.youtube.com/embed/wOZE4ErAStA"
        frameborder="0"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen
        class="youtube-video"
    >
    </iframe>
</div>

## Steps

### Prerequisites

1. Check Windows Update
2. Open Powershell as an administrator
2. run `msinfo32`
    - we want 'Hyper-V Virtualization Enabled in Firmware' to say yes
3. continue only if virtualization is not enabled.
    - If you have an intel system you want `VT-X` enabled, AMD, `AMD-V`. For
      both systems it may also be called `Virtualization Technology`
    - You can try and figure it out using one of the following two sites, or
      reach out to your computer's technical support.
        1. [YouTube: How To Enable Virtualization Technology VT-x AMD v from BIOS with UEFI firmware settings in Windows](https://youtu.be/8-HypXAzo6o)
        2. [Enabling virtualization on your computer](https://2nwiki.2n.cz/pages/viewpage.action?pageId=75202968#:~:text=Press%20F2%20key%20at%20startup,changes%20and%20Reboot%20into%20Windows.)



### install wsl1

1. From an administrative powershell session run the following command, it'll
enable features built into windows & it will auto reboot, be prepared

    ```js
    // File: "install-wsl-1-and-reboot.ps"
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart; dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart; dism.exe /online /enable-feature /featurename:HypervisorPlatform /all /norestart; Restart-Computer
    ```

### update wsl1 to wsl2
1. Download and install this
   [WSL2 Linux kernel update package for x64 machines](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)
2. Launch powershell terminal `wsl --set-default-version 2`

### Install Ubuntu

1. From the Microsoft Store, download [Ubuntu 20.04 lts](https://www.microsoft.com/store/apps/9n6svws3rx71)

### Setup Ubuntu

1. Open ubuntu 20.04 LTS and go through initial setup for the user & password
2. Run the below shell script, which must be copied into the terminal in one
   shot. While this is running move onto Installing
   [Windows Terminal](#windows-terminal)

#### Install Script

Feel free to read the comments that explain step by step what this script does.
{:.note}

```bash
# file: "update-and-setup.sh"
touch update-and-setup.sh && chmod u+x update-and-setup.sh && echo '
# gather info
echo Please enter your full name for GitHub:
read name
echo Please enter the email you wish to use with GitHub:
read email
echo

# validation of user input
echo name: $name
echo e-mail: $email
echo "Are you sure this is all correct and you wish to proceed? [yN]:"
read REPLY

# Check if the reply is "y" or "Y"
if [[ ! $REPLY =~ ^[Yy]$ ]]
then
    # if it is...
    echo No changes made, try again
    rm ~/update-and-setup.sh # remove this script
    exit 1 #  exit the script with status code 1, and error
fi

# configure the system

# update Ubuntu
sudo apt update # update the package versions
sudo apt upgrade -y # upgrade installed packages to new versions if was found

# config git
git config --global user.name "$name" # update github name
git config --global user.email "$email" # use your github email
git config --global init.defaultBranch main # change git main branch to, main
git config --global core.editor "code --wait" # set VS Code as default git editor

# install nvm, node, npm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash # Install nvm
export NVM_DIR="$HOME/.nvm" # add NVM to the path
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion" # this loads bash completion for nvm
nvm install --lts # install latest node LTS module
nvm install-latest-npm # install latest version of node

# install wsl extension for vs code https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl
code --install-extension ms-vscode-remote.remote-wsl

# set up ubuntu for vscode

code . # open VS Code for first time to install wsl server for VS Code

# clean up after script

rm update-and-setup.sh # cleaning up
' >> update-and-setup.sh && ./update-and-setup.sh # run the script
```
### Windows Terminal

#### Setup

1. install [Windows Terminal](https://aka.ms/terminal)
2. Open terminal settings
3. Set default directory for Ubuntu 20.04 `"startingDirectory": "//wsl$/Ubuntu-20.04/home/ubuntuUserName/"`
4. Optionally Set Ubuntu as the default shell by updating the guid to the correct shell.

#### Tips and Tricks

- Microsoft wrote this post,
    [Windows Terminal Tips And Tricks](https://devblogs.microsoft.com/commandline/windows-terminal-tips-and-tricks/),
    but specifically I wanted to call out their section on
    [Panes](https://devblogs.microsoft.com/commandline/windows-terminal-tips-and-tricks/#panes)
- [6 Tips for an Amazing Workflow With Windows Terminal and WSL2](https://medium.com/swlh/6-tips-for-an-awesome-workflow-with-windows-terminal-and-wsl2-c430306369df)
  Start with Tip 2 as we already did 1.



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
