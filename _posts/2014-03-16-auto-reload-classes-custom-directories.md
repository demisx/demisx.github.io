---
layout: post
title:  "How to have Rails auto reload classes in custom directories under app/"
date:   2014-03-16 17:13:12
categories: rails
---

Over time, your rails models go from being a super model to being fat and eventually end up
obese waiting any time for a heart attack to choke your app. When refactoring such models, I really
like and follow the approach described by Code Climate in [7 Patterns to Refactor Fat ActiveRecord Models]
(http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/).

<!--more-->

Leveraging their technique, I create this directory structure for the new objects:

 ```
app/
----------
  |___controllers/ -> rails default dir to store controller classes
  |___models/      -> rails default dir to store models classes
  |___views/       -> rails default dir to store views classes
  |___... ... ...
  |___decorators/  -> custom dir to store decorator objects
  |___policies/    -> custom dir to store policy objects
  |___presenters/  -> custom dir to store presenter objects
  |___services/    -> custom dir to store service objects
  |___validators/  -> custom dir to store validator objects
  |___... ... ...
```

and then name space my new objects as I believe it leads to better code organization and
avoids potential name collisions in the future:


{% highlight ruby linenos %}
# Example of a service object in "app/services/doctor_finder.rb" directory
module Services
  class DoctorFinder
    def self.find_doctors(location, distance=Settings.finder.default_distance)
      Doctor.near(location, distance).first(Settings.finder.max_results_number)
    end
  end
end
{% endhighlight %}

The issue with this approach (Rails 4 at the time of writing) that these classes/objects in custom
diretories will not be auto reloaded in development environment with default rails config. In fact,
if name spaced, these classes will throw `Uninitialized constant [your_calling_class]::Services`
type of exception, because Rails will not know how to resolve your new name spaced constants.
To resolve this issue and make sure that your classes auto reload, you need to do two things:

1. Update _config/application.rb_ file to add "app/" directory to the auto load paths:
`config.autoload_paths += %W(#{config.root}/app)`
1. Make sure you don't manually require your new classes anywhere in the code. Let Rails magic
do that for you.

That's all. Your new classes should auto reload now similar to rails default controllers, models,
views etc.

Special 'thank you' to the awesome rails team @fxn, @jeremy, @thedarkone and all others who helped me with
resolving this issue [Classes not reloaded...](https://github.com/rails/rails/issues/14382)
___
##### Today my environment was:

- Rails 4.1.0.rc1
