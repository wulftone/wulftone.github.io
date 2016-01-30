---
layout: post
title: My OS Essentials
date: 24/02/2012
categories: desktop-manager linux keyboard
---

## Features I actually use in an operating system

Currently I use Linux Mint Debian Edition (LMDE) and it is quite nice and stable.  I had to update compiz a bit with some new packages--all of which were available via `apt-get`, but overall I am happy.  My coveted productivity features are all present in Gnome 2.30.2, and I'm writing this to record what are the bare essentials for my workflow.

DISCLAIMER: I currently develop Ruby and CoffeeScript applications, and mostly for the web, so my needs might be slightly different from yours.

## Keyboard shortcuts

Holy freaking cow.  I love keyboard shortcuts.  Having to move my hand to the mouse and use a time-sink method like my visual cortex's negative-feedback loop in order to do something like *aim* at a button to click it.  Man that sucks.  So, I use the keyboard.  The keyboard never moves, never changes, and is *fast*.  I know where it is and I can hit it with it with pinpoint accuracy.  Every.  Single.  Time.  So, when I tried out Gnome 3 and Unity at various times, I naturally tried to make all of my coveted keyboard shortcuts the same.  *Not happening*.  Throughout this post, you may hopefully notice that all of my keyboard shortcuts are executable with a single hand (left hand), and arranged to maximize comfort.

Here are some keyboard shortcuts I use regularly that I think should be available in some format on every OS I plan on using:

  * `ctrl+alt+t`: gnome-terminal.  I need it.  I like it.
  * `<super>+d`: show-desktop.  It's nice to clear the air once in a while.
  * `<super>+a`: gnome-do.  I don't know why anyone would launch an application in any other way.  Really, you clickers are nuts!
  * `<super>+e`: opens my home folder.  This provides me with a quick way to get at the files I most need.

## Desktop wall

I use keyboard shortcuts in a very specific way with the compiz desktop wall.  The wall is nice.  I don't ever need more than four desktops.  As a longtime Windows user, I could never see the point of using more than one desktop, since that isn't a standard feature of theirs--that is, until I started using Linux.  With the glory that is the wall, I can set up workspaces not based on some random `alt+tab` scheme, but on a *workspace role* theme.  Let me explain:

I have a few categories of activities I do during a workday:

  * Email - for business stuff, responses to bugs, etc.
  * Research - to learn how to code stuff
  * Coding - uhh, for money?.
  * Testing - so my money is consistent
  * Editing/verifying HTML/CSS - so people like to give me money
  * OS functions - so I am productive with my computer

That pretty much sums it up.  These can be further broken down into generalized categories in terms of application and role:

  * Internet stuff
  * Code/editing stuff
  * Command-line stuff
  * Viewing/editing my current project in a browser

This maps neatly to a quartet of workspaces--thus, *Desktop Wall*!

## Wall and Shortcuts

Combining the two, I can provide myself with a huge amount of desktop area and insane ease-of-use.  I set up my wall in a 2x2 format, and edit it so that it responds not to relative keyboard shortcuts (as is supplied by default) but by absolute shortcuts.  That is, `ctrl+alt+right` is useless to me, since I'll only ever be moving a distance of one workspace. Furthermore, I map the keys to physical locations on the keyboard that mimic the virtual location of the workspaces:

  * `ctrl+alt+q` is workspace 1
  * `ctrl+alt+w` is workspace 2
  * `ctrl+alt+a` is workspace 3
  * `ctrl+alt+s` is workspace 4

In a 2x2 grid, this means I don't have to remember how far away I am from any other workspace.  To make this scheme truly useful, however, each workspace must mean something.  Enter my four application roles:

  * Workspace 1 is for Internet
  * Workspace 2 is for Viewing/editing my current project in a browser (firebug!)
  * Workspace 3 is for Code/text editing
  * Workspace 4 is for Command-line stuff

Now, it is really easy to get from one "topic" or "role" to the next.  My brain quickly learned that `ctrl+alt+a` means "Code," and `ctrl+alt+w` means "Email/Research".

So why do I put Email and Research on the same workspace?  Most of the time, I only need two windows open on any given workspace, so using `alt+tab` never gets confusing (if you don't believe it gets confusing when you have 8 windows open on a single workspace, you must be a super-humanoid from Krypton).  This way, I always know that I'll get "Internet-y" things on workspace 1.

I can do the same thing with any other workspace.  So no workspace--at it's maximum convolutedness--ever gets more than three windows open at a time, and all the windows on that workspace have similar roles.  One thing to note is that all of my windows are maximized. I like space on my screen to read things without scrolling, and taking in large chunks of, let's say, test failures (wooooo!) at a single glance.  Also, it just feels nice.

## Transparency

Ok, so I lied.  *Sometimes* I open a terminal on my Internet workspace, and when I do, I really need transparency (they should call it translucency, but I digress....).  The reason is that when I'm looking up some obscure bit of computery junk that I need to type into the command line, it's often much simpler to have a transparent window open on top of the browser that I can then see through to copy whatever command I'm trying to execute.  Why not just copy/paste it?  Often these commands are generalized, so you need to actually put your own username or folder name or whatever into the command so it runs well.  Thus, it is faster to simply type it out myself.

I also like to have transparency on my code window and terminal simply so I can actually enjoy whatever my current desktop background is, but that is simply icing on top of the linux cake.

## Laptop function keyboard controls

What does that mean?  Just the regular brightness, volume, mute, wifi, touchpad, battery status, etc. controls.  They're important.  I code in a varitey of lighting situations, and I want it to be comfortable and long-lasting (that's what she... well you know).  I lied again: I don't care if these are executable with one hand, they are not often used... but when they are needed, they must be available!

## Package manager

I don't know why every OS on the planet doesn't have a simple package manager.  The thought that Windows and Mac users have been installing stuff from CDs, DVDs, and a variety of online sources is baffling.  Linux has had it done right for a long time: one centralized repository for everything you could ever want your computer to do.  Duh.

## Menu/start button/application list

Everyone needs a simple list of what programs are on their computer.  Obfuscating this is a bad idea.  See [Android](http://www.android.com/) for a wonderful list.

## Standard internety and office-y things

Firefox, Chrome, LibreOffice, PDF viewer, image viewer, gimp, inkscape.  People send me docs, pdfs, gifs, jpgs, svgs, ppts, and whatever else.  It's necessary to have things that can deal with a wide variety of file formats.  The browser requirement should be self-evident.

## Sublime Text 2

I love this editor.  [Download](http://www.sublimetext.com/2) it now.  It's fast, configurable (everything is configured by editing easy-to-understand JSON), and has a growing lineup of awesome plugins--also written in python.  It was easy to develop [my own plugin](https://github.com/wulftone/sublime-text-2-quick-file-renamer), and I don't even know python!  It has syntax highlighting, of course, but you can use any TextMate bundle to provide you with a multitude of language, color, and snippet options, and again, it's easy to just make your own.  It also has syntax checking, via the Sublime Linter plugin, for any language that has a command-line syntax checking tool (like `ruby -c`, `coffee -l`, or even use html-tidy with the `tidy -eq` command)--and once again, if support isn't there, you can make your own without much fuss (like I did for tidy).  Nice.

## Conclusions

Hopefully this post has shown you, at the least, what my take on an efficient workspace is.  I don't need to monitor ten servers at once, so perhaps a [fancy window manager](http://xmonad.org/) isn't what I need, but you might.  In the most selfish of terms, this post serves to remind me what my current requirements are, so when I start fiddling with [Cinnamon](http://cinnamon.linuxmint.com/), I don't forget what is important to my workflow.  LMDE gives me all of the above things right out of the box, with a minimum of fuss.  On top of that, it's a rolling distribution, so only updates happen intead of reformat/reinstalls.

I'll probably update this post once my tastes change or I remember something else, but this environment is really good for me, and it might be good for you.
