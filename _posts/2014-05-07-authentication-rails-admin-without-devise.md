---
layout: post
title:  "Basic authentication with Rails Admin without Devise"
date:   2014-05-07 12:19:12
categories: rails-admin
---

[RailsAdmin](https://github.com/sferik/rails_admin) is a very nice simple gem
that allows you to manage data in models via a simple to use GUI. By default, it
comes without any authentication enabled, so anyone can access the `/admin` URL.
It does allow you to integrate with popular authentication frameworks, though,
like devise and CanCan, however, I find it to be an overkill in small simple
projects. In case of the latter, I simply opt in for basic authentication over
HTTPS.

<!--more-->

Here is what needs to be done to enable basic authentication for RailsAdmin:

**Step 1.** Add user and password you wish to protect access with:

```ruby
# in config/secrets.yml
development:
  ... ... ...
  user: radmin
  password: [my_dev_password]
production:
  ... ... ...
  user: radmin
  password: [myProdPassword]
```

**Step 2.** Add these lines to RA initializer:

```ruby
# in config/initializers/reails_admin.rb
  config.authorize_with do
    authenticate_or_request_with_http_basic('Login required') do |username, password|
      username == Rails.application.secrets.user &&
      password == Rails.application.secrets.password
    end
  end
```

**Step 3.** Restart

Now, restart your rails app server and try to access the RailsAdmin URL (e.g. `/admin`).
It should prompt for a user name and password.

___
##### Today my environment was:

- Rails 4.1.0.rc2
- Rails Admin 0.6.1
