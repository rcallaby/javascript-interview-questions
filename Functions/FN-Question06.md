# Question 06 - Functions

## What are arrow functions, and how do they differ from regular functions in JavaScript?

### What Are Arrow Functions in JavaScript?

Arrow functions are a more concise syntax for writing functions, introduced in ES6 (ECMAScript 2015). They allow you to define functions using a "fat arrow" (`=>`), which often results in cleaner and shorter code.

#### Syntax of Arrow Functions:
```javascript
// Regular function
function add(a, b) {
  return a + b;
}

// Arrow function
const add = (a, b) => a + b;
```

#### Key Differences Between Arrow Functions and Regular Functions:

1. **`this` Binding:**
   - Arrow functions do **not** have their own `this`. Instead, they inherit `this` from the surrounding lexical scope (where they are defined).
   - Regular functions have their own `this` context, determined by how the function is called (`call`, `apply`, or the object invoking it).

   **Example:**
   ```javascript
   function RegularFunction() {
       this.value = 10;

       setTimeout(function() {
           console.log(this.value); // `this` is undefined or the global object
       }, 100);
   }

   function ArrowFunction() {
       this.value = 10;

       setTimeout(() => {
           console.log(this.value); // `this` is inherited from the enclosing scope
       }, 100);
   }

   new RegularFunction(); // undefined or error in strict mode
   new ArrowFunction();   // 10
   ```

2. **`arguments` Object:**
   - Regular functions have access to the `arguments` object, which is an array-like object containing all arguments passed to the function.
   - Arrow functions do **not** have their own `arguments`. If you try to access `arguments` inside an arrow function, it will reference the `arguments` of the outer function or be undefined if no outer function exists.

   **Example:**
   ```javascript
   function regularFunc() {
       console.log(arguments);
   }

   const arrowFunc = () => {
       console.log(arguments); // ReferenceError: arguments is not defined
   };

   regularFunc(1, 2, 3); // [1, 2, 3]
   arrowFunc(1, 2, 3);   // Error
   ```

3. **Syntax:**
   - Arrow functions are concise and can omit the `function` keyword, curly braces, and the `return` keyword for single-expression returns.
   - Regular functions require the `function` keyword and, typically, curly braces and the `return` keyword for returning values.

   **Example:**
   ```javascript
   // Regular function
   const squareRegular = function(x) {
       return x * x;
   };

   // Arrow function (concise version)
   const squareArrow = x => x * x;
   ```

4. **Usage as Methods:**
   - Arrow functions are **not suitable** as object methods because they lack their own `this`.
   - Regular functions are ideal for methods, as their `this` is dynamically determined based on how the method is called.

   **Example:**
   ```javascript
   const obj = {
       value: 42,
       regularMethod: function() {
           console.log(this.value); // Works as expected
       },
       arrowMethod: () => {
           console.log(this.value); // `this` is undefined or global (lexical scope)
       }
   };

   obj.regularMethod(); // 42
   obj.arrowMethod();   // undefined
   ```

5. **Cannot Be Used as Constructors:**
   - Arrow functions cannot be used with the `new` keyword because they lack a `[[Construct]]` method (required for creating instances).
   - Regular functions can be used as constructors.

   **Example:**
   ```javascript
   const ArrowFunc = () => {};
   function RegularFunc() {}

   new RegularFunc(); // Works
   new ArrowFunc();   // TypeError: ArrowFunc is not a constructor
   ```

6. **No `prototype` Property:**
   - Arrow functions do not have a `prototype` property.
   - Regular functions do, which allows them to define methods or properties that instances can inherit.

   **Example:**
   ```javascript
   function RegularFunc() {}
   const ArrowFunc = () => {};

   console.log(RegularFunc.prototype); // {}
   console.log(ArrowFunc.prototype);  // undefined
   ```

### When to Use Arrow Functions:
- When you need a concise syntax for simple, one-line functions.
- When you want `this` to be inherited from the surrounding context.
- For callbacks, especially in array methods like `map`, `filter`, and `reduce`.

### When to Use Regular Functions:
- When you need dynamic `this` binding (e.g., object methods or event handlers).
- When you need access to the `arguments` object.
- When you are defining a constructor function. 

