---
layout: post
title: Rails Forms - Using a Helper
modified:
categories: 
excerpt: A nice little post for beginners (hopefully).
tags: [Rails, forms, helpers]
image:
  feature:
date: 2016-03-04T12:36:28-06:00
---

So, you're just starting in Rails and you've created a form. Awesome! You're CRUDing everything up and creating instances of models left and right. However, you'll soon get tired of typing in certain things just right and will soon be looking for away to make the form a little more user friendly. What do I mean by this? Wouldn't it be nice to be able to have a dropdown of say...all US states or something similar? Thankfully, doing this is as easy as utilizing a helper.

So let's work off the US states example. Say you want a user to be able select the state of their address in a dropdown. The first step will be to create a file in the helpers folder of your rails project. 

##### Step 1
Navigate to *yourapp*/app/helpers and create a file called states_helper.rb which will create a file that looks like this:

{% highlight ruby %}
module StatesHelper
end
{% endhighlight %}

##### Step 2
Now that we've created the States Helper, we can actually add some code in between the module.

Let's create a method called us_states like so.

{% highlight ruby %}
module StatesHelper
  def us_states
  end
end
{% endhighlight %}

Now its time to add the states to the code. Now instead of handwriting each state into the helper, I'll advocate copy and pasting (cue ominous music), but in this instance I think its a-ok to do so. Basically, you'll want the states helper file to end up looking like this:

{% highlight ruby %}
module StatesHelper
  def us_states
  [
    ['Alabama', 'AL'],
    ['Alaska', 'AK'],
    ['Arizona', 'AZ'],
    ['Arkansas', 'AR'],
    ['California', 'CA'],
    ['Colorado', 'CO'],
    ['Connecticut', 'CT'],
    ['Delaware', 'DE'],
    ['District of Columbia', 'DC'],
    ['Florida', 'FL'],
    ['Georgia', 'GA'],
    ['Hawaii', 'HI'],
    ['Idaho', 'ID'],
    ['Illinois', 'IL'],
    ['Indiana', 'IN'],
    ['Iowa', 'IA'],
    ['Kansas', 'KS'],
    ['Kentucky', 'KY'],
    ['Louisiana', 'LA'],
    ['Maine', 'ME'],
    ['Maryland', 'MD'],
    ['Massachusetts', 'MA'],
    ['Michigan', 'MI'],
    ['Minnesota', 'MN'],
    ['Mississippi', 'MS'],
    ['Missouri', 'MO'],
    ['Montana', 'MT'],
    ['Nebraska', 'NE'],
    ['Nevada', 'NV'],
    ['New Hampshire', 'NH'],
    ['New Jersey', 'NJ'],
    ['New Mexico', 'NM'],
    ['New York', 'NY'],
    ['North Carolina', 'NC'],
    ['North Dakota', 'ND'],
    ['Ohio', 'OH'],
    ['Oklahoma', 'OK'],
    ['Oregon', 'OR'],
    ['Pennsylvania', 'PA'],
    ['Puerto Rico', 'PR'],
    ['Rhode Island', 'RI'],
    ['South Carolina', 'SC'],
    ['South Dakota', 'SD'],
    ['Tennessee', 'TN'],
    ['Texas', 'TX'],
    ['Utah', 'UT'],
    ['Vermont', 'VT'],
    ['Virginia', 'VA'],
    ['Washington', 'WA'],
    ['West Virginia', 'WV'],
    ['Wisconsin', 'WI'],
    ['Wyoming', 'WY']
  ]
  end
end
{% endhighlight %}

So now we have an array of arrays that contain the full state name, and the postal abbreviation for each state. The full state name (or whatever you put in array[0]) will be what appears in the form dropdown, and the postal abbreviation (or whatever you put in array[1]) will be what posts to the database.

##### Step 3
We're done with the helper! Now we can add it to your Rails form.

Here is how to implement the helper (with "IL" being the default choice) in a form_for:

{% highlight ruby %}
#Create Action
<%= f.select :state, options_for_select(us_states, "IL") %>
{% endhighlight %}

{% highlight ruby %}
#Update Action
<%= f.select :state, options_for_select(us_states, @your_user.state ) %>
{% endhighlight %}

##### Step 4
Celebrate a job well done.



