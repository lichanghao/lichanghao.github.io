---
title:             "Configure my iTerm2 terminal on MacOS"
date:              2023-11-05
permalink:         /posts/2023/11/05/configure-iterm2-terminal
tags:              Note
---

This post is a note on configuring the iTerm2 terminal on my new computer.

## iTerm 2

iTerm2 is a powerful and highly customizable terminal emulator for macOS. It serves as a robust replacement for the default macOS Terminal application, offering a wide range of features and enhancements that make it a popular choice among developers, sysadmins, and power users. iTerm2 not only provides a clean and intuitive interface for running command-line tasks but also boasts advanced capabilities, such as split panes, multi-tab support, extensive color schemes, and a robust scripting API. Its rich set of features includes support for various shells, dynamic profile switching, mouseless copy, and many more. Whether you're a casual user or a seasoned developer, iTerm2's flexibility and feature set make it a valuable tool for improving your terminal experience on macOS.

Of course, the above introduction is written by ChatGPT. The primary terminal given by MacOS is okay but minimal. So normally MacOS user would choose another terminal for better apparance and extensibility. iTerm2 is one of the most famous choice.

## Install iTerm2

iTerm2 is free to download and very straightforward to install. 

[Official download link](https://iterm2.com/downloads.html)

## Configure iTerm2

The vanilla iTerm2 does not seems very attractive. Users need to configure this terminal based on their tastes and needs.

The first thing I would do is activating the shortcuts for hotkey window. Following [this blog](https://medium.com/@gveloper/how-to-set-up-the-hotkey-window-in-iterm2-637d86ac70ff#:~:text=To%20enable%20the%20hotkey%20window,or%20a%20double%2Dtap%20key.) to setup the keyboard shortcuts.

The second thing. By default is a pain to jump between words and lines in iTerm2 using the keyboard combination `option+left` or `option+right`. Following [this blog](https://medium.com/@ThisIsUpen/how-to-jump-between-words-in-iterm2-3c22eb5a25ef) to recover the keyboard setting to a more intuitive way.

The third thing, configure the apparence. The transparency, font, theme can be changed in the preference settings. The setting is straightforward and based on your personal taste.

## Download zsh

Install the `zsh` shell by `homebrew`:

```zsh
brew install zsh
```

Of course, if you haven't install `homebrew` yet, use this one-liner to install:

```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

The user-defined configuration for zsh is recorded in `~/.zshrc`.

## Install Oh-My-Zsh

Install `Oh-My-Zsh` by:

```zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Then you can add plugins by modifying `~/.zshrc`:

```zsh
vim ~/.zshrc
```

```zsh
# Which plugins would you like to load? (plugins can be found in ~/.oh-my-zsh/plugins/*)
# Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(
  git
)
```

Or adding alias:

```zsh
vi ~/.zshrc
```

```zsh
# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"
alias dkps="docker ps"
alias dkst="docker stats"
alias dkpsa="docker ps -a"
alias dkimgs="docker images"
alias dkcpup="docker-compose up -d"
alias dkcpdown="docker-compose down"
alias dkcpstart="docker-compose start"
alias dkcpstop="docker-compose stop"
```

## Install other plugins

I installed three additional plugins (the usage is self-explanatory from their name),

```zsh
brew install zsh-syntax-highlighting
brew install zsh-autosuggestions
brew install powerlevel10k
```

and manually add then into `~/.zshrc`:

```zsh
source /opt/homebrew/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /opt/homebrew/share/zsh-autosuggestions/zsh-autosuggestions.zsh
source /opt/homebrew/share/powerlevel10k/powerlevel10k.zsh-theme\
```

Now, with this one-hour effort, my terminal seems more powerful and better-looking! 