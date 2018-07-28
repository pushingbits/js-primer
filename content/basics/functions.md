# Functions and Declarative programming

First a little bit of preamble to give a quick overview on syntax and styles.

## Syntax
Functions are, like Objects, _everywhere_ in JavaScript.
As of ES6 there are two different ways to declare a function,
each with subtly different behaviour.

```js
// can be declared with or without a name
function normalFunction() { }

// anonymous function
const normalFunction = function () {};

// anonymous function in line.
array.map(function (item) { return item + 1 });

// for all of these types of functions, the value of 'this'
// is bound to the enclosing object.
```

However... (as of ES6)

```js
// we now have arrow functions.
// The arrow is shorthand for return.
// it's good practice to include parenthesis around args,
// but for single arguments, you don't need to.
const returns10 = () => 10;
const greet = name => `Hello ${name}`;

// for the example above we get the much nicer
array.map(item => item + 1);

// But arrow functions also have one special property!
// they bind not to the enclosing object, but the surrounding context.
// This is important in areas where you may normally have had to 'bind' a fn.
element.addEventListener('click', handler.fn.bind(handler));
// vs. (FIXME This example could probs be better)
element.addEventListener('click', () => handler.fn()); // yay!
```

In some cases you may need to explicitly alter the value of `this`
that you're trying to manipulate. In such cases, one form of the below methods
will be appropriate.

### Bind `.bind(thisArg, ...args)`
Bind explicitly nominates a function's `this` value and
returns this new function. Common use with event handlers as above.

```js
// now objects methods will be bound to object (as it should be)
// rather than element (which is the enclosing object)
element.addEventListener('click', object.method.bind(object));
```

### Call `.call(thisArg, ...args)`
Call is very similar to `.bind`, except that rather than returning a function,
it also makes the function call.

```js
const returnVal = object.method.call(object, arg1, arg2...);
```

### Apply `.apply()`
Apply does exactly the same thing as `.call`, except that it takes an Array,
rather than an arguments list. IE.

```js
const returnVal = object.method.apply(object, [arg1, arg2]);
```

## Thinking Functionally

Functions in JavaScript are first class objects; that is they
can be more or less treated in the same way as variables, passed around,
and even returned by other functions. This isn't a unique trait of JavaScript
but it does allow you to think and program differently than you might
otherwise be used to.

### Imperative vs Declarative programming
If you've come from a language like `C` you're likely to think
in an imperative way when you think about your code patterns.

_Imperative_ programming is like a recipe. Do this, then this, then that.
It's a set of commands that the process will follow.

_Declarative_ programming on the other hand is a way of
describing a program's logic rather than the direct process.

```js
const double = num => num * 2

// imperative
for (const value of list) {
   doubledList.push(value * 2)
}

// declarative
const doubledList = list.map(double)
```

Declarative programming relies on what are known as **pure functions**.
Pure functions are functions with no side effects, they simple take in
an input, and spit out an output without affecting any sense of global state.

```js
// pure functions are very easy to reason about and test
const double = num => num * 2
const addCurlyBraces = string => `{${string}}`

// impure because list is affected
const list = []
const impure = num => list.push(num)
```

What's more, because these functions are stateless, we can very easily combine
them with composition.