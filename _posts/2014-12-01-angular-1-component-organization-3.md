---
layout: post
title:  "AngularAtom&mdash;Component-based organization for AngularJS 1.x apps (Part&nbsp;3)"
date:   2014-12-12 10:39:00
categories: angularjs atom component-feature-based-organization
---

## Level 2: `app/` folder organization

No, we are ready to start reviewing organization structure at the second level.
If you've missed my
[previous posts](/angularjs/atom/component-feature-based-organization/2014/12/02/angular-1-component-organization-1.html), recommend reading them first before continueing
here.

In this post we'll examine organization of the `app/` folder.
Since the `app/` folder contains user generated application code, we divide it
like this:

<!--more-->

{% gist cbbf605db31e7c9f5cf6 angularjs-1-component-organization-l2-app.sh %}

* `components/`&mdash;container for all user written application components.
A `Component` defined as a single isolated feature or a piece of functionality
that application is broken into. Components are stateless and can be shared
between other components. This is somewhat similar to the concept of features
in Agile/XP methodology. Examples of such components are:
  * `auth`&mdash;enables application to authenticate users (authn) and control their access (authz)
  * `profile`&mdash;enables profile management in an application
  * `site-search`&mdash;enables search within an application
  * `dashboard`&mdash;gives ability to users to manage application dashboard(s)
  * ...

* `layouts/`&mdash;container for the layout definition partial files (one file per
each supported layout).
* `states/`&mdash;container for all application UI states.

We'll look into organization of each of these folders in more detail when we start
examining organization of the third level in the upcoming posts.

The rest of the folders located at the second level of the application root
have either a standard package-managed (e.g. `bower_components`, `node_modules`)
or flexible organization, so we won't be covering them here in detail.

As always, please feel free to comment on this post below.

<div id="post-navigation" >
  <div class="previous">
    {% if page.previous.url %}
    <a href="{{page.previous.url}}" title="Previous post: {{page.next.title}}">
    <i class="fa fa-lg fa-arrow-circle-left"></i>
    {{page.previous.title}}
    </a>
    {% endif %}
  </div>
  <div class="next">
    {% if page.next.url %}
    <a href="{{page.next.url}}" title="Next post:
    {{page.next.title}}">NEXT: {{page.next.title}} <i class="fa fa-2x fa-arrow-circle-right"></i></a>
    {% endif %}
  </div>

  <div class="next">
  <a href="#">
  <i class="fa fa-2x fa-arrow-circle-right"></i>
  </a>Continue to Part 4... (Coming soon)
  </div>
</div>

___

Today my environment was:

- Angular 1.3.5
- AngularUI Router 0.2.12
- Mac OS X 10.10.1
