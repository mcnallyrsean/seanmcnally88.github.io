---
layout: post
title: DateTimePicker for Rails - Default Times in an Update Form
modified:
categories: 
excerpt:
tags: [Rails, DateTimePicker, gem]
image:
  feature:
date: 2016-03-07T11:15:39-06:00
---

Inevitably, you will work on a project that necessitates the use of calendars and scheduling. Dates and times in Ruby can be a difficult concept for a beginner to fully grasp. I know that it still causes me to scratch my head. Anyways, a real nice gem to use to implement this is [bootstrap-datetimepicker](https://github.com/Eonasdan/bootstrap-datetimepicker).

Bootstrap-datetimepicker creates a widget (wrapped in Bootstrap styling) that allows users to pick a calendar date and a time, which can then be posted to your application's database (granted you have them as attributes). 

An issue with this gem can arise when attempting an Update action, which will produce weirdnesses in the database. Thanks DateTime!

Anyways, the issue I ran into when implementing my update action/form was that the information would be saved inconsistently. For example, let's say I had the following appointment scheduled.

#### Appointment for John Doe
* Expected Start Time - 2016-03-06 00:09:00
* Expected End Time - 2016-03-06 00:10:00

Now, I learn that John Doe's appointment will start at 10am and end at 11am. So, I'd like to update the appointment through an update form. Here is the relevant part of the update form:

{% highlight html %}
  <h6>Expected Start Time:</h6>
  <div class='input-group date' id='datetimepicker2'>
    <input type='text' class="form-control" name="expected_start_time"/>
    <span class="input-group-addon">
      <span class="glyphicon glyphicon-calendar"></span>
    </span>
  </div>
  <script type="text/javascript">
    $(function () {
      $('#datetimepicker2').datetimepicker({
        defaultDate: "<%= @patient.appointment.expected_start_time %>",
      });
    });
  </script>
  <h6>Expected End Time:</h6>
  <div class='input-group date' id='datetimepicker3'>
    <input type='text' class="form-control" name="expected_end_time"/>
    <span class="input-group-addon">
      <span class="glyphicon glyphicon-calendar"></span>
    </span>
  </div>
  <script type="text/javascript">
    $(function () {
      $('#datetimepicker3').datetimepicker({
        defaultDate: "<%= @patient.appointment.expected_end_time %>",
      });
    });
  </script>

{% endhighlight %}

However, I notice something strange after I update the appointment. Here is the newly updated version:

#### Appointment for John Doe
* Expected Start Time - 2016-06-03 00:15:00
* Expected End Time - 2016-06-03 00:16:00

Poor John Doe has to wait till June 3rd for his appointment now. That's definitely a bug that needs to be squished. And here's how to squish it: add a default format to the datetimepicker function like so:

{% highlight html %}
  <h6>Expected Start Time:</h6>
  <div class='input-group date' id='datetimepicker2'>
    <input type='text' class="form-control" name="expected_start_time"/>
    <span class="input-group-addon">
      <span class="glyphicon glyphicon-calendar"></span>
    </span>
  </div>
  <script type="text/javascript">
    $(function () {
      $('#datetimepicker2').datetimepicker({
        defaultDate: "<%= @patient.appointment.expected_start_time %>",
        format: "YYYY-MM-DD hh:mm a"
      });
    });
  </script>
  <h6>Expected End Time:</h6>
  <div class='input-group date' id='datetimepicker3'>
    <input type='text' class="form-control" name="expected_end_time"/>
    <span class="input-group-addon">
      <span class="glyphicon glyphicon-calendar"></span>
    </span>
  </div>
  <script type="text/javascript">
    $(function () {
      $('#datetimepicker3').datetimepicker({
        defaultDate: "<%= @patient.appointment.expected_end_time %>",
        format: "YYYY-MM-DD hh:mm a"
      });
    });
  </script>
{% endhighlight %}

John Doe can now breathe easy knowing that his updated appointment will not be three months from his actual appointment.