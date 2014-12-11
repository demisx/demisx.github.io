---
layout: post
title:  "Unit testing AngularJS child directive that requires parent directive"
date:   2014-10-28 21:45:00
categories: angularjs unit-testing
---

In AngularJS we can have one or more directives communicate with each other. This
gives us an ability to create web components (hello AngularJS 2.0) that are built
from multiple directives. In order to for a directive A to access the API methods
defined in a controller of a directive B, the latter needs to define the `require:`
property with a value set to the name of directive A and specify how the search
should be performed (in my example, I prefix directive name with `^^`, so the
search for controlelr is done on the parent DOM elements only). The compilation
of directive B will fail if directive A controller cannot be found in the "search path".

<!--more-->

<div class="alert alert-warning">
<i class="fa fa-bell-o fa-2x"></i>
  Please note, when using `require` property, the directives will share
  <b>the same instance</b> of the controller.
</div>

Here is an example of a directive B requiring presence of directive A controller
on a parent DOM element:

{% highlight coffeescript linenos %}
angular.module 'app.directives', []

.directive 'directiveA', ->
  restrict: 'E'
  controller: ->
    @add = (x,y) ->
      x+y

.directive 'directiveB', ->
  restrict: 'E'
  require: '^^directiveA' # search for directive A controller on the parent elements
  link: (scope, elem, attrs, ctrl) ->
    # let's define some function on directive B scope
    # that leverages the business logic of directive A
    scope.letsAdd = (x,y) ->
      ctrl.add x,y
    return
{% endhighlight %}

And let's assume these two directives have simple "parent-child" relationship in HTML:

```html
<directive-a>
  <directive-b></directive-b>
</directive-a>
```

The issue comes in when we want to unit test directive B in isolation, without
compiling directive A and everything that may come with it.
If we try to test directive B just as a standalone directive, the compilation
will fail as expected with the
`Controller 'directiveA', required by directive 'directiveB', can't be found!`
error due to the `require: ^^directiveA` property of the directive B.

To eleminate this error and still unit test directive B in isolation,
we need to mock the directive A controller and add it
to the data store of some parent DOM element, e.g. `<fake-parent>` (see below).
We don't care what this parent is, as long as we can put a controller mock into
its data strore (via `.data` method).

Note, the naming convention for the controller key looked up by Angular is:

`'$' + name of directive + 'Controller'`.

Therefore, in order for Angular to find the mocked directive A controller
and have our test pass, we'd need to associate this mock
with `$directiveAController` key in the parent element data store:

{% highlight coffeescript linenos %}
it.only 'ensures letsAdd can add two numbers', ->
  module 'directives.cdSlideviewer' # this usually goes into beforeEach block

  # mock directive A controller and spy on 'add' method
  directiveAControllerMock =
    add: ->
      123 # you can return anything here, since we are not unit testing it

  # create Sinon spy
  mySpy = sinon.spy directiveAControllerMock, 'add'

  # create angular element from HTML string
  elem = angular.element '<fake-parent><directive-b><directive-b></fake-parent>'

  # put controller mock into <fake-parent> data store
  elem.data '$directiveAController', directiveAControllerMock

  # compile element
  inject ($compile, $rootScope) ->
    $compile(elem) $rootScope.$new()
    return

  # retrieve directive B element and its scope
  elem = elem.find 'directive-b'
  scope = elem.scope()

  # call method under test
  scope.letsAdd 3, 4

  # assert that our spy was called with proper arguments
  expect(mySpy).to.have.been.calledWith 3, 4 #
  return
{% endhighlight %}

The directive A controller is found and the test happily passes. <i class="fa fa-smile-o fa-lg" style="color:green"> </i>

This was a contrived example inteded to demonstrate a high level approach on
how one can remove a dependency on other directives during a unit test. Use
[js2coffe](http://js2coffee.org/) if you need to convert my examples written in
CoffeeScript to vanilla JavaScript and then learn CoffeeScript.</p>

<a class="twitter-follow-button"
  href="https://twitter.com/demisx1"
  data-show-count="false"
  data-lang="en"
  data-size="large">
</a> for more tips and best practices.

___

Today my environment was:

- CoffeeScript 1.8.0
- AngularJS 1.3.0
- Node.js 0.10.31
- Mocha 1.21.4 / Chai 1.9.2
