---
layout:	post
title: "Make your own dotfiles repo today"
date:  2020-10-31 12:15:00 -0800
categories: misc
--- 

# **Introduction**
Dotfiles, we all love them. If you're anything like me and use the command line a lot you surely use dotfiles. Things like your vimrc, zshrc, tmux config, etc are all dotfiles.

# **So why should you make a repo with your dotfiles?**
A github repo including all your dotfiles makes it easier to deploy them later, also makes it possible for other people to use them. I get asked occasionally, "how did you make your setup look like that?" and now I can just direct them to my dotfiles on github.

# **Setting up this github repo**
Quite easy actually, I wrote a deployment script, you should too for easy deployment, and you can use my repo as a template, just please credit me. :)
[dotfiles repo](https://github.com/lyra64/dotfiles)

# Things to know when scripting your dotfiles install
* ALWAYS declare your shell at top in this way to make it *nix agnostic
```
#!/usr/bin/env bash
```
* DO NOT make one big script, split it up into multiple scripts for easy editing and partial deployment.
* Always Comment your scripts...this shouldn't have to be said but it's being said.
* If you use distro/os specific commands, be sure to add detection so those commands don't run on an unsupported OS.
* The more complex the script is the more room for mistakes, this is one reason why I recommend breaking it up into multiple little scripts.
