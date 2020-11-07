# Setting up Cucumber-JS without the browser
**Author:** [Alan Mills]
**Date:** [15 June 2017 10:51]
**Tags:** [Refactoring]
**Status**: Draft

The [cucumber-js nodejs_example.md](https://github.com/cucumber/cucumber-js/blob/master/docs/nodejs_example.md) is written with the assumption that you want to drive the UI using Chrome and Selenium.

There is a nice [in-browser](http://cucumber.github.io/cucumber-js/) example of using Cucumber for a simple maths example.  How do we run this from the command line?


************
[Todd Anderson, BDD in JavaScript: CucumberJS](https://www.custardbelly.com/blog/blog-posts/2014/01/08/bdd-in-js-cucumberjs/index.html)
[Mark Denford, Cucumber.js tutorial](https://denford.me/cucumber-js-tutorial-cfb053fe3e7)
[BrowserStack, Cucumber JS](https://www.browserstack.com/automate/cucumberjs)
************


``` bash
mkdir simplemaths
cd simplemanths
npm init
    test: cucumber-js
npm install cucumber --save-dev
mkdir features features/step_definitions
vim features/simple_maths.features
:w
:!npm test
:e features/step_definitions/simple_maths_steps.js
:w
:!npm test
```

## simple_maths.feature
``` javascript
Feature: Simple maths
  In order to do maths
  As a developer
  I want to increment variables

  Scenario: easy maths
    Given a variable set to 1
    When I increment the variable by 1
    Then the variable should contain 2

  Scenario Outline: much more complex stuff
    Given a variable set to <var>
    When I increment the variable by <increment>
    Then the variable should contain <result>

    Examples:
      | var | increment | result |
      | 100 |         5 |    105 |
      |  99 |      1234 |   1333 |
      |  12 |         5 |     18 |
```

## simple_maths_steps.js
``` javascript
Cucumber.defineSupportCode(function(context) {
  var setWorldConstructor = context.setWorldConstructor;
  var Given = context.Given
  var When = context.When
  var Then = context.Then

  ///// Your World /////
  //
  // Call 'setWorldConstructor' with to your custom world (optional)
  //

  var CustomWorld = function() {};

  CustomWorld.prototype.variable = 0;

  CustomWorld.prototype.setTo = function(number) {
    this.variable = parseInt(number);
  };

  CustomWorld.prototype.incrementBy = function(number) {
    this.variable += parseInt(number);
  };

  setWorldConstructor(CustomWorld);

  ///// Your step definitions /////
  //
  // use 'Given', 'When' and 'Then' to declare step definitions
  //

  Given(/^a variable set to (\d+)$/, function(number) {
    this.setTo(number);
  });

  When(/^I increment the variable by (\d+)$/, function(number) {
    this.incrementBy(number);
  });

  Then(/^the variable should contain (\d+)$/, function(number) {
    if (this.variable != parseInt(number))
      throw new Error('Variable should contain ' + number +
        ' but it contains ' + this.variable + '.');
  });
})
```