---
layout: post
title:  "AngularAtom&mdash;Component-based organization for AngularJS 1.x apps (Part&nbsp;2)"
date:   2014-12-11 15:59:00
categories: angularjs atom component-feature-based-organization
---

## Global file conventions

* an individual file should be created per each
Angular resource (e.g. controller, directive, animation, service etc.) and
the file name should incorporate the name of that particular resource,
ex. `login-directive.js`, `facebook-service.js`, `phone-animation.js`.
Do not combine multiple resources in one file.
* the file name should be suffixed with the type of resource it contains, e.g. `...-model.js`,
`...-controller.js`, `...-service.js` and so on
* unit tests should append `_spec` to the file name and be placed alongside
each file the logic of which they are testing. There is no dedicated folder
for the unit tests. This is done intentionally, so it is easier to locate
the existing and the missing unit tests.


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

* `.build/`&mdash;destination directory for your automatic build program
(e.g. gulp, grunt etc.) to write to. This is where you would usually point your
development web server to and output:
  1. _Development build_, i.e. development specific project files.
  Make sure your build tool writes here the full version of
  JS libraries (not minified ones) and the source maps for all preprocessed
  files (e.g. CoffeeScript, SASS, LESS etc.) in order to aid your debugging
  experience. We prefix this directory with `.` intentionally, so it shows
  up at the top of directory listing.
  1. _Production build_, i.e. production ready release files. Therefore, the library files in this
  directory are concatenated and minified. It contains no source maps or
  anything that is not needed for your release to function in production mode.

* `app/`&mdash;this is  where all **user-generated** application source code
goes to. A rule of thumb: if you didn't write it, don't place it here. Exceptions
are the 3rd party libraries necessary to support a given component. Though, you
didn't write them, they are still placed inside `app/components/[componentName]/vendor/`
directory in order to maintain the integrity of a component.

* `bower_components/`&mdash;container for the global 3rd party client libraries managed by the [bower&nbsp;package&nbsp;manager](http://bower.io/)

* `config/`&mdash;contains all application configuration files required by your project. These include, but not limited to, config files for unit and e2e testing, web server
setup, CI, deployments etc.

* `node_modules/`&mdash;container for the global 3rd party Node.js libraries managed by the [npm&nbsp;package&nbsp;manager](https://www.npmjs.com/)

* `scripts/`&mdash;container for all shell scripts used in the project, if any

* `specs/`&mdash;container for various helpers, request specs, end-to-end specs (lifecycle tests
using 3rd party APIs, no mocks) etc. Do not place unit tests here. They will be
placed inside `app/` directory alongside the logic they are testing
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
