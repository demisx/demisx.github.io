---
layout: post
title:  "AngularJS 1.x App Component-based Organization <br/>(Part 1)"
date:   2014-12-02 13:59:00
categories: angularjs component-feature-based-organization
---

Many of us started the Angular journey by reading the official docs at www.angularjs.org
and following their [online PhoneCat app tutorial](https://docs.angularjs.org/tutorial). 
I personally think this is a very good tutorial giving an immediate exposure to 
the various important concepts of Angular. The code itself 
is [organized by type](https://github.com/angular/angular-phonecat) 
where all controllers are placed into one `controllers.js` file, all directives
are placed in `directives.js`, services in `services.js` and so forth.

Though, this type-based organization works just fine for the small short-lived apps, 
it has some serious flaws, in my opinion, for any medium or large application that needs
to be maintained over time (a.k.a any production app):

* Coupling of various components that belong to different features
* Difficult to locate a particular piece of code related to a given type of functionality
* Hard to identify all feature related code and reuse it with other apps
* The files grow longer and longer as more controllers, services etc. being added
* No clear view into what the application is and what it really does
* Not web-component friendly (think of Angular 2.0)

In the next few posts, we'll try to overcome these shortcomings by learning 
about component-based organization that groups various web resources around a 
particular component (or feature) instead of grouping them by type. We'll take the 
original [type-based Angular PhoneCat app tutorial](https://docs.angularjs.org/tutorial) 
and reorganize it to be [component-based](https://github.com/demisx/angular-phonecat-components).
This will also help us to get our mind set on the component oriented development 
today and be better prepared when Angular 2.0 hits the prime time.

The future posts on this topic will be anounced via Twitter, so make sure to
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

**LIFT** organization principle:

* **L** - Locate code easy
* **I** - Identify code at a glance
* **F** - Flat structure as long as we can
* **T** - Try to stay DRY

{% gist cbbf605db31e7c9f5cf6 angularjs-1-component-organization-sample.sh %} 

<div id='footer-spacing'></div>
<p class="text-right"><a href="#"><i class="fa fa-arrow-circle-right"></i></a> Continue to Part 2... (Coming soon)</p>
___

Today my environment was:

- Angular 1.3.2
- Mac OS X 10.10.1
