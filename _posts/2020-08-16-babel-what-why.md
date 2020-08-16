---
layout: post
title: Babel, What is it? Why use it?
description: >
  A introduction of Babel for JavaScript
sitemap: true
comments: true
---

- this unordered seed list will be replaced by the toc
{:toc}

# What and why?

A short description. [Babel][1] is a JavaScript tool that compiles code into JavaScript that is compatible with one or more specified environments. While Babel, can work with input languages other than JavaScript, in this article, I will be focusing on JavaScript to JavaScript compiling.

Why would anyone want to go from one language, back to the same language? That is because while JavaScript doesn't have versions, it has [specifications][2], it is up for the consuming JavaScript engine to implement the features in the specifications. The various engines implement different features of the specs in different orders and on different timelines. These days, a new specification is released annually. We, as developers, want our creations to work in as many environments as possible, even in environments that past their end of life, for example, Windows 7's Internet Explorer 11.

Some developers will check [Can I Use][3] religiously to see if they can use a new feature they heard about only to be dismayed that there are people using browsers (or a Node environment) that doesn't support it. These developers get left behind in their skills. They can't practice the new stuff because their target environment(s) have yet to be updated. This is where Babel comes in. You can instruct Babel, to take in a file with all the new features that have been in the latest released specs, and have it output a new file that will support the targeted environments. Babel uses several techniques from implementing polyfills to code substitutions.

# Some Examples

For the following examples, I opted to have Babel target the browsers and their versions that make up 95% of the active browsers as tabulated by Can I Use's [browser usage table][4].

The environment selection is quite flexible, we can target a NodeJS, Electron, or browser environment. Within those, we can target specific versions. Specifically for browsers, we can target base on usage percentages.
{:.note}

## Example 1: Array destructuring with the spread operator, and Object destructuring

in: example1.js

```JavaScript
const array = [1, 2, 3, 4];
const [first, second, ...theRest] = array;
console.log(first); // > 1
console.log(second); // > 2
console.log(theRest); // > [3, 4]

const person = { name: "David", age: 21 };
const { name: theName, age } = person;
console.log(`${theName} is ${age} years old!`); // > David is 21 years old!
```

out: example1.js

```JavaScript
"use strict";
require("core-js/modules/es.array.concat");
require("core-js/modules/es.array.slice");

var array = [1, 2, 3, 4];
var first = array[0],
  second = array[1],
  theRest = array.slice(2);
console.log(first); // > 1
console.log(second); // > 2
console.log(theRest); // > [3, 4]

var person = {
  name: "David",
  age: 21,
};
var theName = person.name,
  age = person.age;
console.log("".concat(theName, " is ").concat(age, " years old!")); // > David is 21 years old!
```

## Example 2: Function using object destructuring with default value as an input

in: example2.js

```JavaScript
const reportMostWantedPerson = ({
  name = "John Doe",
  age = "Undetermined",
}) => {
  console.log("New most wanted person - Name: " + name + " Age: " + age);
};

const firstPerson = { age: 25 };
let secondPerson = { name: "Jason Smith" };
var thirdPerson = { name: "Michael Kupe", age: 31 };

reportMostWantedPerson(firstPerson); // New most wanted person - Name: John Doe, Age: 25
reportMostWantedPerson(secondPerson); // New most wanted person - Name: Jason Smith Age: Undetermined
reportMostWantedPerson(thirdPerson); // New most wanted person - Name: Michael Kupe Age: 31
```

out: example2.js

```JavaScript
"use strict";
require("core-js/modules/es.function.name");

var reportMostWantedPerson = function reportMostWantedPerson(_ref) {
  var _ref$name = _ref.name,
    name = _ref$name === void 0 ? "John Doe" : _ref$name,
    _ref$age = _ref.age,
    age = _ref$age === void 0 ? "Undetermined" : _ref$age;

  console.log("New most wanted person - Name: " + name + " Age: " + age);
};

var firstPerson = {
  age: 25,
};
var secondPerson = {
  name: "Jason Smith",
};
var thirdPerson = {
  name: "Michael Kupe",
  age: 31,
};

reportMostWantedPerson(firstPerson); // New most wanted person - Name: John Doe, Age: 25
reportMostWantedPerson(secondPerson); // New most wanted person - Name: Jason Smith Age: Undetermined
reportMostWantedPerson(thirdPerson); // New most wanted person - Name: Michael Kupe Age: 31
```

[1]: https://babeljs.io/
[2]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Language_Resources
[3]: https://caniuse.com/
[4]: https://caniuse.com/usage-table
