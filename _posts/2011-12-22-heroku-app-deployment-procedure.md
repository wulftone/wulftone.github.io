---
layout: post
title: Heroku app deployment procedure
date: 22/12/2011
categories: heroku
---

## Process for deploying app on heroku

1. sudo gem install heroku
2. heroku create app_name
3. git init
4. git add ( add the files present in the app, see the files by ls command)
5. git commit -m "commit the app"
6. cd ~/.ssh (see that ssh directory exists or not)

        if exists then
          $ ls
          $ config id_rsa.pub
          $ id_rsa known_hosts
          $ mkdir key_backup
          $ cp id_rsa* key_backup
          $ rm id_rsa*--
        if "not exists" then
          $ ssh-keygen -t rsa -C "your email"

7. heroku create
8. heroku keys:add
9. git push heroku master
10. heroku rake db:migrate.

To see the error in opening the site in the browser

    heroku logs --app app_name (run that command on the terminal)

if `git push heroku` master gives following error

    ------Agent admitted failure to sign using the key.
    Permission denied (publickey).
    fatal: The remote end hung up unexpectedly

then run this on prompt

    ssh-add ~/.ssh/id_rsa
