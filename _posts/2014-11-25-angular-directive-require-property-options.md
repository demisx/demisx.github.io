---
layout: post
title:  "Angular 1.x Directive Definition Object: `require` property"
date:   2014-11-25 13:59:00
categories: angularjs directives
---

As we all know, the DDO (Directive Definition Object) returned by the Angular 1.x directive callback
function has a `require` property, that sort of establishes dependency of one
directive on others. This is a convenient way of building components that
contain multiple directives where one directive may depend on the functionality
of others. In practice, the `require` property simply tells Angular compiler
where a particular directive controller should be search for.

<!--more-->

Recently, reviewing AngularJS 1.3 source code, I've realized that there were some
additional options for the `require` property besides what I already knew about.

<div class="alert alert-warning">
<i class="fa fa-bell-o fa-2x"></i>
  The way directives currently "require" other directives will be obsolete in Angular 2.0
  along with many other things. It is my understanding that the preferred way of
  directive communication will be via Dependency Injection of one directive into another.
</div>

Let's take this trivial example of defining a UI "Tabs" component in Angular via two directives:

{% highlight javascript linenos %}

angular.module('components.tabs', [])
.directive('tabset', function() {
  return {
    restrict: 'EA',
    transclude: true,
    scope: {},
    controller: function($scope) {
      // ...
    }
  }
}).directive('tab', function() {
  return {
    restrict: 'EA',
    require: '^tabset'
    link: function (scope, elem, attrs, ctrl) {
      var tabsetController = ctrl;
      // ...
    }
  }
});

{% endhighlight %}

What `require: '^tabset'` line does here is tells Angular compiler that our `tab`
directive "depends" on the `tabset` directive via controller defined in the `tabset`
directive. When `require` property is present, Angular attempts to locate this
controller object in the search path and then inject it into the `tab` directive's
`link` function as the fourth argument (please note, when using `require` property,
both directives share the same instance of the controller). Where exactly Angular searches for
this controller object depends on the prefix of the directive name (e.g. `^` in this case)
given in the `require` property value of DDO.

Here is a list of all prefixes supported by the `require:` property and their
corresponding effect on the search path:

<style>
  table#angular-table > tbody > tr > td:nth-child(1) {
    width: 20%;
    padding-left: 20px;
  }
  table#angular-table > tbody > tr > td:nth-child(5) {
    width: 30%;
  }
  table#angular-table > tbody > tr > td:nth-child(1) > ol {
    padding-left: 20px;
  }
  table#angular-table > tbody > tr > td li {
    padding-bottom: 5px;
  }

  table#angular-table > tbody > tr > td:nth-child(1) {
    font-weight: bold;
  }

  table#angular-table > tbody > tr > td:nth-child(1),
  table#angular-table > tbody > tr > td:nth-child(3),
  table#angular-table > tbody > tr > td:nth-child(4) {
    text-align: center;


  }
  table#angular-table > tbody > tr > td:nth-child(4) {
    white-space: nowrap;
    font-family: Menlo,Monaco,Consolas,"Courier New",monospace;
    color: #666;
    font-size: 0.8em;
  }
</style>

<table id="angular-table" class="table table-striped table-bordered">
  <tr>
    <th class="text-center">Prefix</th>
    <th class="text-center">Search path</th>
    <th class="text-center">Error raised if controller not found?</th>
    <th class="text-center">Example</th>
    <th class="text-center">Link function receives as 4<sup>th</sup> argument</th>
  </tr>
  <tr>
    <td>
      (no prefix)
    </td>
    <td>Current DOM element that directive is applied to</td>
    <td>Yes</td>
    <td>
      tabset
    </td>
    <td>
      Controller object, if found.
    </td>
  </tr>
  <tr>
    <td>
      ?
    </td>
    <td>Current DOM element that directive is applied to</td>
    <td>No</td>
    <td>
      ?tabset
    </td>
    <td>
      Controller object, if found, `null` otherwise
    </td>
  </tr>
  <tr>
    <td>
      ^
    </td>
    <td>Current DOM element that directive is applied to and its parents</td>
    <td>Yes</td>
    <td>
      ^tabset
    </td>
    <td>
      Controller object, if found.
    </td>
  </tr>
  <tr>
    <td>
      ^^
    </td>
    <td>Parent elements of the DOM element that the directive is applied to</td>
    <td>Yes</td>
    <td>
      ^^tabset
    </td>
    <td>
      Controller object, if found.
    </td>
  </tr>
  <tr>
    <td class="text-center">
      ?^
    </td>
    <td>Current DOM element that directive is applied to and its parents</td>
    <td>No</td>
    <td>
      ?^tabset<br/>
      ^?tabset
    </td>
    <td>
      Controller object, if found, `null` otherwise
    </td>
  </tr>
  <tr>
    <td>
      ?^^
    </td>
    <td>Parent elements of the DOM element that the directive is applied to</td>
    <td>No</td>
    <td>
      ?^^tabset<br/>
      ^^?tabset
    </td>
    <td>
      Controller object, if found, `null` otherwise
    </td>
  </tr>
</table>

As you've probably already noticed, the `?` in the prefix simply makes the search optional, so
the compiler doesn't throw any errors and the `link` function simply receives
`null` as the fourth argument, if such controller is not found.

Also, worth noting that if multiple directives are required, the `require`
property of the directive can take an array argument:

{% highlight javascript linenos %}
// ...
.directive('tabs', function() {
  return {
    restrict: "EA",
    require: ['^tabset', 'ngModel'] // <- this directive depends on tabset and ngModel directives
    link: function (scope, elem, attrs, ctrls) {
      var tabsetController = ctrls[0]
      var ngModelController = ctrls[1];
      // ...
    }
  };
});
{% endhighlight %}

In this case, the injected 4th argument in the `link` function will be an array of
controller objects in corresponding order.


<div id='footer-spacing' ></div>
<p class="text-center" style="line-height:20px;">
  <a class="twitter-follow-button"
    href="https://twitter.com/demisx1"
    data-show-count="false"
    data-lang="en"
    data-size="large">Follow @demisx1
  </a> for more tips and best practices.
</p>

<div id='footer-spacing'></div>

___

Today my environment was:

- Angular 1.3.2
- Mac OS X 10.10.1
