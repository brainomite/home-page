---
layout: post
title: Setting up a JS Dev Env with WSL2, Ubuntu and VS Code
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

I was able to complete all of the steps from this post in seven minutes. That is
with a 300 Megabit internet connection. Depending on the speed of your computer
and Internet connection it should take between 5 & 15 minutes to do the 3 steps.
{:.note}


1. this ordered seed list will be replaced by the toc
{:toc}

## What we set up

- Windows Subsystem For Linux 2 (WSL2)
- The Linux distribution, Ubuntu 20.04 LTS
    - We will update & upgrade Ubuntu's packages
    - We will set git defaults
        - Git default name will be set to GitHub's Name
        - Git default name will be set to Github default email
        - Git default starting branch will be set to `main`
        - Git default editor will be set to VS Code
    - We will install Node Version Manager (nvm)
    - We will use NVM to download the current LTS version of Node
    - We will install the VS Code WSL Server
- Install as needed VS Code extensions


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

## Video for steps

<div class="youtube-container">
    <iframe
        src="https://www.youtube.com/embed/WBp70xPYk5Y"
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

1. From an administrative powershell session run the following command and it'll auto reboot when done.
   it'll enable, if not already:
   - Microsoft Windows Subsystem for Linux
   - Virtual Machine Platform
   - Hypervisor Platform
   - Windows developer mode

#### Install Script

    ```js
    // File: "install-wsl-1-and-reboot.ps"
    function start-script {
        clear
        $Shell = New-Object -ComObject ("WScript.Shell")
        $Favorite = $Shell.CreateShortcut($env:USERPROFILE + "\Desktop\wsl2.url")
        $Favorite.TargetPath = "https://aaronyoung.dev/blog/2021-02-24-wsl2-node-set-up/#update-wsl1-to-wsl2-and-install-ubuntu";
        $Favorite.Save()

        reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"
        dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
        dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
        dism.exe /online /enable-feature /featurename:HypervisorPlatform /all /norestart
        Restart-Computer
    }
    start-script
    ```

### update wsl1 to wsl2 and install Ubuntu

1. Launch powershell terminal as an administrator and run:

#### install script

```powershell
function Install-From-App-Store {
[CmdletBinding()]
param (
  [string]$Uri,
  [string]$Path = "."
)

  process {
    $progressPreference = 'silentlyContinue'

    $StopWatch = [system.diagnostics.stopwatch]::startnew()
    $Path = (Resolve-Path $Path).Path
    #Get Urls to download
    $WebResponse = Invoke-WebRequest -UseBasicParsing -Method 'POST' -Uri 'https://store.rg-adguard.net/api/GetFiles' -Body "type=url&url=$Uri&ring=Retail" -ContentType 'application/x-www-form-urlencoded'
    $LinksMatch = ($WebResponse.Links | where {$_ -like '*.appx*'} | where {$_ -like '*_neutral_*' -or $_ -like "*_"+$env:PROCESSOR_ARCHITECTURE.Replace("AMD","X").Replace("IA","X")+"_*"} | Select-String -Pattern '(?<=a href=").+(?=" r)').matches.value
    $Files = ($WebResponse.Links | where {$_ -like '*.appx*'} | where {$_ -like '*_neutral_*' -or $_ -like "*_"+$env:PROCESSOR_ARCHITECTURE.Replace("AMD","X").Replace("IA","X")+"_*"} | where {$_ } | Select-String -Pattern '(?<=noreferrer">).+(?=</a>)').matches.value
    #Create array of links and filenames
    $DownloadLinks = @()
    for($i = 0;$i -lt $LinksMatch.Count; $i++){
        $Array += ,@($LinksMatch[$i],$Files[$i])
    }
    #Sort by filename descending
    $Array = $Array | sort-object @{Expression={$_[1]}; Descending=$True}
    $LastFile = "temp123"
    for($i = 0;$i -lt $LinksMatch.Count; $i++){
        $CurrentFile = $Array[$i][1]
        $CurrentUrl = $Array[$i][0]
        #Find first number index of current and last processed filename
        if ($CurrentFile -match "(?<number>\d)"){}
        $FileIndex = $CurrentFile.indexof($Matches.number)
        if ($LastFile -match "(?<number>\d)"){}
        $LastFileIndex = $LastFile.indexof($Matches.number)

        #If current filename product not equal to last filename product
        if (($CurrentFile.SubString(0,$FileIndex-1)) -ne ($LastFile.SubString(0,$LastFileIndex-1))) {
            #If file not already downloaded, add to the download queue
            if (-Not (Test-Path "$Path\$CurrentFile")) {
                "Downloading $Path\$CurrentFile"
                $FilePath = "$Path\$CurrentFile"
                $Downloaded = $CurrentFile
                $FileRequest = Invoke-WebRequest -Uri $CurrentUrl -UseBasicParsing #-Method Head
                [System.IO.File]::WriteAllBytes($FilePath, $FileRequest.content)
            }
        #Delete file outdated and already exist
        }elseif ((Test-Path "$Path\$CurrentFile")) {
            Remove-Item "$Path\$CurrentFile"
            "Removing $Path\$CurrentFile"
        }
        $LastFile = $CurrentFile
    }
    "Time to download: "+$StopWatch.ElapsedMilliseconds/1000 + " seconds"

    $progressPreference = 'Continue'
    Add-AppxPackage -Path $Path\$Downloaded
    cd $Path
    rm $Downloaded
  }
}
function install-wsl2 {
    $progressPreference = 'silentlyContinue'
    Write-Host -ForegroundColor Green "Downloading wsl2"
    Invoke-WebRequest -Uri "https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi" -OutFile $env:USERPROFILE\Downloads\wsl_update_x64.msi
    cd $env:USERPROFILE\Downloads
    Write-Host -ForegroundColor Green "Installing wsl2"
    .\wsl_update_x64.msi -passive
    Write-Host -ForegroundColor Green "Waiting 10 seconds"
    Start-Sleep -s 10
    rm wsl_update_x64.msi
    wsl --set-default-version 2
    $progressPreference = 'Continue'
}

function Kick-It-Off {
    clear
    install-wsl2
    Write-Host -ForegroundColor Green "Starting download of Ubuntu, this may take a few minutes as its a sizeable download"
    Install-From-App-Store "https://www.microsoft.com/en-us/p/ubuntu-2004-lts/9n6svws3rx71" $env:USERPROFILE\Downloads
    start powershell{ubuntu2004.exe}
    cd $env:USERPROFILE\Desktop\
    rm wsl2.url
    exit
}
Kick-It-Off
```

### Setup Ubuntu

1. Open ubuntu 20.04 LTS and go through initial setup for the user & password
2. Run the below shell script, which must be copied into the terminal in one
   shot. While this is running, optionally move onto Installing
   [Windows Terminal](#windows-terminal)

#### Install Script

Feel free to read the comments that explain step by step what this script does.
{:.note}

```bash
# file: "update-and-setup.sh"
touch update-and-setup.sh && chmod u+x update-and-setup.sh && echo '
clear # clear the screen
# gather info
echo Please enter your full name for git:
read name
echo Please enter the email you wish to use with git:
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

# install wsl extension for vs code https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl
code --install-extension ms-vscode-remote.remote-wsl

# set up ubuntu for vscode

code . # open VS Code for first time to install wsl server for VS Code

# clean up after script

rm update-and-setup.sh # cleaning up
' >> update-and-setup.sh && ./update-and-setup.sh && exit # run the script then exit powershell's ubuntu
```
<!-- ### Windows Terminal

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
  Start with Tip 2 as we already did 1. -->



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
