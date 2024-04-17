---
layout: ../../layouts/MarkdownLayout.astro
pubDate: 2024-04-17
title: "Setting up my Dotfiles"
subtitle: ""
tags: ["dev", "dotfiles", "yadm"]
---

Having recently transfered over to a mac for my personal machine, it hasn't been long since I've gone through the process installing and setting everything up the way I want it. Now that I am going to be swtiching over to a mac on my work machine, I didn't want to go through the whole process again. This is where dotfiles come into play.

### What are Dotfiles

In the terminal, if you run `ls`, you will see a list of files and directories in the current directory. If you add the `-a` flag (`ls -a`), you'll notice (most likely depending on which directory you are in) some extras appear that weren't previously returned that are prefixed with a `.`.

These are configuration files that applications use. The most common one that everybody on a mac should have is the `~/.zshrc` file which is the configuration file for the zsh shell. If you want to try this out, put `echo "Hello world!` into this file, fire up a new terminal, and ensure the message is printed at the start.

### How Can You Use Dotfiles to Help Setup a New Machine

As Dotfiles configure your machine and apps to work the way they do, by bringing them over to a new machine, you can bring your apps, experience and workflows over too.

So just copy and paste across? Yes, in theory that works, but what happens if your machine breaks? All that configuration, gone. To avoid this, you can store dotfiles remotely e.g. using GitHub.

### Approaches to Managing Dotfiles

Having looked into it over the last few days, the most common approach I've seen is creating a `dotfiles` directory, putting the files in there, and then creating symlinks from the home directory back to the appropriate files. You can then use version control as you would with any other directory in `dotfiles`.

The other approach I've seen is using a dotfiles manager like [yadm](https://yadm.io/) (yet another dotfiles manager), which allows you to keep your files in the home directory and whilst maintaining the Git workflow. 

### My Setup

I chose to use yadm to manage my dotfiles, mostly because it was personally recommended to me, but also because I didn't particularly like the idea of having to manage a bunch of symlinks to a different directory. If you are interested, here is a [link to yadm's overview page](https://yadm.io/docs/overview) which explains more around the motivations for it's creation and reasons to use it.

Installation is simple with Homebrew `brew install yadm`, initialise it with `yadm init`, add dotfiles with `yadm add <file>`, and finally commit with `yadm commit`. If you haven't already noticed, yadm builds upon git so you can generally do `yadm <git command>`.

### Setting up Yadm to Auto Install Apps

Yadm has this idea of the `Bootstrap` file which can be run by `yadm bootstrap` or when a yadm repository is cloned, the user will be prompted to run it. Pairing this with a `Brewfile`, I've now got it setup so that on my new machine, I'll run the bootstrap file and everything will be auto installed.

A `Brewfile` is essentially just a list of all the tools and apps you have installed using Homebrew. To get a Brewfile, run `brew bundle dump`. To install the Brewfile, run `brew bundle --global`. Here is a [link to an example Bootstrap file](https://arc.net/l/quote/atjrvqzg) that does this all for you.

### Useful Links

[Yadm docs](https://yadm.io/), [my personal dotfiles](https://github.com/RowMur/dotfiles)
