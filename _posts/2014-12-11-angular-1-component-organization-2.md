---
layout: post
title:  "AngularAtom&mdash;Component-based organization for AngularJS 1.x apps (Part&nbsp;2)"
date:   2014-12-11 15:59:00
categories: angularjs atom component-feature-based-organization
---

## Level 1 organization

Now we are ready to start reviewing organization structure at level one.
If you've missed my introduction post into [component-based organization called
AngularAtom]({% post_url 2014-12-02-angular-1-component-organization-1 %}), recommend reading it first before continuing further:

### Application root

At first level, also known as application root, we break down our application into two main chunks:

1. User generated application code
1. Plus, everything else required for this application code to run.

<!--more-->

{% gist cbbf605db31e7c9f5cf6 angularjs-1-component-organization-l1.sh %}

* `.build/` - this is the destination directory that contains necessary files for
your app to run in development or debug mode. This is where you would usually output development specific project files and point your development web server instance to. Make sure your build tool writes here the full version of JS libraries (not minified ones) and the source maps for all preprocessed files (e.g. CoffeeScript, SASS, LESS etc.) in order to aid your debugging experience. We prefix this directory with `.` intentionally, so it shows up at the top of directory listing.

* `.dist/` - this is the destination directory for your production ready release files. Therefore, the library files in this directory are concatenated and minified.
It contains no source maps or anything that is not needed for your release to function in production mode.

* `app/` - this is  where all **user-generated** application code goes to. If you
didn't write it, don't place it here.

* `bower_components/` - container for the global 3rd party client libraries managed by the [bower&nbsp;package&nbsp;manager](http://bower.io/)

* `config/` - contains all application configuration files required by your project. These include, but not limited to, config files for unit and e2e testing, web server
setup, CI, deployments etc.

* `node_modules/` - container for the global 3rd party Node.js libraries managed by the [npm&nbsp;package&nbsp;manager](https://www.npmjs.com/)

* `scripts/` - container for all shell scripts used in the project, if any

* `tests-e2e/` - container for end-to-end tests. These are the full lifecycle tests
using 3rd party APIs. There are no mocks in e2e tests. Do
not place unit tests here. They will be placed inside `app/` directory alongside
the logic they are testing (we'll talk about this more in the future posts).

* `tests-request/` - container for request tests. Request tests differ from e2e tests that they mock out 3rd party APIs (e.g. Facebook, Google etc.). Each request test lifecycle starts and ends within the application itself and never reaches outside of its boundaries. Do not place unit tests here. They will be placed inside `app/` directory alongside the logic they are testing
(we'll talk about this more in the future posts).

All other files placed in the root of an application (e.g. `bower.json`, `pakages.json`, `README.md` etc.) are very common to any JS project and not really
specific to the component-based organization of a project. Thus, we'll not go into details about them.

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
</div>

___

Today my environment was:

- Angular 1.3.5
- AngularUI Router 0.2.12
- Mac OS X 10.10.1
