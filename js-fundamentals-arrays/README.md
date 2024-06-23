# JavaScript fundamentals: Arrays

_Before we begin, I recommend you read this article on GitHub, which has proper code highlighting and extra readability improvements! https://github.com/jack3898/guides/tree/main/js-fundamentals-arrays_

In this guide we will explore the power and flexibility of arrays.

- What are arrays?
- Why are they useful?

# What is an array?

An array is a special datatype that can store an ordered list of values of _any_ type, even mixed.

It is most commonly declared using the square bracket `[]` notation.

```js
const myArray = [1, "hello", { key: "value" }];
```

# Indexing the array

In order to access values stored in an array, you index the array with the same square brackets:

```js
const myArray = [1, "hello", { key: "value" }];

const item2 = myArray[1]; // item2 is now equal to "hello"
```

It is important to know that arrays use zero-based indexes. So item 1 is at position 0, and item 2 is at position 1 and so forth.

# Arrays are objects

Under the hood, an array is just an instance of the `Array` type. This means that declaring arrays is syntax sugar over using it like this:

```js
const myArray = new Array(1, "hello", { key: "value" });
```

# Why are arrays useful?

Arrays are an enumerable and iterable datatype, which means you can pull values out, transform the values, loop through the values and change the order and structure of the array.

You get the same feeling using an array as you would learning of the powers of Microsoft Excel! I know that might sound weird, but hear me out!

# Some examples

To show the power of arrays, here are some examples. Don't let the examples fool you either, arrays can stretch their wings far more than what is shown!

## Sum or total

If you have an array of numbers, you can do some handy arithmetic on the numbers adding all of the numbers to create a sum. This is again because you can naturally loop through the array, and JavaScript will automatically break out of the loop when the array has been exhausted.

```js
const numbers = [1, 2, 3];

let sum = 0;

for (number of numbers) {
  sum += number;
}

console.log(sum); // 6
```

## Average

You can calculate averages

```js
const numbers = [1, 2, 3];

let sum = 0;

for (number of numbers) {
  sum += number;
}

console.log(sum / numbers.length); // 2
```

## Median (value in the middle)

What about getting the Median? We can pull out the value in the middle, and if the array is of an even length, average the two values in the middle.

```js
const numbers = [0, 40, 50, 1000];
const arrayLengthIsEven = numbers.length % 2 === 0;

let median = 0;

// There is no direct "middle" value in the array
// So we average the two values in the middle instead
if (arrayLengthIsEven) {
  const midLeft = numbers[numbers.length / 2 - 1];
  const midRight = numbers[numbers.length / 2];

  median = (midLeft + midRight) / 2;
} else {
  median = numbers[(numbers.length - 1) / 2];
}

console.log(median); // 45
```

# Array built-in methods

The above examples only scratch the surface of _how_ an array can be used.

However, arrays, like most things in JS, are objects that can contain methods. On an array instance, it has loads of utility methods that make working with arrays even easier.

I can't go through each and every array method, but I cherry-picked some for this guide.

You can read more about array methods on [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)!

## `.pop()`

Pop is used to remove the item at the end of an array mutating the original array.

```js
const numbers = [1, 2, 3];

const poppedNumber = numbers.pop();

console.log(numbers); // [1, 2]
console.log(poppedNumber); // 3
```

## `.push()`

`push` does the opposite of `pop`. It adds an item to the end of the array mutating the original array.

```js
const numbers = [1, 2, 3];

const pushedNumber = numbers.push(4);

console.log(numbers); // [1, 2, 3, 4]
console.log(pushedNumber); // 4
```

## `.forEach()`

`.forEach()` does a similar thing to our above use of the `for (number of numbers) {}` loop. But instead of being a `for` loop, we call the `forEach` method on the array, and a function that will be run against each item in the array.

```js
const numbers = [1, 2, 3];

let sum = 0;

numbers.forEach((number) => {
  sum += number;
});

console.log(sum); // 6
```

Some developers find `forEach` more readable, however, it is usually slower than a `for` loop. I generally find that if I want to do a basic loop, I use a `for` loop.

## `.map()`

This is an extremely powerful array method! You will find this one used so much in the wild, it is for sure one of the most important array methods you need to learn.

`.map()` can transform or replace each item in the array with another value of any type.

Let's square each number in the array as an example:

```js
const numbers = [1, 2, 3];

const squaredNumbers = numbers.map((number) => {
  return number * number;
});

console.log(squaredNumbers); // [1, 4, 9]
```

## `.reduce()`

Reduce runs logic against each value in the array, with the goal of producing an output. This output can be anything, even complex objects.

`.reduce()` is extremely versatile, but a bit more verbose than other array methods because of its flexibility.

This is because there are three components to `reduce`:

- An "accumulator" (I like to call it the "working value") which is mutated when you run code against each item in the array
- An initial value that the accumulator is set to (remember, the output of `reduce` can be literally anything!)
- A current value, which is the value of the entry in the array which you will use to influence what you do with the accumulator

The return value of the function you passed in to `reduce()` determines the next value of our `accumulator`.

Let's optimize our sum logic to use reduce instead.

```js
const numbers = [1, 2, 3];
const initialValue = 0;

const sum = numbers.reduce((accumulator, current) => {
  return accumulator + current;
}, initialValue);

console.log(sum); // 6
```

# Array spread operator

In JavaScript, there is the spread operator `...`.

This allows you to spread another array into a given array.

```js
const array1 = [1, 2, 3];
const array2 = [4, 5, 6];

const mergedArray = [...array1, ...array2, 7];

console.log(mergedArray); // [1, 2, 3, 4, 5, 6, 7]
```

To me this is really readable, and visually shows how the array is structured.

# Function rest operator

In JavaScript, functions can be made to accept a variable amount of parameters using the rest `...` operator. It's lexically the same as the spread operator, but does the inverse, instead of "unpacking" the array to be placed into another array, it "packs" the values _into_ an array.

Inside of the function body, the type the rest operator generates is an array containing value of each argument it captures. This means we unlock all those handy array methods!

The rest operator must always come last, as the quantity of arguments is unknown.

```js
function sum(...numbers) {
  return numbers.reduce((acc, cur) => {
    return acc + cur;
  });
}

const total = sum(1, 2, 3);

console.log(total); // 6
```

# That's a wrap for now!

To recap:

- Arrays are enumerable and iterable, so can be looped through.
- Arrays are really powerful as a means to operate on a list of values.
- You can use built-in array methods to further operate on the values of the arrays.
- The spread operator is used to merge the contents of one array into another.
- Arrays are helpful with functions that have a variable length of parameters as they are the result of the rest parameter.
