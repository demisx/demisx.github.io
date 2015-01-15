---
layout: post
title:  "AngularAtom&mdash;Component-based organization for AngularJS 1.x apps (Part&nbsp;4)"
date:   2014-12-13 10:09:00
categories: angularjs atom component-feature-based-organization
---

## Level 3 organization

Now we are ready to review the organization structure at the third level.
If you've missed my previous posts, recommend reading them first before
continuing further:

* [AngularAtom Component-based organization - Introduction]({% post_url 2014-12-02-angular-1-component-organization-1 %}) (Part 1)
* [AngularAtom Component-based organization - Level 1 Organization]({% post_url 2014-12-11-angular-1-component-organization-2 %}) (Part 2)
* [AngularAtom Component-based organization - Level 2 Organization]({% post_url 2014-12-12-angular-1-component-organization-3 %}) (Part 3)

### `app/components/` folder

This folder contains all application components. A _component_ is perceived as
a single isolated feature or a single piece of application functionality.
They are the building blocks of an app. Each application contains one or more
components. Components are stateless and can be reused by other components. This is
somewhat similar to the concept of features in Agile/XP methodology.

Examples of such components are:

* `auth`&mdash;enables application to authenticate users (authn) and control their access (authz)
* `profile`&mdash;enables profile management in an application
* `site-search`&mdash;enables search within an application
* `dashboard`&mdash;gives ability to users to manage application dashboard(s)
* ...

<!--more-->

There is no hard set requirement on how you break down your app into components.
A _component_ may be a distinctive application feature, or it
may be a UI widget, or a service that other components depend on. As long as
they are stateless, reusable and compose your entire app functionality, the
granularity at which you break your app into components is up to you to decide
as an application designer. Any developer, looking inside the `components/`
directory should clearly see what functionality your application consists of.

{% gist cbbf605db31e7c9f5cf6 angularjs-1-component-organization-l3-components.sh %}

* `[component-n]/`&mdash;contains all resources necessary for this component to
function. May contain sub-components. Sub-components are perceived as integral
part of components and they do not create their own
modules. Instead, they define their methods (controllers, directives, services etc.)
on the parent component's module itself.

* `helpers/`&mdash;an optional component reserved for the helper utility functions usually
implemented as [pure functions](http://en.wikipedia.org/wiki/Pure_function).
Alternatively, you may choose to create a separate component for each of these
functions in lieu of the common `helpers/` folder. Whatever works best from your app's
perspective.

### `app/layouts/` folder

This folder contains partial files individually describing each layout supported
by the application. Layouts are injected into the main `app/index.html` by the
abstract states. Your public section, for example, may inject one layout
(ex. public.html) from the `app.public` abstract state, whilst the secure
section may inject a completely different layout (ex. secure.html) from
the `app.secure` abstract state.

Even if your application supports only one layout at the moment,
I still recommend creating this folder and describing your single layout in
`default.html` file. This is a very flexible approach that will allow you to
additional layouts in the future, if needed.

{% gist cbbf605db31e7c9f5cf6 angularjs-1-component-organization-l3-layouts.sh %}



### `app/states/` folder

This folder contains a hierarchy of all application UI states.
A _state_ corresponds to a "place" in the application in terms of the overall UI
and navigation. A state describes (via the controller/template and view properties)
what the UI looks like and does at that place. Your application UI can
transition from one state to another when certain event is triggered. UI can be in
only one state at a time (a.k.a current state), but child state still activates its parents.
[Read more on _states_...](https://github.com/angular-ui/ui-router/wiki)

I strongly recommend creating a UI state diagram prior to coding states. The directory
structure inside your `app/states/` folder will mimic the hierarchy of that
UI state diagram. Any developer, looking inside the `states/` directory should
get a clear idea what UI states your application supports.

{% gist cbbf605db31e7c9f5cf6 angularjs-1-component-organization-l3-states.sh %}

* `[ui-state-n]/`&mdash;contains necessary UI resources (e.g. ui-view templates,
ui-view controllers, images, CSS etc.) to render this particular
UI state. Each state may contain one or more child states that, unlike sub-components,
each define their own module. This is because each state, be it parent or child,
is looked at as an independent citizen that can be added or removed
at any time of the application lifecycle.

### Unit tests

Unit tests are suffixed with `_spec` and placed alongside each file they are testing.
There is no separate folder for the unit tests. This is done intentionally,
so it is easier to locate the existing and the missing unit tests.

I recommend [Mocha](http://mochajs.org/) framework with
[Chai assertion library](http://chaijs.com/) for writing all of your specs.

## Conclusion

This concludes my posts on the "AngularAtom&mdash;the component-based organization of
AngularJS 1.x apps" topic. The entire component-based organization directory
structure can be viewed in the
"**tl;dr**" section of the
[introduction post]({% post_url 2014-12-02-angular-1-component-organization-1 %}).
Make sure to check out the original AngularJS PhoneCat app converted to the [new
AngularAtom component-based structure](https://github.com/demisx/angular-phonecat-components).

I hope you found this useful in organizing your Angular 1.x apps.
I may be making further updates to this organization structure based on the feedback I get,
so please make sure to share your thoughts in the comments below.

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
