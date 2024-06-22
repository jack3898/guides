# JavaScript fundamentals: Variables

_Before we begin, I recommend you read this article on GitHub, which has proper code highlighting and extra readability improvements! https://github.com/jack3898/guides/tree/main/js-fundamentals-variables_

A variable in JavaScript lets you store data and give it a name, to pass around and manipulate in your application which is why they are useful.

There are three fundamental types of variables in JavaScript:

- `var`
- `let`
- `const`

This guide will explain the why's and when's for each type of variable.

# `var` type

`var` is the oldest variable type and can be declared like so:

```js
var name = "Hello!";

console.log(hey); // outputs: "Hello!"
```

`var` will not throw an error if you try to use before its definition.

```js
console.log(name); // outputs: undefined

var name = "Hello!";
```

This is because it is hoisted to the top of the scope behind the scenes, and later assigned. The above is equivalent to this:

```js
var name; // Not assigned

console.log(name);

name = "Hello!";
```

If it is defined in a block, (if statement, loop, etc.) you can still access it.

```js
if (condition) {
  var name = "Hello!";
}

console.log(name); // outputs: "Hello!"
```

However, that rule does not apply when the variable lives in a function because `var` is function scoped.

```js
function myFunction() {
  var name = "Hello!";

  console.log(name); // outputs: "Hello!"
}

console.log(name); // throws an error
```

`var` can be reassigned at any time.

```js
var name = "Hello!";

name = "Goodbye";

console.log(name); // outputs: "Goodbye"
```

`var` is highly discouraged for use in any JavaScript application, and you should ONLY use it for backwards compatibility with old browsers. This is due to the quirks listed above, which create room for bugs.

# `let` type

`let` was introduced as part of the ES6 standard in 2015, and it aimed to address the flaws that exist with `var`.

As usual, it can be declared like so:

```js
let name = "Hello!";

console.log(hey); // outputs: "Hello!"
```

It will throw an error if you try to use it before it is defined. This is because it does not hoist like `var`.

```js
console.log(name); // throws a ReferenceError

let name = "Hello!";
```

And, is strictly block-scoped, unlike `var`.

```js
if (condition) {
  let name = "Hello!";
}

console.log(name); // throws a ReferenceError
```

However, `let` can still be reassigned.

```js
let name = "Hello!";

name = "Goodbye";

console.log(name); // outputs: "Goodbye"
```

`let` to me, feels like what `var` should have been from the start!

# `const` type

`const` is very similar to `let`, in that it retains the safety nets `let` has over `var`, however you cannot modify its reference to a value in memory.

It came out at the same time as `let`.

When you create any type of variable in JavaScript, that variable is an identifier to the location of the value in memory. That is what a reference is; a pointer to a value.

When you assign a primitive to a variable, for example any of:

- string
- number
- bigint
- boolean
- undefined
- symbol
- null

The reference to that value in memory is constant. Reassigning `let` or `var` with a new value will create a new reference.

`const` preserves this reference, preventing reassigning. The term "const" means "this reference to this value must remain constant".

```js
const name = "Hello!";

name = "Goodbye"; // throws a TypeError
```

However it is important to note that more complex types like objects and arrays could be considered "containers" to references. When you create a `const` variable and assign it a non-primitive value, it essentially becomes a reference to a container of references.

A `const` only cares about the direct reference to the object in memory, so it is safe to change the internal references of an object even with a `const`.

```js
const name = {
  message: "Hello!",
};

name.message = "Goodbye";

console.log(name.message); // outputs "Goodbye"
```

So, a `const` is not strictly constant.

If you use TypeScript, you can add this extra layer of protection with `as const`, which make an object purely readonly at compile time (but not at runtime still). It's probably the most constant thing you can get!

```ts
const name = {
  message: "Hello!",
} as const;

name.message = "Goodbye"; // does not throw any errors (as it compiles to JavaScript), but will give a compile-time error
```

A `const` must be assigned when it is declared. Unlike, `var` or `let`. When you think about it, this makes sense. Why would you not provide a value to something that cannot be reassigned?

```js
var variable1; // undefined
let variable2; // undefined
const variable3; // throws a SyntaxError
```

`const` encourages "declarative" over "imperative" programming, which is a pattern that describes what a value is rather than the instructions needed to work it out. For example:

Imperative:

```js
let numArr = [1, 2, 3];
let sum = 0;

for (let item of numArr) {
  sum += item;
}

console.log(sum); // outputs: 6 (this is not wrong)
```

Declarative:

```js
const sumArr = [1, 2, 3];
const sum = sumArr.reduce((acc, num) => acc + num, 0);

console.log(sum); // outputs: 6 (same as above, but ✨declarative✨)
```

Declarative is often considered the best pattern, but that may not always be the case.

# In conclusion

`var` is the oldest, and least-safe type of variable. It is only function scoped, and does not throw errors when you try to use it after is lexical declaration. It is best used for legacy applications and backwards compatibility with old browsers.

`let` is the next best alternative, it is block-scoped and will throw a `ReferenceError` if you try to use it before it is lexically defined. It can still be reassigned, but will not work in old browsers as it was new in 2015.

And lastly, `const`. `const` came out at the same time as `let`, but preserves the reference to the value it was assigned in memory preventing the developer from reassigning it a different value. However, despite that, you can still reassign values within non-primitive types like object literals and arrays. `const` also encourages declarative programming.

- `const` should be every developer's default.
- `let` should be used when you cannot use `const`.
- `var` should not be used except for backwards compatibility purposes.
