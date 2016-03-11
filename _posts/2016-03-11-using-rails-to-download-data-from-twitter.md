---
layout: post
title: Using Rails to Download Data From Twitter
modified:
categories: 
excerpt:
tags: [rails, twitter, postgresql, api]
image:
  feature:
date: 2016-03-11T09:46:59-06:00
---

#### Objective
1. Utilize the Twitter API to download data to the application database

#### Tools
1. [Twitter](https://twitter.com/)
2. [Twitter gem](https://github.com/sferik/twitter)
3. seeds.rb 

#### Let's get to it!

So you want to download data from Twitter to use in your application? Very cool and very easy to do (once you have the tools and know-how).

The first step is to add the 'twitter' gem to your gemfile like such - 

{%highlight ruby%}
gem 'twitter'
{%endhighlight%}

and then run ```bundle install```. Once this is in your gemfile, you're well on your way to integrating Twitter (and its data) into your application, but we're not quite there yet. The 'twitter' gem specifies that it needs 4 configuration values to work

{%highlight ruby%}
 client = Twitter::REST::Client.new do |config|
  config.consumer_key        = "YOUR_CONSUMER_KEY"
  config.consumer_secret     = "YOUR_CONSUMER_SECRET"
  config.access_token        = "YOUR_ACCESS_TOKEN"
  config.access_token_secret = "YOUR_ACCESS_SECRET"
end
{%endhighlight%}

so it looks like we need to head over to Twitter to request a developer key, token, and secrets. Assuming you have an account with Twitter already, this is as simple as [registering your app](https://apps.twitter.com/) and generating they required configuration values. 

Once you have these configuration values, you're pretty much good to go. I would recommend utilizing a gem like [figaro](https://github.com/laserlemon/figaro) and wrapping these sensitive values in ENV variables. Here is how my configuration ended up looking

{%highlight ruby%}
client = Twitter::REST::Client.new do |config|
  config.consumer_key = ENV["TWITTER_CONSUMER_KEY"]
  config.consumer_secret = ENV["TWITTER_CONSUMER_SECRET"]
  config.access_token = ENV["TWITTER_ACCESS_TOKEN"]
  config.access_token_secret = ENV["TWITTER_ACCESS_SECRET"]
end
{%endhighlight%}

Now, you can utilize all the methods offered by the 'twitter' gem to access Twitter data. Browse through the gem's [docs](http://www.rubydoc.info/gems/twitter) to see all the methods afforded to you.

#### Seeding your database!

So now that we have access to Twitter's data thanks to the 'twitter' gem, we can work on adding data to our database.

Let's say we want to add the user data from each member of a public list. Let's use [this list of NBA team accounts](https://twitter.com/nba/lists/nbateams/members) compiled by the NBA's official account. Let's also grab the id of this particular list while we're at it. I use Chrome's inspector tools to locate this (you'll be searching for the data-list-id in this instance).

<figure>
  <img src="/images/twitter_id.jpg">
</figure>

Once we have the list id, we have what we need to seed our database. Exciting right?

In my application, I created a TeamAccount model with the following attributes 

* name
* description
* handle
* twitter_id
* profile_image_url

and luckily for me, thanks to the 'twitter' gem, I can get all the information for every member of the list simply by looping through the list using the ```each``` method and correspondingly add all that data to my TeamAccount model.

So, head over to db/seeds.rb and write your code. Here is my seeds file

{%highlight ruby%}
client = Twitter::REST::Client.new do |config|
  config.consumer_key = ENV["TWITTER_CONSUMER_KEY"]
  config.consumer_secret = ENV["TWITTER_CONSUMER_SECRET"]
  config.access_token = ENV["TWITTER_ACCESS_TOKEN"]
  config.access_token_secret = ENV["TWITTER_ACCESS_SECRET"]
end

teams = client.list_members(3738526)

teams.each do |team|
  TeamAccount.create(
    name: team.name,
    description: team.description,
    handle: team.screen_name,
    twitter_id: team.id,
    url: team.url,
    profile_image_url: team.profile_image_url
    )
end
{%endhighlight%}

Note that the keys in my ```TeamAccount.create``` action are my model attributes and the values are the 'twitter' gem methods.

All that's left to do is run ```rake db:seed``` in your Terminal.

The result...

<figure>
  <img src="/images/nba_seed.jpg">
</figure> 

sweet, sweet data.