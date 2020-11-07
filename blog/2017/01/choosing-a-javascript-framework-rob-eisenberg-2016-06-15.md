Choosing a JavaScript framework - Rob Eisenberg (2016-06-15)
============================================================
**Author:** Alan Mills  
**Date:** [16 January 2017 12:47]
**Tags:** [JavaScript]
**Status**: Draft

NDC { Oslo } have posted a video on YouTube that Rob Eisenberg presented on [Choosing a JavaScript Framework, A Look at Six Popular Options](https://youtu.be/6I_GwgoGm1w).

Rob has a long history with UI frameworks as the creator of Caliburn Micro, Durandal, a former Google employee working on Augular 2 and Augular Material.  And at the time of the recording the CEO of Durandal Inc and Architect of Aurelia.

Rob makes a call to understand the business needs, team, culture, etc before making a decision on what tools you should use to solve the problem.

The frameworks that he compared are:
* [AngularJS 1.x](angularjs 1.x)
* [Angular 2](angular 2)
* [Aurelia](aurelia)
* [Ember](ember)
* [Polymer](polymer)
* [React](react)

### AngularJS 1.x
Key features
* ES1.5 and therefore uses a fluent API with lots of 'strings' to match the declarations of different components.
* Uses **ng-model** to perform two-way data binding
* Simple and it works

### Angular 2
Key features
* Pushes you down the TypeScript road
* Have to use a module loader
* Uses the class syntax to declare components
* Uses a template to output HTML with **ngModel** bindings

### Aurelia
Key features
* Uses ES6 (Babel)
* Uses the HTML web components specification
* has a value.bind to perform two-way databinding
* Very similar to Angular 2

### Ember
Key features
* Ember is a strict MVC frameworks
* You have to have the router where as the other frameworks do not require this until you need it.

### Polymer
Key features
* Is based on web components and tries to do everything in the DOM
* has two way data-binding but uses a different syntax `value="{{firstName::input}}"`
* uses the `<script>` tag within the html file to implement the required behavior.
* Rob is concerned this will not work for more complicated projects; he reflected on a similar approach Microsoft took to a previous framework that failed.

### React
Key features
* A view only framework
* Uses a custom language **JSX**
* React uses a one-way data-binding, the other frameworks have two-way data-binding.  Therefore, you have to respond to the event if you want to get the value back from the user.

Technical
---------
### Size
React is the smallest however as it is a view only framework, the size can be undertermanistic as you add on other capabilities.
