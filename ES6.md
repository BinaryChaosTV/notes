# Javascript ECMAScript 6

ECMAScript, or ES, is a standardized version of JavaScript. Because all major browsers follow this specification, the terms ECMAScript and JavaScript are interchangeable.

ES6, released in 2015, added many powerful new features to the language, including arrow functions, destructuring, classes, promises, and modules.

## Functions

### Arrow Functions

In JS, we often pass a function as an argument to another. We create inline functions because we don't want to reuse them elsewhere. Normally, we'd use the following syntax:

```
const myFunc = function() {
  const myVar = "value";
  return myVar;
}
```

In ES6, we can simplify this with arrow functions. Notice the curly brackets when we have multiple commands to pass.

```
const myFunc = () => {
  const myVar = "value";
  return myVar;
}
```

However, if we want to simply pass a one-liner with only a return value, we can omit the curly brackets altogether: `const myFunc = () => "value"`

#### Parameters

You can pass parameters with arrow functions by placing it in the parentheses: `const doubler = (item) => item * 2;`
We can also pass multiple parameters: `const doubler = (item, multi) => item * multi;`
If there is **only one** parameter, we can omit the `()`: `const doubler = item => item * 2;`
Lastly, we can also set **default values** to our parameters:

```
const greeting = (name = "Anonymous") => "Hello " + name;`
console.log(greeting("John")); // Returns Hello John
console.log(greeting()); // Returns Hello Anonymous
```

## Objects

### Object Freezing

`const` doesn't stop arrays and objects from being mutable. To ensure an object doesn't mutate, we can use `Object.freeze()`.

```
let obj = {
  name:"FreeCodeCamp",
  review:"Awesome"
};
Object.freeze(obj);
obj.review = "bad"; // Will not change review.
obj.newProp = "Test"; // Will not add newProp.
```

### Pass Object into function

You can also destructure the object in a function argument itself. Consider the following:

```
const profileUpdate = (profileData) => {
  const { name, age, nationality, location } = profileData;
}
```

The following can also be done in place, as we're effectively passing the object and passes it into the function:

```
const profileUpdate = ({ name, age, nationality, location }) => {

}
```

### Pass parameters into objects

Inversely, we can easily pass parameters as an object. Consider the following code:

```
const getMousePosition = (x, y) => ({
  x: x,
  y: y
});
```

Instead, we can do it in a single line: `const getMousePosition = (x, y) => ({ x, y });`

### Declaring functions in Objects

We can pass a function inside an object as follows:

```
const person = {
  name: "Taylor",
  sayHello() {
    return `Hello! My name is ${this.name}.`;
  }
};
```

The `this` is refers to the parent object ("Taylor").

## Rest Parameter

ES6 allows us to create functions that take a **variable number of arguments**. These arguments are stored in an array to be accessed later. We use the ellipses `...` followed by the array to pass. (such as `...args`):

```
function howMany(...args) {
  return "You have passed " + args.length + " arguments.";
}
console.log(howMany(0, 1, 2)); // Returns "You have passed 3 arguments"
console.log(howMany("string", null, [1, 2, 3], { })); // Returns "You have passed 4 arguments"
```

`...arr` returns an unpacked (spreads) array. **It can only be used as an argument or as an array literal.** The following will not work: `const spreaded = ...arr;`

However, this will: `const arr2 = [...arr1]; // Copies arr1 into arr2`

## Destructuring Assignment

Now in ES6 you can extract values straight from an object. Take the following example:

```
const user = {
  johnDoe: {
    age: 34,
    email: 'johnDoe@freeCodeCamp.com'
  }
};
```

You can extract the age and email using: `const { JohnDoe: { age, email }} = user;`

You can also apply values to variables with different names: `const { johnDoe: { age: userAge, email: userEmail }} = user;`

### Destructuring arrays

Similarly, we can deconstruct arrays: `const [a,b] = [1,2,3,4,5,6]; // Returns 1 and 2`
Or we can refer to a later value: `const [a, b,,, c] = [1, 2, 3, 4, 5, 6]; // Returns 1, 2 and 5`

Finally, we can use the [Rest parameter](#rest-parameter) to destructure an array, very similarly to `Array.prototype.slice()`: `const [a, b, ...arr] = [1, 2, 3, 4, 5, 7]; // Returns 1, 2 and [3,4,5,7]`

**Note:** The Rest element only works correctly as the last variable in the list.

## Template Literals

Historically, to pass variables in a string, we had to close our string and wrap our variable with `+ variable +` such has `"Hello, my name is "+ name +"."`.

ES6 introduces the concept of literals, which makes the whole ordeal much easier. Instead of opening strings with single or double quotes, open them with a "\`". Then, you can pass a variable by inserting it with `${ variable }`:

```
const person = {
  name: "Zodiac Hasbro",
  age: 56
};

const greeting = `Hello, my name is ${person.name}!
I am ${person.age} years old.`;

// Returns Hello, my name is Zodiac Hasbro! I am 56 years old.
```

## Classes

In ES6, a `class` declaration has a `constructor` method that is invoked with the `new` keyword. If the `constructor` method is not explicitly defined, then it is implicitly defined with no arguments.

```
// Explicit constructor
class SpaceShuttle {
  constructor(targetPlanet) {
    this.targetPlanet = targetPlanet;
  }
  takeOff() {
    console.log("To " + this.targetPlanet + "!");
  }
}

// Implicit constructor
class Rocket {
  launch() {
    console.log("To the moon!");
  }
}

const zeus = new SpaceShuttle('Jupiter');
// prints To Jupiter! in console
zeus.takeOff();

const atlas = new Rocket();
// prints To the moon! in console
atlas.launch();
```

It should be noted that the `class` keyword declares a new function, to which a constructor is added. This constructor is invoked when new is called to create a `new` object.

**Note:** UpperCamelCase should be used by convention for ES6 class names, as in `SpaceShuttle` used above.

The `constructor` method is a special method for creating and initializing an object created with a class.

### Getters and Setters

You can obtain values from an object and set the value of a property within an object. These are classically called **getters** and **setters**.

Getter functions are meant to simply return (get) the value of an object's private variable to the user without the user directly accessing the private variable.

Setter functions are meant to modify (set) the value of an object's private variable based on the value passed into the setter function. This change could involve calculations, or even overwriting the previous value completely.

```
class Book {
  constructor(author) {
    this._author = author;
  }
  // getter
  get writer() {
    return this._author;
  }
  // setter
  set writer(updatedAuthor) {
    this._author = updatedAuthor;
  }
}
const novel = new Book('anonymous');
console.log(novel.writer); // Returns anonymous
novel.writer = 'newAuthor';
console.log(novel.writer); // Returns newAuthor
```

Notice the syntax used to invoke the getter and setter. They do not even look like functions. Getters and setters are important because they hide internal implementation details.

**Note:** It is convention to precede the name of a private variable with an underscore (\_). However, the practice itself does not make a variable private.

## Modules

Modules allow for us to export and import scripts from different JS files into our HTML. To do this, we add `type="module"` to our `<script>` instead of `type="text/javascript"`. For example: `<script type="module" src="filename.js"></script>`

### Exporting Functions

Imagine we have a file called `math_functions.js` with **multiple** functions inside. In order to share it with other files, we need to `export` it first, which can be accomplished two ways:

```
// Method 1:
export const add = (x, y) => {
  return x + y;
}

// Method 2:
const add = (x, y) => {
  return x + y;
}

export { add };
```

If we use Method 2, we can export multiple functions at once as follows: `export { add, subtract, multiply };`

#### Default Exports

`export default` creates a fallback value, or default value when exporting a module. If this file is imported and we don't specify which functions to import, the `export default` function will be used.

```
// Named default export
export default function add(x, y) {
  return x + y;
}

// Anonymous default export
export default function(x, y) {
  return x + y;
}
```

**Note:** `export default` cannot be used with `var`, `let` or `const`. Additionally, you can only `export default` a single function in a file.

### Importing Functions

`import` allows us to choose which parts of a file or module to load. Here is how we can import from another file: `import { add } from './math_functions.js';`

In this example, it will **only** add the `add()` function and ignore the rest from the `math_functions.js` file. Keep in mind that we should reference the file using **relative paths**.

We can import multiple functions at once as well: `import { add, subtract, multiply } from './math_functions.js';`

In the case where we want to import ALL contents from a module, we use the asterisk `*`: `import * as myMathModule from "./math_functions.js";`
This kind of import creates an object called `myMathModule` (which can be renamed as we desire). We could then call the newly imported functions:

```
myMathModule.add(2,3);
myMathModule.subtract(5,3);
```

#### Default Imports

When importing a `Default` function ([See Default Exports](#default-exports)), the `import` syntax is slightly different: `import add from "./math_functions.js";`

Notice that the function does not have curly braces around it. The `add` is the equivalent of `myMathModule` and can be renamed however we want.

## Promises

A promise in JavaScript is exactly what it sounds like - you use it to make a promise to do something, usually asynchronously. When the task completes, the promise is either fulfilled or failed. `Promise` is a constructor function, so we need to use the new keyword to create one. It takes a function, as its argument, with two parameters - `resolve` and `reject`. These are methods used to determine the outcome of the promise. The syntax looks like this:

```
const myPromise = new Promise((resolve, reject) => {

});
```

A promise has three states: `pending`, `fulfilled`, and `rejected`. The promise above is forever stuck in the `pending` state because we did not add a way to complete the promise. The `resolve` and `reject` parameters given to the promise argument are used to do this. `resolve` is used when you want your promise to succeed, and `reject` is used when you want it to fail. These are methods that take an argument, as seen below.

```
const myPromise = new Promise((resolve, reject) => {
  if(condition here) {
    resolve("Promise was fulfilled");
  } else {
    reject("Promise was rejected");
  }
});
```

The example above uses strings for the argument of these functions, but it can really be anything (typically objects to be used).

### Then Method (Resolve)

Promises are most useful when you have a process that takes an unknown amount of time in your code (i.e. something asynchronous), often a server request. When you make a server request it takes some amount of time, and after it completes you usually want to do something with the response from the server. This can be achieved by using the `then` method. The `then` method is executed immediately after your promise is fulfilled with `resolve`. Here’s an example:

```
myPromise.then(result => {

});
```

`result` comes from the argument given to the `resolve` method.

### Catch Method (Reject)

`catch` is the method used when your promise has been rejected. It is executed immediately after a promise's `reject` method is called. Here’s the syntax:

```
myPromise.catch(error => {

});
```

`error` is the argument passed in to the `reject` method.
