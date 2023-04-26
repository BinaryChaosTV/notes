# Functional Programming

Functional programming is a style of programming where solutions are simple, isolated functions, without any side effects outside of the function scope: `INPUT -> PROCESS -> OUTPUT`

Functional programming is about:

- Isolated functions - there is no dependence on the state of the program, which includes global variables that are subject to change
- Pure functions - the same input always gives the same output
- Functions with limited side effects - any changes, or mutations, to the state of the program outside the function are carefully controlled

One of the core principles of functional programming is to not change things. Changes lead to bugs. It's easier to prevent bugs knowing that your functions don't change anything, including the function arguments or any global variable.

Recall that in functional programming, changing or altering things is called _mutation_, and the outcome is called a _side effect_. A function, ideally, should be a _pure function_, meaning that it does not cause any side effects.

## Callbacks

Callbacks are the functions that are slipped or passed into another function to decide the invocation of that function. You may have seen them passed to other methods, for example in `filter`, the callback function tells JavaScript the criteria for how to filter an array.

Functions that can be assigned to a variable, passed into another function, or returned from another function just like any other normal value, are called _first class functions_. In JavaScript, **all functions are first class functions**.

The functions that take a function as an argument, or return a function as a return value, are called _higher order functions_.

When functions are passed in to or returned from another function, then those functions which were passed in or returned can be called a _lambda_.
