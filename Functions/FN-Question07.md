# Question 07 - Functions

## What is the difference between function declarations and function expressions in JavaScript?

The difference between **function declarations** and **function expressions** in JavaScript lies in their syntax, behavior, and how they are hoisted by the JavaScript engine. Here's a detailed and accurate explanation:

---

### 1. **Function Declaration**
A **function declaration** defines a named function using the `function` keyword and has the following syntax:

```javascript
function functionName(parameters) {
  // function body
}
```

#### Characteristics:
1. **Hoisting**:
   - Function declarations are **hoisted**. This means the JavaScript engine moves their definitions to the top of their enclosing scope during the compilation phase.
   - As a result, you can call a function declared this way even before its actual definition in the code.

   Example:
   ```javascript
   greet(); // Output: Hello, world!

   function greet() {
     console.log("Hello, world!");
   }
   ```

2. **Naming**:
   - Function declarations require a name. This name can be used to refer to the function inside the scope.

3. **Scope**:
   - They are scoped to the block, function, or global scope in which they are defined.

---

### 2. **Function Expression**
A **function expression** defines a function as part of an expression. It can be either named or anonymous and is assigned to a variable, constant, or passed as an argument.

#### Syntax:
```javascript
// Anonymous function expression
const myFunction = function(parameters) {
  // function body
};

// Named function expression
const myFunction = function functionName(parameters) {
  // function body
};
```

#### Characteristics:
1. **Hoisting**:
   - Function expressions are **not hoisted** the way function declarations are.
   - The variable (e.g., `myFunction`) is hoisted, but it remains uninitialized until the assignment is reached during runtime. This means you cannot call the function before the assignment.

   Example:
   ```javascript
   greet(); // Error: Cannot access 'greet' before initialization

   const greet = function () {
     console.log("Hello, world!");
   };
   ```

2. **Naming**:
   - Function expressions can be **anonymous** or **named**.
   - Anonymous functions have no name and are often used in callbacks or immediately invoked function expressions (IIFEs).
   - Named function expressions provide a name for the function, which can be useful for debugging or recursion.

   Example:
   ```javascript
   const factorial = function fact(n) {
     if (n <= 1) return 1;
     return n * fact(n - 1); // Uses the `fact` name internally
   };

   console.log(factorial(5)); // Output: 120
   ```

3. **Scope**:
   - Like function declarations, function expressions are block-scoped if declared using `let` or `const`, and function-scoped if declared with `var`.

---

### Key Differences at a Glance

| Feature                 | Function Declaration                          | Function Expression                          |
|-------------------------|-----------------------------------------------|---------------------------------------------|
| **Hoisting**            | Hoisted (can be called before the declaration). | Not hoisted (cannot be called before initialization). |
| **Syntax**              | Uses the `function` keyword followed by a name and parameters. | Can be anonymous or named, typically assigned to a variable. |
| **Use Case**            | Often used for declaring reusable functions.   | Often used in callbacks, IIFEs, or dynamic function definitions. |
| **Debugging/Recursion** | The function name is always available for debugging or recursion. | Named function expressions allow internal reference for recursion. |
| **When It’s Defined**   | Defined at compile time.                      | Defined at runtime (when the assignment is reached). |

---

### Practical Example: Side-by-Side Comparison

```javascript
// Function Declaration
function add(a, b) {
  return a + b;
}

console.log(add(2, 3)); // Output: 5
```

```javascript
// Function Expression
const subtract = function (a, b) {
  return a - b;
};

console.log(subtract(5, 2)); // Output: 3
```

**Behavioral Difference:**
```javascript
// Function Declaration (hoisted)
console.log(declared()); // Output: "I am declared"

function declared() {
  return "I am declared";
}

// Function Expression (not hoisted)
console.log(expression()); // Error: Cannot access 'expression' before initialization

const expression = function () {
  return "I am an expression";
};
```

---

### Conclusion
The choice between **function declarations** and **function expressions** depends on the use case:
- Use **function declarations** for defining reusable, named functions in predictable contexts.
- Use **function expressions** when you need flexibility, such as creating anonymous functions, using closures, or defining functions conditionally.