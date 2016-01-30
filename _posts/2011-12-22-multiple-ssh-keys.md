---
layout: post
title: How to setup several GitHub accounts on multiple machines with separate SSH keys
date: 22/12/2011
categories: github ssh
---

## Problem

So you've got a personal GitHub account, one for one client, one for another and you want to keep them all separate. How do you setup your box to clone private repositories based on which account you're using? Here's the simple guide on how to do this

## Assumptions

I have set this up on Linux (Ubuntu 11.10) and Windows, but only via the Git Bash and/or Cygwin. This post will not cover how to setup and where to store SSH keys for Windows or Mac, though Mac should be fairly similar to Linux.

## Setup first SSH key

I'm not going to go into depth on how to setup SSH keys, you can read more about that here. However, I will show the steps I used to get there. For first-account: (please replace "first-account" with your GitHub account)

1. Change to your home .ssh folder

        cd ~/.ssh

2. Setup your RSA key

        ssh-keygen -t rsa -f ~/.ssh/first-account-rsa.pub -C "first-account@example.com"

3. After following the directions, you should get something that looks like:

        The key fingerprint is:
        00:ff:00:cc:13:00:bb:ll:aa:hh

        The key's randomart image is:
        +--[ RSA 2048]----+
        |     .+   +      |
        |       = o P .   |
        |        = * *    |
        |       o = +     |
        |      o S .      |
        |     o o =       |
        |      o . E      |
        |                 |
        |                 |
        +-----------------+

4. Open your GitHub.com account setup page, click on the "SSH Public Keys" tab.
5. Create a new SSH key and copy the contents of `~/.ssh/first-account-rsa.pub` and place into the new text field. Name it something unique so you can remember which machine you're using for your account. If you plan on each of your accounts needing access, open each of those accounts and add your SSH key to each account. This isn't very common, so only do this if you need to.
6. To test, type in:

        ssh -t git@github.com

As long as you do not see "access denied", you're good. You might see "failed on channel 0", this is ok.

## Adding a second account

Now you should have your first account working, let's setup and add the second account.

Repeat process 1-5 and stop (using the next account)
Create a file named "config" in your `~/.ssh` folder

    echo '' > ~/.ssh/config

Use your favorite editor to add the configuration information for the SSH client to determine which RSA key to use:

    # first-account GitHub account
    Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/first-account-rsa # Note, this is the private key matching the .pub

    # second-account
    Host github-second-account  # NOTE: The host is NOT github.com, more on this further down
    HostName github.com
    User git
    IdentityFile ~/.ssh/second-account-rsa # Private key for the second account

    # repeat for the third and so on

Save the file and your configuration is done

Let's look at the config file in a bit more detail. The first account is the base github.com host name, and you typically see a git URL as `git@github.com:user/project.git`. With your first account, you will leave this the domain the same**.

NOTE: This doesn't stop you from modifying your `~/.ssh/config` file and instead of using "github.com" as your default host, setting the value to "github-first-account".

Now, for your second account, you will need to modify the URL you are using to clone the repository. This is done by changing the domain from "github.com" to "github-second-account". For example: `git@github-second-account:user/project.git`

## Summary

You should now have the setup to clone and modify code for multiple accounts on the same machine. Repeat this process for each machine you would like to make available to GitHub.
