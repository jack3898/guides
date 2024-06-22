# JavaScript fundamentals: Functions

A function in JavaScript lets you assign a block of code to a variable, which you can run more than once and pass around.

This guide will explain what functions are, and the many ways they can be used in any JavaScript application and reach into advanced usage of functions after the basics.

This guide will not go into the `this` keyword or arrow functions, generators or even some other concepts. They will be written separately!

# Creating a function

A function can be defined like so:

```js
function myFunction() {
  console.log("Hello!");
}
```

Above, I created a function called `myFunction`, and has the sole responsibility of logging "Hello!" to the console.

# Invoking the function

To invoke a function, you use a pair of parentheses. This calls the code inside of the function.

```js
myFunction(); // outputs "Hello!" to the console.
```

Thanks to it being a function, the code is reusable!

```js
// outputs "Hello!" to the console every time
myFunction();
myFunction();
myFunction();
```

# Aliasing the function

If you do not provide a pair of parentheses `()`, then you are not invoking the function.

```js
myFunction; // Does nothing
```

This is because without the parentheses, you are simply referring to the definition of the function. As it is the definition, this means that it is a tangible datatype and can assign it to variables.

```js
const renamedFunction = myFunction;

renamedFunction(); // outputs: "Hello!" to the console
```

# Passing in values

You may have noticed the empty parenthesis `()` directly after the function name.

```js
function myFunction() {}
```

These are used to provide placeholder variable names to the function code.

The placeholder variable name is called a "parameter", and the value of the parameter is an "argument".

```js
function myFunction(param) {
  console.log(param);
}
```

Like above, you can invoke the function with an argument of your choosing.

```js
myFunction("Hello!"); // outputs argument "Hello!" to the console
```

If you forget to pass something in, then the placeholder will by default be `undefined`. So be sure to practice defensive programming to catch any potential runtime errors. Or you may define a default value of your own!

```js
function myFunction(param = "Default!") {
  console.log(param);
}

myFunction(); // outputs "Default!" to the console
myFunction("Hello!"); // outputs "Hello!" to the console
```

And lastly, you can define as many parameters as you like as well by separating the parameter names with commas.

```js
function myFunction(param1, param2, param3) {
  // ...
}
```

# Returning values

When a function has finished with its execution, it can return a value to the caller. Returning immediately stops any further execution of the function and is final.

For example, take this common example which adds two numbers together:

```js
function sum(num1, num2) {
  return num1 + num2;
}
```

Because it _returns_ a value to the caller we can store the result in a variable.

```js
const firstSum = sum(1, 1);
const secondSum = sum(2, 3);

console.log(firstSum); // outputs 2
console.log(secondSum); // outputs 5
```

# Anonymous functions

A function may be anonymous, which is, it isn't given a name.

```js
function () {
  console.log("Hello!");
};
```

The above written as-is will give you an error, because anonymous functions are only valid in an _expression_ context. This is because it has no name, and would be unreachable otherwise!

Here's an example of a valid use of an anonymous function:

```js
const myFunction = function () {
  console.log("Hello!");
};
```

It is an anonymous function assigned to a named variable so we can reach it. There are other more complex ways to use anonymous functions, but for the sake of simplicity, I used the above. I will show the more complex examples further on.

# Pure functions

A pure function is a function whos output is predicable by its input.

Here's an example of an _impure_ function:

```js
let number = 0;

function increment() {
  number += 1;

  return number;
}
```

It is impure because other external factors beyond the control of the function can influence the value it returns. `increment` can read values of variables defined outside of itself and whenever `increment` is invoked, the `number` variable will be permanently mutated. It is entirely possible that `number` is accessed by other code potentially introducing bugs.

Here is an example of a pure function:

```js
function increment(number) {
  number += 1;

  return number;
}
```

Pure functions are almost always the preferred way to define a function, as they are easily testable and predictable and are easier to reason with.

# IIFE (pronounced iffy)

An IIFE (also known as an Immediately Invoked Function Expression) is an anonymous function, made into an expression, then immediately invoked.

```js
(function () {
  console.log("Hello!");
})();
```

If we did not first wrap the function in parentheses the JavaScript intepreter would just see your code, which will not work:

```js
function () {
console.log("Hello!");
};

()
```

# Higher-order functions

A higher order function is a function that can either:

- Accept a function as a argument
- Return a function from itself

# Passing a function to another function

Earlier in the article, I showed you how to alias a function by assigning it to another variable.

Then, later on, I explained that a parameter to a function is a placeholder variable name exposed to function code.

It is possible to combine the above two concepts. You can pass a reference to a function as a value to a parameter. The parameter is then the alias for the function, which you can execute in the consuming function.

```js
function callbackFn() {
  console.log("Hello!");
}

function myFunction(callback) {
  callback();
}

myFunction(callbackFn); // outputs "Hello!" to the console
```

This may be slightly confusing to wrap your head around if you're learning for the first time, but it really powerful for reusable and composable code.

# Returning a function from a function

Even more confusingly, you can also return a function from a function! The return value is a function definition.

```js
function myFunction() {
  return function () {
    console.log("Hello!");
  };
}
```

Note, here's another good use of an anonymous function.

You can then run the function:

```js
const returnedFn = myFunction();

returnedFn(); // outputs "Hello!" to the console
```

# Currying

Currying is executing a chain of returned functions. Above, we gave the value of the returned function a name. But you could have just immediately invoked the returned function.

```js
myFunction()(); // outputs "Hello!" to the console
```

# Closures

The returned function can also read the variable contents of the function it was defined in. This is the known as the closure of the function as you cannot access variables inside of a function without using a closure or being in the execution context of that function.

```js
function myFunction() {
  const message = "Hello!";

  return function () {
    console.log(message);
  };
}

myFunction()(); // outputs "Hello!" to the console
```

A good example of using a closure is to create a custom counter function.

```js
function counter() {
  let counter = 0;

  return function (amount = 1) {
    counter += amount;

    return counter;
  };
}

const count = counter();

const firstNumber = count(); // value is 1 (default amount)
const secondNumber = count(10); // secondNumber is 11 (1 + 10)
const thirdNumber = count(5); // thirdNumber is 16 (1 + 10 + 5)
```

# In conclusion

- A function is a reusable block of code at its most basic level, and an advanced state machine at its most complex.
- Functions are defined with the function keyword, followed by a name. If they have no name, it is an anonymous function.
- An IIFE is an anonymous function that is wrapped in parentheses so it can be immediately executed.
- Functions can be invoked/executed with parentheses, or passed around by not invoking them.
- Functions can include parameters, allowing developers to pass arguments in to influence the function execution.
- Functions can return values after they have been invoked, which you can then store into a variable.
- Functions can accept "callback" functions as an argument, allowing developers to run a different functions inside of a function.
- Pure functions are predicable by its inputs. Impure functions have their execution affected by values outside of the function body.
- Functions can also return a function which can then be invoked after main function call.
- A closure is simply the values that exist in the scope of the function body in which a function was returned.
- Currying is running returned functions immediately after the main function call.

And now you know how functions work!
