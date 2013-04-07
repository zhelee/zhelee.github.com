---
layout: post
title: "production rails - deploy"
date: 2013-04-07 23:01
comments: true
categories: 
---
Capistrano deployment use ssh to deploy. In most of the time, you will first push your code to a vcs system. Let's say we are using git and github. The deployment process is like this:

1. ssh into server
2. checkout code from github

We don't want to input password whenver we want to deploy. So we need to copy ssh public key to the server. We Can use `ssh-copy-id username@server` to simplify this step. after this step we can ssh into server without input any password.

When checking code from github it needs the ssh key in our develop computer, so here we need to setup ssh-forward.

1. add `ssh_options[:forward_agent] = true` to `config/deploy.rb`
2. be sure a `ssh-agent` enabled `bash` is started. if not, use `exec ssh-agent bash` to start
3. be sure `ssh key` is added, use `ssh-add -l` to check your ssh key is added, if not, use `ssh-add .ssh/your-key` to add it

by doing the above steps, you can use `cap deploy` to checkout code from github in the server whitout add another ssh key for the server to github.