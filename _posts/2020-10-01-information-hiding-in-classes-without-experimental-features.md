---
layout: post
title: How to create private class instance variables without using experimental features in JavaScript.
description: >
  I explain how to hide internal functions and variables from the consumer of the class
sitemap: true
comments: true
---

I had a friend tell me the other day that they figured out how to use an API.
What they explained to me is they created that they created the class object as
required, but then they were manipulating properties and/or using functions
that were prefixed with the `_` character. As you may or may not know, there is
an implicit contract dictating that properties that have names beginning with
`_` shall not be accessed, altered, or even invoked. I believe that this implicit
contract isn't always sufficient and it isn't defensive programming. People,
like my friend, can and do break the implicit contract when consuming a library.

There happens to be a [stage 3 proposal](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields)
for private class variables, which is considered experimental, and I won't be
delving into it here. Even though it reached stage 3 in July 2017 it hasn't been
implemented widely yet. For example, Firefox hasn't as per [Can I Use](https://caniuse.com/mdn-javascript_classes_private_class_fields).
I believe we'll be waiting at least another year before, or if, it is added to
final specs. I don't like using experimental features as the syntax can change
at any time. Therefore, I have come up with a way to encapsulate private
instance variables and functions that won't be accessible by the consumers of
the API by using a [higher-order function](https://medium.com/javascript-scene/higher-order-functions-composing-software-5365cf2cbe99).

Below I've created two examples that are functionally the same, albeit the first
example will have additional properties, but the contract is, no one should use
or modify it. In the second example, I used
[Object.defineProperty()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
a cool function that, as its clever name indicates, defines a property on an
object. You can define, setters, getters, and normal class variables (that can
be any variable, object, or function). You can also specify whether a class
variable is writable. I encourage you to read the documentation on it, and it's
sibling function [Object.defineProperties()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties).


### example 1 - Contract

```js
class ContractSecretKeeper {
  constructor(secret, password) {
    // No one should touch or examine this because we have a contract
    // that these are internal variables, and not to be relied upon.
    // Sadly I'm still publicly available.
    this._secret = secret;
    this._password = password;
  }

  getSecret(password) {
    if (password === this._password) {
      return this._secret;
    } else {
      throw new Error("password incorrect");
    }
  }

  set password(newPassword) {
    this._password = newPassword;
    // no no no, you can't get the secret now!
    this._secret = "";
  }

  get password() {
    throw new Error("password access denied");
  }

  set secret(newSecret) {
    this._secret = newSecret;
  }

  get secret() {
    throw new Error("I'll never give up the secret, even if you torture me");
  }
}
const badSecretKeeper = new ContractSecretKeeper("I stole some cookies", "cookieMonster");

console.log(badSecretKeeper); //=> {_secret: "I stole some cookies", _password: "cookieMonster"}
console.log(badSecretKeeper.getSecret("cookieMonster")); //=> I stole some cookies
// Secret? What is so secret? I can get the secret without knowing the password, watch!
console.log(badSecretKeeper.getSecret(badSecretKeeper._password)); //=> I stole some cookies
console.log(badSecretKeeper._password); //=> cookieMonster
console.log(badSecretKeeper._secret); //=> I stole some cookies
```

### 2 Truly hidden data

```js
function functionGenerator(secret, password) {
  // this function is an higher-order function

  // super secret variables available via closure
  let _secret = secret;
  let _password = password;

  /*****
   * Notice 'this' isn't used in any of these 3 functions though if you wanted
   * to, you can pass in the object as a variable to a generator function so you may
   * use class functions and class variables.
   *****/
  return {
    getSecret: (password) => {
      if (password === _password) {
        return _secret;
      } else {
        throw new Error("password incorrect");
      }
    },
    passwordSetter: (newPassword) => {
      _password = newPassword;
      // Just because you now know the password by changing it
      // I still ain't gonna tell you!
      _secret = "";
    },
    secretSetter: (newSecret) => {
      _secret = newSecret;
    },
  };
}

class PrivateSecretKeeper {
  constructor(secret, password) {
    const { passwordSetter, secretSetter, getSecret } = functionGenerator(
      secret,
      password
    );

    // lets put the functions that closed over the secret variables
    // onto 'this'
    this.getSecret = getSecret;
    Object.defineProperty(this, "password", { set: passwordSetter });
    Object.defineProperty(this, "secret", { set: secretSetter });
  }

  get password() {
    throw new Error("password access denied");
  }

  get secret() {
    throw new Error("I'll never give up the secret, even if you torture me");
  }
}

const superSecretKeeper = new PrivateSecretKeeper(
  "I stole some cookies",
  "cookieMonster"
);

console.log(superSecretKeeper); //=> {getSecret: ƒ}
console.log(superSecretKeeper.getSecret("cookieMonster")); //=> I stole some cookies
console.log(superSecretKeeper._secret); //=> undefined
console.log(superSecretKeeper._password); //=> undefined
```
