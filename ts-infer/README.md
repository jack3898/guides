# TypeScript's "infer" keyword

This is going to be a short article that explains how the "infer" keyword is used in TypeScript.

- What is it?
- What is it used for?
- How would you use it?

# What is it?

The `infer` keyword is a powerful keyword that can extract types buried within some other complex type.

Questions like:

- "How can I extract the type of an item in an array?"
- "How can I get the return value of a function?"
- "What it the type of the array, in an object with an `id` property?"

Infer is really powerful, and builds off of the [conditional type](https://www.typescriptlang.org/docs/handbook/2/conditional-types.html) system in TypeScript which determines what type something is with the assumption a previous condition has been met. This is crucial for infer, as in order to extract the type of something within a function signature, object or array we need to make TypeScript confident that we're dealing with the correct shape of type in the first place.

For example "_IF_ this type is a function, infer its return value" or "_IF_ this type is an array, infer the type of item x in the array".

# How to use it

First let's declare an array in TypeScript with some properties.

```ts
const array = [3, "hello", { key: "value" }];
```

Let's ask a question:

> How would I know what type the second property of an array is?

There are some key things you know already:

1. We're working with an array
2. It needs at least 2 entries
3. It's the second entry we wish to infer the type of
4. Type of `"hello"` is a string, so it's going to be a string

So we first create a conditional type which asserts that before we can infer entries in an array, we first need to confirm it is an array:

```ts
// Our array to query from
const array = [3, "hello", { key: "value" }];

// Our utility "conditional" type
type ArraySecondIndexType<T> = T extends unknown[] ? unknown : never;

// Using the utility type
type SecondIndexType = ArraySecondIndexType<typeof array>; // Equals `unknown`
```

Inspecting the resolved type for `SecondIndexType` will reveal that indeed the type is `unknown`. This confirms that the constraint `T extends unknown[]` has been met because it is an array with potentially any amount of unknown values.

Now on to second part, we need to add an extra level to our conditional type, that being that the array must have at least two items in it. We can't infer the second item of an array if we first haven't given TypeScript confidence that the array is at least 2 items long!

Let's amend our conditional type to now assert that in order to be `unknown` we need use an array with at least two items. Using the spread `...` operator in addition to a tuple type, tells TS that the array is a minimum of 2 items long.

```ts
// Our array to query from
const array = [3, "hello", { key: "value" }];

// Our utility type
type ArraySecondIndexType<T> = T extends [unknown, unknown, ...unknown[]]
  ? unknown
  : never;

// Using the utility type
type SecondIndexType = ArraySecondIndexType<typeof array>; // Equals `never`??
```

Now if you inspect the type of `SecondIndexType`, the condition has no longer been met and equals `never`? Why is that?

Our array has three items in it, so why does TypeScript think that the `[unknown, unknown, ...unknown[]]` constraint does not work with our array?

This is because TypeScript is inferring the array too loosely. TypeScript infers the type of our constant to be "any length array that is a string OR number OR object with at least one property "key" of type "string"". Which in most cases is fine, but in our case, it doesn't really tell TypeScript the array is definitely at least 2 items long.

So it is this:

```ts
type Arr = (string | number | { key: string })[];
```

When really, it should be this tuple type:

```ts
type Arr = [number, string, { key: string }];
```

One quick way around this is simply asserting that the array is that type by typing the constant as so which will solve our constraint issues.

```ts
type Arr = [number, string, { key: string }];

// Our array to query from
const array: Arr = [3, "hello", { key: "value" }];

// Our utility type
type ArraySecondIndexType<T> = T extends [unknown, unknown, ...unknown[]]
  ? unknown
  : never;

// Using the utility type
type SecondIndexType = ArraySecondIndexType<typeof array>; // Equals `unknown` again!
```

Great! TypeScript is happy, and our use of the utility type has given us `unknown` again.

So that begs the question, now TypeScript knows that the item is an array, with at least two entries, how can we turn our `unknown` type into the _real_ type which is a `string`?

Drumroll please, you will never guess this...

Use `infer`!

```ts
type Arr = [number, string, { key: string }];

// Our array to query from
const array: Arr = [3, "hello", { key: "value" }];

// Our utility type
type ArraySecondIndexType<T> = T extends [unknown, infer V, ...unknown[]]
  ? V
  : never;

// Using the utility type
type SecondIndexType = ArraySecondIndexType<typeof array>; // Equals `string`!
```

In the place of item 2, simply add `infer V` which acts as a placeholder for whatever entry 2 is.

So when we use the utility type, `V` becomes a value, and as the array is at least two items long, the value of `V` is the result of the conditional type. A string!

`V` is actually a name I chose. It does not have to be `V`. It could be renamed to something more descriptive. But for now let's assume `V` means "value".

# A challenge for you

Using what you have just learned with arrays, how might you go about doing the same thing with functions and objects?

- How can you infer the values (and keys!) of an object literal type?
- How can you infer the parameters and return value of a function?

_And for bonus points_

- How can you infer the type of a property of an object type returned from a function?

# And that's infer!

To recap:

- Infer extracts a subtype within a complex type. For example, an object, array or function signature.
- Infer only works with conditional types

_This article and all of its imagery is open source! Spot a mistake, or just want to improve it further? Feel free to open a PR at https://github.com/jack3898/guides/tree/main/ts-infer_
