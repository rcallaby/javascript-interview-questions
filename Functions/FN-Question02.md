# Question 02 - Functions

## What is a pure function? Please demonstrate this type of function with source code

A **pure function** in JavaScript is a function that adheres to the following principles:

1. **Deterministic**: Given the same inputs, it always produces the same output.
2. **No Side Effects**: It does not modify any external state, variables, or data outside its scope. Instead, it relies solely on its input parameters and returns a new value.

### Characteristics of Pure Functions:
- They don't depend on or modify any external state.
- They don't perform operations like database writes, logging, or making API calls.

### Example of a Pure Function

Here’s a simple example of a pure function that calculates the square of a number:

```javascript
// Pure function: Always produces the same result for the same input.
function square(num) {
  return num * num;
}

console.log(square(4)); // Output: 16
console.log(square(4)); // Output: 16 (always the same output for the same input)
```

### Example of an Impure Function

To contrast, here’s an example of an **impure function**:

```javascript
let multiplier = 2;

function impureMultiply(num) {
  return num * multiplier; // Depends on an external variable `multiplier`
}

console.log(impureMultiply(4)); // Output: 8

// If `multiplier` changes, the output changes, making it impure
multiplier = 3;
console.log(impureMultiply(4)); // Output: 12
```

This function is impure because:
1. It relies on the external variable `multiplier`.
2. The result is not solely determined by the function's input (`num`).

### Example of a Pure Function That Does Not Mutate State

Consider an array transformation example:

```javascript
// Pure function: It does not modify the original array but returns a new one.
function addElementToArray(arr, element) {
  return [...arr, element];
}

const originalArray = [1, 2, 3];
const newArray = addElementToArray(originalArray, 4);

console.log(originalArray); // Output: [1, 2, 3] (original array remains unchanged)
console.log(newArray);      // Output: [1, 2, 3, 4] (new array with the added element)
```

In this case, the function `addElementToArray`:
1. Does not modify the `originalArray`.
2. Returns a new array with the added element, making it a pure function.

### Why Use Pure Functions?

1. **Testability**: Pure functions are easier to test because they don’t depend on external states or cause side effects.
2. **Predictability**: Their behavior is predictable due to determinism.
3. **Immutability**: They promote immutability, which is a key concept in functional programming.

### Practical Use in Functional Programming

Pure functions are the building blocks of functional programming. For instance, `map`, `filter`, and `reduce` in JavaScript work well with pure functions:

```javascript
const numbers = [1, 2, 3, 4];

// Pure function to double a number
function double(num) {
  return num * 2;
}

// Using the pure function with map
const doubledNumbers = numbers.map(double);

console.log(numbers);       // Output: [1, 2, 3, 4] (original array unchanged)
console.log(doubledNumbers); // Output: [2, 4, 6, 8]
```

In this case, the pure function `double` is applied to each element in the array using `map`, ensuring the original data is not mutated.