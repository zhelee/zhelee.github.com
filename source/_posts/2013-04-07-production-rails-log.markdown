---
layout: post
title: "production rails - log"
date: 2013-04-07 23:28
comments: true
categories: 
---
I use heroku when I started learning rails. Heroku provide very good log tool to let you check what's happeing when the app is running. There is no capistrano recipes by default, I created the following log recipes, hope it will help someone.

```
desc "tail log files"
namespace :log do
  task :rails, :roles => :app do
    run "tail -f #{shared_path}/log/production.log" do |channel, stream, data|
      puts "#{channel[:host]}: #{data}"
      break if stream == :err
    end
  end

  task :unicorn_error, :roles => :app do
    run "tail -f #{shared_path}/log/unicorn_stderr.log" do |channel, stream, data|
      puts "#{channel[:host]}: #{data}"
      break if stream == :err
    end
  end

  task :unicorn_out, :roles => :app do
    run "tail -f #{shared_path}/log/unicorn_stdout.log" do |channel, stream, data|
      puts "#{channel[:host]}: #{data}"
      break if stream == :err
    end
  end

  task :nginx_error, :roles => :web do
    run "tail -f /var/log/nginx/error.log" do |channel, stream, data|
      puts "#{channel[:host]}: #{data}"
      break if stream == :err
    end
  end

  task :nginx_access, :roles => :web do
    run "tail -f /var/log/nginx/access.log" do |channel, stream, data|
      puts "#{channel[:host]}: #{data}"
      break if stream == :err
    end
  end
end
```