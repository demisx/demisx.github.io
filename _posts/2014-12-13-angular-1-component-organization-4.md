---
layout: post
title:  "AngularAtom&mdash;Component-based organization for AngularJS 1.x apps (Part&nbsp;4)"
date:   2014-12-13 10:09:00
categories: angularjs atom component-feature-based-organization
---

## Level 3 organization

Now we are ready to start reviewing organization structure at the third level.
If you've missed my previous posts, recommend reading them first before
continuing further:

* [AngularAtom Component-based organization - Introduction]({% post_url 2014-12-02-angular-1-component-organization-1 %}) (Part 1)
* [AngularAtom Component-based organization - Level 1 Organization]({% post_url 2014-12-11-angular-1-component-organization-2 %}) (Part 2)
* [AngularAtom Component-based organization - Level 2 Organization]({% post_url 2014-12-12-angular-1-component-organization-3 %}) (Part 3)

### `components/` folder

A `component` is defined as a single isolated feature or a piece of functionality
that application is broken into. Components are stateless and can be shared
between other components. This is somewhat similar to the concept of features
in Agile/XP methodology. Examples of such components are:
* `auth`&mdash;enables application to authenticate users (authn) and control their access (authz)
* `profile`&mdash;enables profile management in an application
* `site-search`&mdash;enables search within an application
* `dashboard`&mdash;gives ability to users to manage application dashboard(s)
* ...

<!--more-->

{% gist cbbf605db31e7c9f5cf6 angularjs-1-component-organization-l3-components.sh %}

More detail coming soon...

### `layouts/` folder

{% gist cbbf605db31e7c9f5cf6 angularjs-1-component-organization-l3-layouts.sh %}

Coming soon...

### `states/` folder

{% gist cbbf605db31e7c9f5cf6 angularjs-1-component-organization-l3-states.sh %}

Coming soon...

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
