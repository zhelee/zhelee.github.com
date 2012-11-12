---
layout: post
title: "Production Rails"
date: 2012-11-12 07:24
comments: true
categories: 
---
** The tools **

1. Nginx
2. Passenger
3. Memedcache
4. Capistrano
5. RVM
6. Git
7. Linux (We are using ubuntu 12.10)

** Ruby and Rails **

1. Ruby 1.9.3
2. Rails 3.2.8

** Database **

1. PostgresSQL 9.1

** Some linux knowledge **

1. How to start, stop, restart a linux service
2. How to let a service start when linux boot up
3. How to create a user in linux
4. SSH

** The big overview for deploying an rails production application **

1. Use Git as the source control system. Before deploying the application to the server, forbid any code change to be pushed to the Git server;
2. Check the code through Git in CI server, and run the test, if tests are passed, go to step 3;
3. Run capistrano to deploy the application, in the capistrano deploying file, there are the following steps:
    1. set the status of the current production application server to be **maintenance**;
    2. deploy the code
    3. run the assets compile
    4. run the database migration
    5. set the status of the current production application server to be **running**;
    6. if the new code fails to run, use capistrano to rollback the previous version;
    7. __**something about cache**__

----------------
## Create a depoyer user

** 1. Why do we need to create a new user to deploy the application **

When we deploy a proudction rails, it's a little different from its development mode and it's better to let a specific user with specific rights to do the deployment to avoid mistakes, and it's easier to log the deployments.

** 2. How to **

In ubuntu, we use the following command to create an user with specific home directory and a password: `sudo adduser deployer`, and follow the prompted instruction, it's easily to create a user called deployer with a home directory /home/deployer.

## Install the softwares

** 1. Install RVM, Ruby, Rails, PostgresSQL ** 

There are thousands of tutorials to install these things, just remember the version we mentioned above. And there are some points which could help if there got some problems:

1. RVM for single user or system. I recommend to install as system on a server, because ruby could be used for other users, for example maintenance users. and the user wants to user rvm is needed to be added to the rvm group;
2. __** For PostgresSQL. When you first install PostgresSQL, it's needed to configure `/etc/postgres/9.1/main/pg_hba.conf` to let PostgresSQL to be able to be connected.**__;

** 2. Install passenger **

Installing passenger is so easy, just the following 2 steps:

1. `gem install passenger`
2. `[rvmsudo] passenger-install-nginx-module`
