---
layout: post
title:  "Livereload Doesn't Reload BetterErrors Page"
date:   2014-01-02 12:17:04
categories: rails
---

My livereload was working fine until I've added the better_errors gem. Then, each time when
an error page was shown by better_errors, I had to manually hit Command+R to reload that page in
order to see the changes I've made. Needless to say, this was quite inconvenient in the development.

<!--more-->

This issue was solved by placing the `Rack::LiveReload` above the `BetterErrors::Middleware` one
in the middleware stack by changing this line in the `/config/environments/development.rb` from
the suggested:

```
config.middleware.use Rack::LiveReload
```

to:

```
config.middleware.insert_before Rack::Lock, Rack::LiveReload  
```

and then restarting Rails server.

Make sure to run `rake middleware` in the terminal and confirm that `Rack::LiveReload` entry now
precedes the `BetterErrors::Middleware` entry in the middleware list.

___
##### Today my environment was:

- better_errors 1.0.1
- rack-livereload 0.3.15
- Rails 4.0.0
