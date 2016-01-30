---
layout: post
title: Multiple heroku accounts
date: 22/12/2011
categories: github heroku ssh
---

## Heroku Accounts Plugin

David Dollar of Heroku recently released an official Heroku plugin called [heroku-accounts](https://github.com/ddollar/heroku-accounts). With this plugin, we can switch Heroku accounts automatically.

To get started, we first install the plugin:

    heroku plugins:install git://github.com/ddollar/heroku-accounts.git

The installation process will download the plugin from github and save it to the ~/.heroku/plugins directory. Now, we can setup each of our Heroku accounts with add command:

    heroku accounts:add work

and for our personal account, we can run:

    heroku accounts:add personal

The add command will ask you for your Heroku email address and password for each account. The plugin will maintain your account credentials in the ~/.heroku/accounts folder in your home directory. (Passwords are not saved in plain text)

To assign a project to a specific Heroku account, we run the following command in the project root:

    heroku accounts:set personal # or work

This will assign a Heroku account to the project by adding an 'account' variable to the project's git config file.

## Celebrate

Hooray! Now we can switch Heroku accounts automatically. Awesome.

# Heroku Accounts README.md

Helps use multiple accounts on Heroku.

## Installation

    $ heroku plugins:install git://github.com/ddollar/heroku-accounts.git

## Usage

To add accounts:

    $ heroku accounts:add personal
    Enter your Heroku credentials.
    Email: david@heroku.com
    Password: ******

    Add the following to your ~/.ssh/config

    Host heroku.personal
      HostName heroku.com
      IdentityFile /PATH/TO/PRIVATE/KEY
      IdentitiesOnly yes

Or you can choose a fully-automated approach:

    $ heroku accounts:add work --auto
    Enter your Heroku credentials.
    Email: work@example.org
    Password: ******
    Generating new SSH key
    Generating public/private rsa key pair.
    Your identification has been saved in ~/.ssh/identity.heroku.work.
    Your public key has been saved in ~/.ssh/identity.heroku.work.pub.
    Adding entry to ~/.ssh/config
    Adding public key to Heroku account: work@example.org

To switch an app to a different account:

    # in project root
    heroku accounts:set personal

To list accounts:

    $ heroku accounts
    personal
    work

To remove an account:

    $ heroku accounts:remove personal
    Account removed: personal

Set a machine-wide default account:

    $ heroku accounts:default personal

To clone a git repository from Heroku, change 'heroku.com' to the Host of the desired account defined in your .ssh/config:

    $ git clone git@heroku.work:repository.git
