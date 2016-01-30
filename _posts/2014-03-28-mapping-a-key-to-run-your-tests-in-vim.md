---
layout: post
title: Mapping a key to run your tests in vim
date: 28/03/2014
categories: vim
---

I stole this right from Gary Bernhardt, and I just wanted to remember it in a clear place:

    :map ,t :w\|:!mocha --compilers coffee:coffee-script %<cr>

It's really nice to press two keys quickly in succession to save your file and run your tests, *and* view the test output.  Other text editors (like my beloved Sublime Text) don't automatically switch views so you can watch your test run when you save the file.  I could program my keyboard to do that, but not many people have a programmable keyboard, and even then it'd be a key combination (key-chord), not a finger-friendly rapid `,t` succession.  Since I do this about, say, 1,000,000 times a day, mapping a key like that in vim could potentially save keystrokes in the long run if you run the same spec (or all specs) a bunch of times.

It would be nice to have an automatic way to set this up so you don't have to map it to the current file all the time.  Time to do some more investigation.
