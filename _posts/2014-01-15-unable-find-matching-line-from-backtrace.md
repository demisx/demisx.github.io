---
layout: post
title:  "Error: Unable to Find Matching Line from Backtrace"
date:   2014-01-15 9:51:12
categories: ruby-on-rails
---

You've just upgraded to Ruby 2.1 and Ruby is hitting you back with this error?

```
Failure/Error: Unable to find matching line from backtrace
ArgumentError:
comparison of NilClass with 0 failed
```

<!--more-->

You are not alone. <i class="fa fa-smile-o fa-lg"> </i>

Looks like the [GC module](http://ruby-doc.org/core-2.1.0/GC.html) has been updated in Ruby MRI 2.1
and returning different hash keys now. In my case, this is what caused this error in RSpec
`spec/spec_helper.rb`:

{% highlight ruby linenos %}
config.after(:each) do
  slots_number = [slots_number, GC.stat[:heap_live_num]].max
end
{% endhighlight %}

Since, there is no longer `:heap_live_num` hash key returned by Ruby 2.1 `GC.stat` method, `.max`
method throws an argument error while trying to compare against `nil`.

The fix was to replace the outdated `:heap_live_num` key with the new `:heap_live_slot` one:

{% highlight ruby linenos %}
config.after(:each) do
  slots_number = [slots_number, GC.stat[:heap_live_slot]].max
end
{% endhighlight %}

And we are back in green land now. <i class="fa fa-thumbs-o-up fa-lg"> </i>

So, check to see whether you are referencing the [latest hash keys](http://ruby-doc.org/core-2.1.0/GC.html#method-c-stat)
of the `GC.stat` hash.

___
##### Today my environment was:

- Rails 4.0.2
- MRI Ruby 2.1.0
- Rspec-rails 2.14.1
