---
layout: post
title: Dynamic Accumulator Pattern
description: >
  I demonstrate an improved accumulator pattern when we don’t know ‘what’ we are counting yet.
sitemap: true
comments: true
---

Before I start, I wanted to indicate that this post is primarily targeted
towards my Thinkful engineering students.

By now, we’ve seen the accumulator pattern, and we can now accumulate a specific
set of items. But what if we don’t know beforehand that list of things, or if
the quantity of variables needed to count a particular set of items is a large
number?

Let me introduce a problem I'll solve using this new pattern.

## An example problem

We are filling Noah’s ark. However, whoever gathered all the animals forgot that
we need a pair of each animal. Please help me find all the single animals
missing a mate. At most, there will be 2 of each animal.

Please write a function, `findSingleAnimals`, that will take in an array of
non-predetermined animals and return an array of single animals.

### example

```javascript
let animals = ["dog", "cat", "dog", "sheep", "cow", "cow"];
console.log(findSingleAnimals(animals)); //>> ['cat', 'sheep']
```

## A new approach

First, we need some data structure to store a non-predetermined list of items,
and both arrays and objects come to mind. I’m choosing not to use an array
because the index needs to be a number, limiting our options. Objects, on the
other hand, can use strings or variables that can be
[coerced](https://developer.mozilla.org/en-US/docs/Glossary/Type_coercion) to a
string this gives us a lot more flexibility.

In the examples ahead, I’ll be writing a helper function, countAnimals this
function is an example of the dynamic accumulation pattern. I’ll start with the
basic version of my algorithm. Then I’ll refactor the code step by step.
[TL;DR here is the final version](#final-version-of-the-code)

### Initial Solution Code

```javascript
function findSingleAnimals(listOfAnimalsInArk) {
  // create a counter object from listOfAnimalsInArk
  const animalCounterObj = countAnimals(listOfAnimalsInArk);
  return Object.keys(animalCounterObj).filter(
    // only return an array of animals that have a count of 1
    (animalKeyStr) => animalCounterObj[animalKeyStr] === 1
  );
}

function countAnimals(animalArr) {
  // create an empty accumulation object
  const animalCounterObj = {};
  // loop through the array and count each item
  for (let idx = 0; idx < animalArr.length; idx++) {
    const animalName = animalArr[idx];
    if (animalCounterObj[animalName] === undefined) {
      // we've not counted this one yet, so lets count starting at 1
      animalCounterObj[animalName] = 1;
    } else {
      // otherwise add 1
      animalCounterObj[animalName]++;
    }
  }
  // return the new object of counts
  return animalCounterObj;
}
```

### First refactor

At 17 lines, including the comments, the `countAnimals` function is a bit
lengthy. Whenever I turn an array into a single variable, I immediately think of
the reduce method. I also think reduce is more semantic as developers know what
it does in loose terms without having to explain it. So lets implement it. Which
reduces the function from 17 lines to 14

```javascript
function findSingleAnimals(listOfAnimalsInArk) {
  // create a counter object from listOfAnimalsInArk
  const animalCounterObj = countAnimals(listOfAnimalsInArk);
  return Object.keys(animalCounterObj).filter(
    // only return an array of animals that have a count of 1
    (animalKeyStr) => animalCounterObj[animalKeyStr] === 1
  );
}

function countAnimals(animalArr) {
  // return the new object of counts
  return animalArr.reduce((animalCounterObj, animalName) => {
    // count each item
    if (animalCounterObj[animalName] === undefined) {
      // we've not counted this one yet, so lets count starting at 1
      animalCounterObj[animalName] = 1;
    } else {
      // otherwise add 1
      animalCounterObj[animalName]++;
    }
    return animalCounterObj;
  }, {}); // create an empty accumulation object
}
```

### Second refactor

Well, at 14 lines, I still think we can improve this function. Whenever I see an
if-else statement being used to assign a value to the same variable in both
conditions, I think of using the conditional or ternary operator `?`.
This also has the advantage of reducing the function from 14 lines to 11.

```javascript
function findSingleAnimals(listOfAnimalsInArk) {
  // create a counter object from listOfAnimalsInArk
  const animalCounterObj = countAnimals(listOfAnimalsInArk);
  return Object.keys(animalCounterObj).filter(
    // only return an array of animals that have a count of 1
    (animalKeyStr) => animalCounterObj[animalKeyStr] === 1
  );
}

function countAnimals(animalArr) {
  // return the new object of counts
  return animalArr.reduce((animalCounterObj, animalName) => {
    // count each item
    animalCounterObj[animalName] =
      animalCounterObj[animalName] === undefined
        ? 1 // we've not counted this one yet, so lets count starting at 1
        : animalCounterObj[animalName] + 1; // otherwise add 1
    return animalCounterObj;
  }, {}); // create an empty accumulation object
}
```

### Final version of the code

While you can use any of the above versions, I like the following one because it
is quickest to write and is very semantic to someone who is up to date on their
JS knowledge. I'll be using the following features of destructuring that wasn't
covered in the course.

1. [Destructuring objects to a new variable name](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Assigning_to_new_variable_names)
   instead of the default variable name from destructuring
2. [Assigning default values when destructuring](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Default_values_2)
3. [Destructuring of a dynamic property](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#Computed_object_property_names_and_destructuring)
   using a variable or string

Using these techniques, we've reduced the countAnimals function from 11 lines to
just 9.

```javascript
function findSingleAnimals(listOfAnimalsInArk) {
  // create a counter object from listOfAnimalsInArk
  const animalCounterObj = countAnimals(listOfAnimalsInArk);
  return Object.keys(animalCounterObj).filter(
    // only return an array of animals that have a count of 1
    (animalKeyStr) => animalCounterObj[animalKeyStr] === 1
  );
}

function countAnimals(animalArr) {
  // return the new object of counts
  return animalArr.reduce((animalCounterObj, animalName) => {
    // use object destructuring to variable name countOfAnimal
    const { [animalName]: countOfAnimal = 0 } = animalCounterObj; //default to 0
    animalCounterObj[animalName] = countOfAnimal + 1; // count each animal
    return animalCounterObj;
  }, {}); // create an empty accumulation object
}
```

## Summary

We’ve successfully created a function that uses this new dynamic accumulator
pattern. We’ve also refactored it to a shorter function, reducing the lines of
code and redundant comments. The method, having been reduced from 17 lines to 9,
is just a smidgen over half the original size.

## Bonus Version Using New Features from 2020 Release of JS

While you can use any of the above versions, I like the following one because it
is quickest to write and is very semantic to someone who is up to date on their
JS knowledge. Specifically, we will be using a feature from ES2020, Javascript's
latest standard at the time of this writing. The feature we will be using is the
[Nullish Coalescing Operator, ??](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator).
Well, at 9 lines we still can further shorten this function by implementing the
`??` operator. And with that we are now down to 8 lines including comments.

Beware for Qualified assignments, the nullish operator may not work as I am
uncertain of the Node version they use. At this time to use the nullish
operator, as per MDN, we need to use a
[minimum version of node 14](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator#Browser_compatibility).
{:.note}


Luckily in the Thinkful course, at the time of writing, you’ve installed version
a minimum version of 14.15.3, therefore we can use it for your for sure on
non-qualified assignments or projects!
{:.note}


```javascript
function findSingleAnimals(listOfAnimalsInArk) {
  // create a counter object from listOfAnimalsInArk
  const animalCounterObj = countAnimals(listOfAnimalsInArk);
  return Object.keys(animalCounterObj).filter(
    // only return an array of animals that have a count of 1
    (animalKeyStr) => animalCounterObj[animalKeyStr] === 1
  );
}

function countAnimals(animalArr) {
  // return the new object of counts
  return animalArr.reduce((animalCounterObj, animalName) => {
    // count each item defaulting the count to zero
    animalCounterObj[animalName] = (animalCounterObj[animalName] ?? 0) + 1;
    return animalCounterObj;
  }, {}); // create an empty accumulation object
}
```
