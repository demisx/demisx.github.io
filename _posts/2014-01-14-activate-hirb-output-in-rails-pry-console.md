---
layout: post
title:  "Activate Hirb Formatted Output in Pry-enabled Rails Console"
date:   2014-01-14 12:46:12
categories: ruby-on-rails
---

So, you've finally found some free time to replace your default Rails console with [Pry](https://github.com/pry/pry)
and now you want to see nice [Hirb](https://github.com/cldwalker/hirb) formatted output
instead of the default IRB one? I hear you. To achieve that just add the following lines to your local `~/.pryrc` file
(e.g. /home/dmoore/.pryrc)

<!--more-->

{% gist 8320981 %}

and start (or restart if you already have it running) the Rails console. You should be all set now.

For exmaple, Here is how Hirb formats the output of your Rails model rows:

{% highlight console %}
[dmoore]$ cd <path_to_my_rails_app>
[dmoore]$ rails c
Loading development environment (Rails 4.0.2)
# Graduate is a model class and descendant of ActiveRecord::Base
[1] pry(main)> Graduate.all
  Graduate Load (28.2ms)  SELECT `graduates`.* FROM `graduates`
+----+----------------+-------------------------+-------------------------+
| id | workflow_state | created_at              | updated_at              |
+----+----------------+-------------------------+-------------------------+
| 1  | initiated      | 2014-01-06 18:13:02 UTC | 2014-01-06 18:13:02 UTC |
| 2  | initiated      | 2014-01-06 23:31:02 UTC | 2014-01-06 23:31:02 UTC |
| 3  | initiated      | 2014-01-06 23:36:51 UTC | 2014-01-06 23:36:51 UTC |
+----+----------------+-------------------------+-------------------------+
3 rows in set
{% endhighlight %}

To switch back and forth between the default IRB-foratted output and Hirb-formatted output in Rails
console use `Hirb.disable` | `Hirb.enable` commands respectively:

{% highlight console %}
[2] pry(main)> Hirb.disable
=> false
[3] pry(main)> Graduate.all
  Graduate Load (0.5ms)  SELECT `graduates`.* FROM `graduates`
=> [#<Graduate id: 1, workflow_state: "initiated", created_at: "2014-01-06 18:13:02", updated_at: "2014-01-06 18:13:02">,
 #<Graduate id: 2, workflow_state: "initiated", created_at: "2014-01-06 23:31:02", updated_at: "2014-01-06 23:31:02">,
 #<Graduate id: 3, workflow_state: "initiated", created_at: "2014-01-06 23:36:51", updated_at: "2014-01-06 23:36:51">]
[4] pry(main)> Hirb.enable
=> true
[5] pry(main)> Graduate.all
  Graduate Load (0.5ms)  SELECT `graduates`.* FROM `graduates`
+----+----------------+-------------------------+-------------------------+
| id | workflow_state | created_at              | updated_at              |
+----+----------------+-------------------------+-------------------------+
| 1  | initiated      | 2014-01-06 18:13:02 UTC | 2014-01-06 18:13:02 UTC |
| 2  | initiated      | 2014-01-06 23:31:02 UTC | 2014-01-06 23:31:02 UTC |
| 3  | initiated      | 2014-01-06 23:36:51 UTC | 2014-01-06 23:36:51 UTC |
+----+----------------+-------------------------+-------------------------+
3 rows in set
[6] pry(main)>
{% endhighlight %}

___
##### Today my environment was:

- Rails 4.0.2 pry-enabled console
- Pry version 0.9.12.4
- Ruby MRI 2.1.0.
