---
title: New macOS Development Setup
header:
    image: /assets/images/tianyi-ma-WiONHd_zYI4-unsplash.jpg
    caption: Photo by [Tianyi](https://unsplash.com/@tma?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/macbook?utm_source=unsplash&amp;utm_medium=referral&amp;utm_content=creditCopyText)
date: 2020-12-06T16:35:00-00:00
categories:
    - blog
tags:
    - macOS
    - development
---

# Things to install on a new installation of macOS

The following is a personal guide on what I setup for my development machine.

When setting up a new installation of macOS remember to pick a username that makes sense such as `fvelazquez` and avoid a pseudonym like `kikin81`. This will be helpful when `ssh` into it.

The first thing to consider is to create a backup of your hidden files/folders that you want to keep such as `.zshrc` for zsh and `.ssh` for ssh keys.

## Homebrew

[Homebrew](https://docs.brew.sh/Installation) is a must have for development. With it we can install `python`, `ruby`, `git` and other development essentials.

### prerequisites

Homebrew requires Command Line Tools which you can get by executing the following command in terminal

```
$ xcode-select --install
```

Alternatively you can download the latest Xcode version from the [App Store](xcode-select --install).

### installation

The following command will download and install homebrew

```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

```

### recommended tools

After homebrew is finished setup I then install the following software

* git (_note:_ macOS already comes with git. Homebrew git will provide the latests version)
* python (_note:_ macOS comes with Python but installing libraries would require using `sudo`. Installing via homebrew will get the latest version as well as let you install dependencies on home directory, not sudo required.)
* rbenv (_note:_ macOS comes with ruby out of the box as well, but we will get the latest from homebrew.)
* nvm: lets you manage node environments

```
$ brew install git python rbenv nvm
```

## oh-my-zsh

Next I replace the bundled bash shell with zsh using [oh-my-zsh](https://ohmyz.sh/)

```
$ sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### zsh plug-ins

<p align="center">
  <img alt="Zsh with auto complete" src="/assets/images/iterm.gif" width="980px">
</p>

Recommended zsh plug-ins are [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) and [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

First install via brew:

```
$ brew install zsh-syntax-highlighting
$ brew install zsh-autosuggestions
```

Edit `.zshrc` file:

```
source $ZSH/oh-my-zsh.sh
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```

## Android development

Download the latest stable version of Android studio via the [releases page.](https://developer.android.com/studio/releases)
Alternatively you can install it via `homebrew cask`

```
$ brew install --cask android-studio
```

Add the sdk tools (`adb`) to your path:
```
# ~/.zshrc

export ANDROID_HOME="PATH_TO_STUDIO_/Library/Android/sdk/"
export PATH="/usr/local/bin:$PATH:$ANDROID_HOME"
```

Install [vysor](https://www.vysor.io/) for projecting a device to macOS

```
$ brew install --cask vysor
```

## Miscellaneous applications

Finally, this is a list of my most used macOS applications which can be found in the App Store.

* [1password](https://apps.apple.com/us/app/1password-7-password-manager/id1333542190?mt=12)
* [Alfred](https://www.alfredapp.com/)
* [aText](https://www.trankynam.com/atext/)
* [Bartender](https://www.macbartender.com/)
* [iTerm2](https://iterm2.com/)
* [Kaleidoscope](https://kaleidoscope.app/)
* [Navicat](https://www.navicat.com/en/download/navicat-premium)
* [Paw](https://paw.cloud/)
* [Pixelmator](https://www.pixelmator.com/pro/)
* [Sublime Text](https://www.sublimetext.com/)
* [Transmit](https://panic.com/transmit/)

