# Service Portal: Angular Style Guide

## Purpose

A sensible & opinionated Angular style guide for teams using Service Portal.

*Based off of the [Angular 1 Style Guide](https://github.com/johnpapa/angular-styleguide/tree/master/a1) by [@john_papa](http://twitter.com/john_papa) and up-to-date with Angular 1.5 components.*

## Table of Contents

  1. [IIFE](#iife)
  1. [Modules](#modules)
  1. [Components](#components)
  1. [$onInit](#oninit)
  1. [One-way Binding](#one-way-binding)
  1. [Named Controllers](#named-controllers)
  1. [Controller as Syntax](#controller-as-syntax)
  1. [Bindable Members at Top](#bindable-members-at-top)
  1. [Defer Logic to Services](#defer-logic-to-services)
  1. [File Naming](#file-naming)

## IIFE

  Where possible, wrap components in an (IIFE) Immediately Invoked Function Expression. This removes variables from the global namespace, thus preventing variable collisions and providing an enclosed scope.

  ```javascript
  (function() {
    'use strict';

    function eventsService() {}

    angular
      .module('pe-timeline')
      .service('eventsService', eventsService);
  })();
  ```

**[Back to top](#table-of-contents)**

## Modules

  Set a module only once in it's own file.

  ```javascript
    angular.module('pe-timeline', []);
  ```

  Get instances of the module this way.

  ```javascript
    angular.module('pe-timeline');
  ```

**[Back to top](#table-of-contents)**

## Components

  Angular 1.5 brings the new .component() helper method, which allows developers to write in an Angular 2 style and makes upgrading to Angular 2 an easier task. Here are the advantages from the Angular documentation:

  * simpler configuration than plain directives
  * promote sane defaults and best practices
  * optimized for component-based architecture
  * writing component directives will make it easier to upgrade to Angular 2

  ```javascript
  (function() {
    'use strict';

    var hamburgerMenu = {
      template: [
        '<button type="button" data-target="#nav-bar">',
        '<span class="sr-only">${Toggle nav}</span>',
        '<span class="icon-bar"></span>',
        '<span class="icon-bar"></span>',
        '<span class="icon-bar"></span>',
        '</button>'
      ].join('')
    };

    angular
      .module('sec-ops')
      .component('hamburgerMenu', hamburgerMenu);
  })();
  ```

**[Back to top](#table-of-contents)**

## $onInit

Angular 1.5 brings the $onInit method. This is a good place to put initialization code for your controller. From the documentation:

*Called on each controller after all the controllers on an element have been constructed and had their bindings initialized (and before the pre & post linking functions for the directives on this element).*

  ```javascript
  function TabSelectionController() {
    var c = this;

    c.$onInit = function() {
      c.selection = 1;
      c.isVisible = false;
    };
  }
  ```

  Another great use case for utilizing $onInit, is when you need a place to fire off an immediately invoked function.

  ```javascript
  c.$onInit = function() {
    activateCharts();
    setDefaults();
  };
  ```

**[Back to top](#table-of-contents)**

## One-way Binding

  Angular 1.3 delivered a new performance enhancing feature called one-time binding. From the Angular documentation:

  *One-time expressions will stop recalculating once they are stable, which happens after the first digest if the expression result is a non-undefined value.*

  ```html
  <div class="panel-heading">{{::options.title}}</div>
  ```

  The syntax can easily be applied inside an ng-repeat as well:

  ```html
  <ul>
    <li ng-repeat="user in ::c.users"></li>
  </ul>
  ```
  - Note: Be careful using one-way data-binding in areas where the data could fluctuate in the future.

**[Back to top](#table-of-contents)**

## Controller as Syntax

  The Controllers as Syntax was introduced in Angular 1.2. Use this syntax instead of $scope inside your controller. Why Controller as?

   * it is a common and best practice
   * removes scope inheritance issues
   * eliminates having to inject $scope as a dependency
   * $scope is phased out in Angular 2 and beyond

  Every time you create a new widget the platform provides this syntax for you by default, which is nice.

  ```javascript
  function() {
    var c = this;
    c.isVisible = true;
  }
  ```

  Try to avoid this, if possible.

  ```javascript
  function($scope) {
    $scope.isVisible = true;
  }
  ```

**[Back to top](#table-of-contents)**

## Named Controllers

  Use named controllers instead of anonymous functions. Why named controllers?

   * easier to debug
   * creates more readable code

  Also, be sure to append the controller name with the suffix Controller. This is a good practice that is commonly used and is descriptive.

  ```javascript
    function PeopleCardController() {
      var c = this;
    }
  ```

  Try to avoid this if possible, despite the fact that every time you create a new widget the platform provides you with an anonymous function by default.

  ```javascript
  function() {
    var c = this;
  }
  ```

**[Back to top](#table-of-contents)**

## Bindable Members at Top

  Place bindable members at the top of the context in alphabetical order. This makes it easy to read and at a glance you know exactly which members are bound to the view and where to find the implementation details.

  ```javascript
  function FeedbackCardController() {
    var c = this;
    c.closeSurvey = closeSurvey;
    c.setRating = setRating;

    function closeSurvey() {
      /* */
    }

    function setRating() {
      /* */
    }
  }
  ```

**[Back to top](#table-of-contents)**

## Defer Logic to Services

  Defer controller logic to services or factories. Why you say?

   * keeps the controller focused and lean
   * promotes reuse via dependency injection
   * reduces repetitive controller logic
   * ideal for uniting testing and mocking

   Create a service and simply inject it into the controller.

  ```javascript
  function TimelineController(eventsService) {
    var c = this;

    c.$onInit = function() {
      c.initialEvents = eventsService.getInitialEvents();
    };
  }
  ```

  Read more on the topic, from the man John Papa.

  [https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md#defer-controller-logic-to-services](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md#defer-controller-logic-to-services)

**[Back to top](#table-of-contents)**

## File Naming

  Keep it simple by going lowercase with dashes. Use the feature or widget to name the file, separated by a period with the file type following it and ending with the file extension. Why?

   * consistency
   * makes finding your code faster
   * ease of understanding

  Here are a list of examples.

  ```
  people-card.server.js
  people-card.module.js
  people-card.component.js
  people-card.service.js
  people-card.directive.js
  people-card.filter.js
  people-card.spec.js
  people-card.html
  people-card.scss
  ```

  Read more on the topic, from Mr. Papa.

  [https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md#naming](https://github.com/johnpapa/angular-styleguide/blob/master/a1/README.md#naming)

**[Back to top](#table-of-contents)**