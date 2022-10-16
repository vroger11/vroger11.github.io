---
layout: post
comments: true
title:  "Improve your bash terminal experience"
ref: improve_default_bash
date:   2020-01-29 17:30:00 +0200
categories: blog dev
category: blog
lang: en
---

This post is focused on bash configuration.
I use bash as it is the default shell in most Linux distributions.
I use it daily and after some tweaks it better suits me.
In this post I will describe my main usage and how I configure it.

# Using the default Ubuntu bashrc configuration
The default configuration in Ubuntu is nice and I use it as a base for my default configuration.
First, let review the basic features it offers.

## Default completion
Completion in terminal is essential to gain productivity.
In bash it is by default activated by pressing the Tab key.
Ubuntu enables most of the useful completions by default, such as command options completion (`apt ins<Tab>` gives you `apt install `) or folders/files completion.
Nevertheless, the default completion behavior seems weird to me.
If you press multiple times the Tab key you end up with the same listing showed to you multiple times.
Such as in the following clip:

![Default completion output](/assets/images/bash_config/default_tab_behavior.gif)
In the next section, we will see how I configure it to be more consistent and useful.

## Recursive search

To get previous typed commands in a terminal you can use the upper arrow key.
But, if you know a part of the command you want to reuse (and it is high in your bash history) you can use the recursive mode.
To enter in recursive mode, you have to press CTRL+R keys and type the first letters of your choice (such as `ssh`).
The result is the first command matching your search in your bash history.
If you want the next matching results in your bash history, you just have to press the CTRL+R key combination.
I appreciate this behavior, so I let it as is in my configuration.

## Alerts

It is a handy alias to send notification and it is in the default Ubuntu bashrc.
To have this functionality working, you have to install the libnotify library:

```bash
sudo apt install libnotify-bin
```

It sends an alert to your desktop environment when the command is executed.
I often use it after logical and while I prototype light models.
Here is an example you can try at home to better understand its usefulness:

```bash
sleep 3 && alert
```

# Personalize your bashrc

First, I advise you to do a backup of your bashrc.

```bash
cp ~/.bashrc ~/.bashrc_back
```

## Personalize the prompt
It is a fancy part, but as you will see the prompt on every line you type I advise you to personalize it.
To do so, you have to change the PS1 variable.
Mine is as follows:
```bash
PS1="\[\033[38;5;11m\]\u\[$(tput sgr0)\]\[\033[38;5;15m\]@\H:\[$(tput sgr0)\]\[\033[38;5;32m\]\w\[$(tput sgr0)\]\[\033[38;5;15m\]\\$ \[$(tput sgr0)\]"
```

You have to put it under the following line in your `~/.bashrc`:
```bash
if [ "$color_prompt" = yes ]; then
```
You have to replace the default PS1 value by yours.

Create the PS1 value can be long as it uses a complex syntax.
If you are lazy like me, you can use [ezprompt](https://ezprompt.net/).
It is an interactive website to create PS1 value.

## PATH modifications

I like to put some scripts in my home directory (to not mess with my system).
So I add a bin folder under my home directory and the following line in my bashrc:

```bash
PATH=~/bin:$PATH
```

## Completion
As you saw in the previous section, the default completion mode is not fantastic.
By default, bash bind the Tab key with the `complete` command.
Fortunately, bash ships with an alternative: menu-complete.
menu-complete makes tab cycle through suggestions after listing them.
Here is the same example shown in the previous section but using menu-complete:

![menu-complete behavior](/assets/images/bash_config/improved_tab_behavior.gif)

To have this behavior, you have to add the following lines in your .bashrc:

```bash
# tab cycle through commands after listing
bind '"\t":menu-complete'
# completion results configuration
bind "set show-all-if-ambiguous on"
bind "set completion-ignore-case on"
bind "set menu-complete-display-prefix on"
# colored completion, for folders and others
bind 'set colored-stats on'
```

Furthermore, menu-complete allows doing backward cycling (in case you miss the desired output).
Unfortunately, I didn't manage to bind the Shift-Tab key combination to this behavior.
Instead, I bind this functionality with the ² key.
Which is only handy for AZERTY keyboards (sorry for the others).
If anyone wants to help me on this, feel free to contribute I will modify this post with your solution (and I will cite you of course).

```bash
# reverse tab cycle
bind '"^[[Z":menu-complete-backward' # Shift+Tab, but it is not working in Konsole.
bind '"²":menu-complete-backward' # for azerty keyboard
```

# My full configuration file

You can find my full bashrc [here](https://github.com/vroger11/vroger11-configs/blob/master/bash/bashrc).
It contains more customizations, like gem configuration and miniconda initialization.
I tested it under Konsole terminal emulator under Ubuntu 18.04 with two username.
Nevertheless, be careful of what you do with it.
I do not provide any warranty.

# Sources

* [Bash man pages](https://linux.die.net/man/1/bash)

Hope it helps some of you.

Cheers, Vincent.
