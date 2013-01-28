---
layout: post
title: "Production Rails - Overview"
date: 2013-1-27 07:24
comments: true
categories: 
---
** The tools **

1. Nginx
2. Unicorn
3. PostgreSQL
4. Capistrano
5. RVM
6. Git
7. Puppet
8. Vagrant
9. Linux (We are using ubuntu 12.10)

** Ruby and Rails **

1. Ruby 1.9.3
2. Rails 3.2.11

** Database **

1. PostgresSQL 9.1

** Some linux knowledge **

1. How to start, stop, restart a linux service
2. How to let a service start when linux boot up
3. How to create a user in linux
4. SSH

----------------
## Create a depoyer user

** 1. Why do we need to create a new user to deploy the application **

When we deploy a proudction rails, it's a little different from its development mode and it's better to let a specific user with specific rights to do the deployment to avoid mistakes, and it's easier to log the deployments.

** 2. How to **

In ubuntu, we use the following command to create an user with specific home directory and a password: `sudo adduser deployer`, and follow the prompted instruction, it's easily to create a user called deployer with a home directory /home/deployer.

## Install the softwares

** 1. Install RVM, Ruby, Rails, PostgresSQL, Vagrant, Puppet** 

There are thousands of tutorials to install these things, just remember the version we mentioned above. And there are some points which could help if there got some problems:

1. RVM for single user or system. I recommend to install as system on a server, because ruby could be used for other users, for example maintenance users. and the user wants to user rvm is needed to be added to the rvm group;
2. __** For PostgresSQL. When you first install PostgresSQL, it's needed to configure `/etc/postgres/9.1/main/pg_hba.conf` to let PostgresSQL to be able to be connected.**__;
3. Vagrant and Puppet are written by ruby, so we can install them by `gem install [vagrant|puppet]`

** 2. Unicorn **

Unicorn is a great app server for rails, it gives the oppertunity to do a zero downtime deployment

1. `gem install unicorn` or add `unicorn` to `Gemfile`

** 3. Set up Vagrant **

We will talk about vagrant in another post, at this time you can check its offical website, [vagrantup.com](http://vagrantup.com)

** 4. Set up Puppet **

We use Puppet to provision the server environment, to keep the same softwares and configurations between the virtual machine and the real server, we will talk about puppet in another post too, at this time you can the cookbook of puppet, [puppet cookbook](http://www.puppetcookbook.com)

** 5. Capistrano **

We will use capistrano to deploy the application. and because the env configration changes depends on the application itself, we use capistrano to set nginx and unicorn configurations during the deployment process.

** The big overview for deploying an rails production application **

1. Use Git as the source control system. Before deploying the application to the server, forbid any code change to be pushed to the Git server;
2. run the unit test, if tests are passed, go to step 3;
3. Use Vagrant to set up a staging server, in this step, code will be deployed to this staging server, if the deployment is successful, go to step 4;
4. Run capistrano to deploy the application, in the capistrano deploying file, there are the following steps:
    1. deploy the code
    2. run the assets compile
    3. run the database migration
    4. restart process if needed
    4. if the new code fails to run, use capistrano to rollback the previous version;

---------------------------
In the following posts, I will talk every topic in a seprated post. and here will be the list of these posts.