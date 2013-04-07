---
layout: post
title: "production rails - assets precompile"
date: 2013-04-07 23:15
comments: true
categories: 
---
Precompile assets can take long time during deployment. And actually you don't need to precompile it for every deployment. Use git to check whether file has changed to avoid precompile assets everytime. 

```
namespace :deploy do
  namespace :assets do
    task :precompile, :roles => :web, :except => { :no_release => true } do
      from = source.next_revision(current_revision)
      if capture("cd #{latest_release} && #{source.local.log(from)} vendor/assets/ app/assets/ | wc -l").to_i > 0
        run %Q{cd #{latest_release} && #{rake} RAILS_ENV=#{rails_env} #{asset_env} assets:precompile}
      else
        logger.info "Skipping asset pre-compilation because there were no asset changes"
      end
    end
  end
end
```
this solution precompile assets in the server side.

```
namespace :deploy do
  namespace :assets do

    task :precompile, :roles => :web do
      from = source.next_revision(current_revision)
      if capture("cd #{latest_release} && #{source.local.log(from)} -- vendor/assets/ lib/assets/ app/assets/ | wc -l").to_i > 0
        run_locally("rake assets:clean && rake assets:precompile")
        run_locally "cd public && tar -jcf assets.tar.bz2 assets"
        top.upload "public/assets.tar.bz2", "#{shared_path}", :via => :scp
        run "cd #{shared_path} && tar -jxf assets.tar.bz2 && rm assets.tar.bz2"
        run_locally "rm public/assets.tar.bz2"
        run_locally("rake assets:clean")
      else
        logger.info "Skipping asset precompilation because there were no asset changes"
      end
    end
  end
end
```
this solution will precompile assets locally and upload the assets to the server. It's better, precompile assets can take a lot of cpu and memory, in this way, it won't affect the server while deploy code.

There is a gem called [turbo-sprockets-rails3](https://github.com/ndbroadbent/turbo-sprockets-rails3) also can solve this problem, just add this gem to your gemfile, and it works.