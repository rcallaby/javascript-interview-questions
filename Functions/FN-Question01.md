# Question 01 - Functions

## What is a first class function and how does it differ from a first order function?

### First-Class Functions in JavaScript

In JavaScript, **first-class functions** mean that functions are treated like any other value in the language. Specifically:

- Functions can be assigned to variables.
- Functions can be passed as arguments to other functions.
- Functions can be returned from other functions.
- Functions can be stored in data structures like arrays or objects.

This concept allows JavaScript to support **functional programming** patterns.

#### Example of First-Class Functions
```javascript
// 1. Assigning a function to a variable
const greet = function(name) {
  return `Hello, ${name}!`;
};

// 2. Passing a function as an argument
function processUserInput(callback) {
  const name = "Alice";
  return callback(name);
}

// 3. Returning a function from another function
function multiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

// Using the examples:
console.log(greet("John")); // "Hello, John!"

console.log(processUserInput(greet)); // "Hello, Alice!"

const triple = multiplier(3);
console.log(triple(5)); // 15
```

In the example above:
1. The `greet` function is assigned to a variable (treated as a value).
2. The `greet` function is passed as an argument to `processUserInput`.
3. The `multiplier` function returns a new function that multiplies numbers by a factor.

---

### First-Order Functions vs First-Class Functions

- **First-Class Functions** refer to the *capability* of functions in a language (i.e., they can be treated as values).
- **First-Order Functions** refer to a specific type of function that does not take other functions as arguments or return other functions. 

In other words:
- All functions in JavaScript are first-class.
- First-order functions are functions that only operate on data, not on other functions.

#### Example of a First-Order Function
```javascript
function add(a, b) {
  return a + b;
}

console.log(add(2, 3)); // 5
```

In this example:
- `add` is a first-order function because it does not take other functions as arguments or return a function.
- It is still a first-class function because you could assign it to a variable or pass it to another function.

#### Example of a Higher-Order Function
```javascript
function calculate(operation, x, y) {
  return operation(x, y);
}

function multiply(a, b) {
  return a * b;
}

console.log(calculate(multiply, 4, 5)); // 20
```

Here:
- `calculate` is a higher-order function because it takes a function (`operation`) as an argument.
- `multiply` is a first-order function that operates on numbers, but since it's in JavaScript, it is still first-class.

---

### Summary
1. **First-Class Functions**: A language feature indicating that functions are treated as values.
   - This is a property of the language.
   - All JavaScript functions are first-class.
2. **First-Order Functions**: Functions that neither take other functions as arguments nor return functions.
   - This is a property of an individual function.
   - Not all JavaScript functions are first-order.