---
layout: post
title: Creating Rake Tasks and Using Heroku Scheduler
modified:
categories: 
excerpt:
tags: [rails, rake, heroku, wicked]
image:
  feature:
date: 2016-03-08T22:42:04-06:00
---

In my last project, I used the [Wicked](https://github.com/schneems/wicked) gem to create multi-step form wizards. More specifically, I closely followed Josh McArthur's [implementation](http://brains.rabid.co.nz/2014/09/29/rails-multistep-forms.html) of Wicked when building my model objects step-by-step.

One of the inconveniences of building out an object in this manner is that there will inevitably be blank or incomplete model instances in your application's database. Schneems, the author of the Wicked gem, recognizes this and recommends certain actions be taken. To quote the Wicked documentation:

> If you have conditional validation it can be easy to have incomplete Products laying around in your database, you should set up a sweeper task using something like Cron, or Heroku's scheduler to clean up Products that are not complete.

So, let's build out that sweeper.

##### Sweeper Set Up

In your Rails application, navigate to 

```
lib/tasks
```

and create a file called cleanup.rb

```
lib/tasks/cleanup.rb
```

Once this file is created, we can build out the database sweeper. Here is the final version from my last project

{% highlight ruby%}
namespace :cleanup do
  #run rake cleanup:my_application to run sweeper
  desc "removes stale and inactive model instances from the database"

  task :my_application => :environment do
    stale_patients = Patient.where(username: nil)
    stale_patients.map(&:destroy)

    stale_providers = Provider.where(username: nil)
    stale_providers.map(&:destroy)
 
    stale_drivers = Driver.where(username: nil)
    stale_drivers.map(&:destroy)
  
    stale_transportations = Transportation.where(username: nil)
    stale_transportations.map(&:destroy) 
  end
end
{% endhighlight %}

Obviously, you'll want to take care in determining the conditions that warrant deletion. 

Once we had this setup, we ran the following command in the command line

```
$ rake cleanup:my_application
```

and all model instances that fit the deletion criteria were destroyed. Pretty neat huh? So this works great in development environment, but obviously you'd like a way to do this in production.

##### Heroku Scheduler

[Heroku Scheduler](https://elements.heroku.com/addons/scheduler) is a free (though you have to provide a credit card number) add-on to Heroku applications that can run tasks at a scheduled interval. To install the Scheduler, run the following code in the command line.

```
$ heroku addons:create scheduler:standard
```

It should now be showing in your Heroku application's portal under "Add-Ons". To test whether the task works in the production/Heroku environment run

```
$ heroku run rake cleanup:my_application
```
and then check Postico to see if the blank/incomplete model instances are gone (they should be).

Once we have verified that the task works, the final step is to set up the Scheduler add-on itself. Navigate to the Add-On section of the application portal and click on Heroku Scheduler. This will direct you to the task set up screen. Here, you specify the task to be run (in our case - rake cleanup:my_application) and the frequency with which the task will be run (every 10 minutes, every hour, every 24 hours). We ended up choosing every 24 hours - more specifically - everyday at 3 in the morning.

Happy coding folks. 



