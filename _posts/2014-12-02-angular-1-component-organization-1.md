---
layout: post
title:  "AngularAtom&mdash;Component-based organization for AngularJS 1.x apps (Part&nbsp;1)"
date:   2014-12-02 13:59:00
categories: angularjs atom component-feature-based-organization
---

## Introduction

Many of us started the Angular journey by reading the official docs at www.angularjs.org
and following their [online PhoneCat app tutorial](https://docs.angularjs.org/tutorial).
I personally think this is a very good tutorial giving an immediate exposure to
the various important concepts of Angular 1. The code is
[organized by type](https://github.com/angular/angular-phonecat)
where all controllers are placed into one `controllers.js` file, all directives
are placed in `directives.js`, services in `services.js` and so forth.

<!--more-->

Though, this type-based organization works just fine for the small short-lived apps,
IMHO, it has some serious flaws for any medium or large application that needs
to be maintained over time (a.k.a any production app):

* Coupling of various components that belong to different app features
* Difficult to locate a particular piece of code related to a given type of functionality
* Hard to identify all feature related code and reuse it with other apps
* The files grow longer and longer as more controllers, services etc. being added
* No clear view into what the application is and what it really does
* Not web-component friendly (think of Angular 2.0)

In the next few posts, we'll try to overcome these shortcomings by learning
about component-based organization named 'AngularAtom' that groups various web
resources around a particular component instead of grouping them
by type. We'll take the original [type-based tutorial](https://docs.angularjs.org/tutorial)
and reorganize it to be a
[component-based Angular PhoneCat app tutorial](https://github.com/demisx/angular-phonecat-components).
This will also help us to get our mind set on the component oriented development
today and be better prepared when Angular 2.0 hits the prime time.

The future posts on this topic will be announced via Twitter, so make sure to
<span style="line-height:20px;">
  <a class="twitter-follow-button"
    href="https://twitter.com/demisx1"
    data-show-count="false"
    data-lang="en"
    data-size="large">Follow @demisx1
  </a> to be notified when that happens.
</span>

# tl;dr
For those, who want to see the end result now. Each section of the suggested
structure below will be discussed in the future posts in detail.

AngularAtom organization follows the **LIFT** principle coined by John Papa:

* **L** - Locate code easy
* **I** - Identify code at a glance
* **F** - Flat structure as long as we can
* **T** - Try to stay DRY

As always, please feel free to comment on this post below.

{% gist cbbf605db31e7c9f5cf6 angularjs-1-component-organization-all.sh %}

<div id="post-navigation" >
  <div class="previous">
    {% if page.previous.url %}
    <a href="{{page.previous.url}}" title="Previous post: {{page.next.title}}">
      <i class="fa fa-lg fa-arrow-circle-left"></i>
      {{page.previous.title}}
    </a>
    {% endif %}
  </div>
  <div class="next text-right">
    {% if page.next.url %}
    <a href="{{page.next.url}}" title="Next post:
    {{page.next.title}}">NEXT: {{page.next.title}} <i class="fa fa-2x fa-arrow-circle-right"></i></a>
    {% endif %}
  </div>
</div>


___

Today my environment was:

- Angular 1.3.2
- AngularUI Router 0.2.12
- Mac OS X 10.10.1
