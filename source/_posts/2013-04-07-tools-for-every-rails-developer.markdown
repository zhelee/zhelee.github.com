---
layout: post
title: "tools and tips for every rails developer"
date: 2013-04-07 21:19
comments: true
categories: 
---
Being a rails developer for almost 2 years, I found a lot of tools for easily developing rails application including ruby gems, editors, browser extensions, frameworks. Here I list some tools where I think should be use in every rails application development.

### 1. Zeus
Whenever after you start rails, you want to start rails console, generate a model or execute rake task, as it needs to load the whole rails application instance, it will take some time, if it is a big application, for example, it has 30 models, it will take so long time. Use Zeus, it will preload the application, so every command will be executed by using this application instance. Check the official github documention. [Zeus](https://github.com/burke/zeus). 
### 2. Shell prompt
![shell prompt](https://a248.e.akamai.net/camo.github.com/f77fc2ea241f3bf84256dde18342c5e25ab352c3/687474703a2f2f692e696d6775722e636f6d2f42726b6d302e706e67)

As a rails developer, we use terminal a lot. the default shell prompt is just the current user, current host and current folder. We want to know the current git status, wether we have changed something, or is there any conflicts, which branch we are working on. There are 2 github repository for easily integrate these kind of prompt, if you use bash, you can use [Bash-it](https://github.com/revans/bash-it), if zsh, you can use [Oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)
### 3. Better Errors And Rails Panel
Default rails error page does not show too mush information for you to debug. Use [Better errors](https://github.com/charliesome/better_errors) can show you more information, and provide an interactive console to dig into the code.
![better errors](https://a248.e.akamai.net/camo.github.com/f05d967fb90cbe3e686ad794062c2151f7ee19a5/687474703a2f2f692e696d6775722e636f6d2f7a594f58462e706e67)

[Rails Panel](https://github.com/dejan/rails_panel) just let you leave rails development log, just focus on the browser itself, it contains a rubygem and google chrome extension, in the google chrome extension, there is an panle show all the development information.
### 4. Short Cuts
Everyday you type `rails s` or `rails s -p 3010` to start the rails server, have you think to type less, for example `rs` to `rails s` or `rails s -p 3010`, `rg` to `rails g`, `rc` to `rails c`. Simply add all of these short cuts to your .bashrc to set your own customized command. Not only rails itself, you can have other short cuts like `git st` to stands for `git status` in .gitconfig
### 5. Guard-Livereload And Guard-Rails
![Guard](https://a248.e.akamai.net/camo.github.com/dfda2a47ddefe982a455c391c6788fa62a5c70d9/687474703a2f2f696d673531352e696d616765736861636b2e75732f696d673531352f313335382f677561726469636f6e2e706e67)

You change a css color and go back to the browser, and refresh it to get the result. In a rails application, you dont need to do this. If you have a bigger monitor or 2 monitor, you will love Guard-Livereload. Simplely add Guard-Livereload, and install livereload extension in google chrome or firefox. after setup the Guard file, whenever you change the view, css, or images, the browser will refresh automatically. You can not miss it if you are developing the view part. 
Whenver you change a file which need to restart rails server, you will kill the rails first, and use `rails s` to start it again. Use Guard-rails you dont need to do this, it will restart rails automatically, you just need to config the Guardfile to add lines to specify which file changement need to restart rails app.
### 6. RSpec, Spork, Guard-RSpec, Guard-Spork
RSpec is a very good testing framework in Ruby, it should be used in every ruby project, of course in rails. As whenver running the tests need to load the rails application, it could need a long time to execute the tests, use [Guard-Spork](https://github.com/guard/guard-spork) will preload the rails application, and use (Guard-RSpec) will only run tests which are related to the changed file. The related rules are configurable in Guardfile.
### 7. Quiet Assets
In most of time you dont need show assets log in rails development log, just add `gem 'quiet_assets', :group => :development` in your Gemfile, and the world will be much more quieter.
### 8. Pry, Pry-Debugger And Pry-Rails
![pry](http://pryrepl.org/images/pry_logo.png)

[Pry](https://github.com/pry/pry) is great tool to inspect ruby variables, simplily add `pry-rails` to your Gemfile, when you execute `rails c` it will use `pry` as the console instead of `irb`. add `pry-debugger` to your Gemfile, you will get a debugger using pry, you can use `next`, `step` to navigate.
### 9. Vagrant
you need a production like environment to simulate something, use [Vagrant](vagrantup.com). you can write a script to setup a production rails environment in minutes, and this environment could be duplicated. You can destroy it and rebuild it very easily.