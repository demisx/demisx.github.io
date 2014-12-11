---
layout: post
title:  "How to Configure CORS Accept Headers in Rails API Application"
date:   2014-02-18 10:41:12
categories: rails-api
---

You've finally realized the value of SOA
([Service Oriented Architecturee](http://en.wikipedia.org/wiki/Service-oriented_architecture))
and decided to move all of your dynamic functionality into an independent service layer. Smart!

<!--more-->

You start turning your cool dynamic features into REST JSON web services and then have your HTML
sites (a.k.a consumer) query these services for JSON data (a.k.a. provider). However, the response from
your web service is coming back blank, though the HTTP response status says 200 OK and
you can see JSON data when accessing the web service endpoint directly. Sounds familiar?

What you may be dealing with here is known as a
[Cross-origin Resource Sharing (CORS)](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing)
and covers situations where a web service you are querying is on a different site compared to the one
initiating the request. Browsers ban such requests trying to comply with so-called
"[Same Origin Security Policy](http://en.wikipedia.org/wiki/Same_origin_policy)".

Here, it is important to understand what a site is in this particular context:

<div class="alert alert-warning">
<i class="fa fa-lightbulb-o fa-2x"></i> "Site" is a combination of scheme, hostname, and port number.
</div>

So, even if your consumer is on "localhost:3000" and your provider is on "localhost:3001", the response
will be neglected unless your web service sets a permissive CORS policy. This policy is
defined by the provider setting specific CORS headers in HTTP response that a consumer app
(e.g. a browser) destined to enforce. For example, configuring these headers:

{% highlight ruby %}
# in config/application.rb
config.action_dispatch.default_headers = {
    'Access-Control-Allow-Origin' => 'http://my-web-service-consumer-site.com',
    'Access-Control-Request-Method' => %w{GET POST OPTIONS}.join(",")
  }
{% endhighlight %}

will allow a web service to service `GET | POST | OPTIONS` requests from your
`http://my-web-service-consumer-site.com` site.

<div class="alert alert-danger">
  <i class="fa fa-exclamation-circle fa-2x"></i>   Please note, that setting
  'Access-Control-Allow-Origin' => '*' is highly discouraged, unless you
  are providing a public API that is intended to be accessed by any consumer out there.
</div>

If you want to allow multiple sites to access your web service, the recommended way is to have your
server read the Origin header from the consumer, compare that to the list of sites you'd like to allow,
and, if it matches, send the value of the Origin header back to the consumer as the
`Access-Control-Allow-Origin` header in the response. Simply listing multiple sites or adding multiple
`Access-Control-Allow-Origin` headers in the response doesn't appear to produce consistent results
accross different browsers.

For those, who prefer to define CORS policy via Rack middleware, take a look at this
rails-cors (https://github.com/cyu/rack-cors) gem.

___
##### Today my environment was:

- Rails 4.0.2
- rails-api 0.2.0
