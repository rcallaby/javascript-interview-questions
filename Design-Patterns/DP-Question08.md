# Question 08 - Design Patterns

## Can you explain the Module Pattern in JavaScript? How does it differ from ES6 Modules?

The **Module Pattern** in JavaScript is a design pattern used to encapsulate functionality and create private and public access to variables and methods. It's a way to organize and structure code in a reusable and maintainable way.

### Module Pattern (Pre-ES6)
The Module Pattern typically uses **closures** to achieve encapsulation. Here's how it works:

1. **Private Scope**: Variables and functions are wrapped inside an IIFE (Immediately Invoked Function Expression), which creates a private scope. These cannot be accessed directly from the outside.
2. **Public Interface**: An object (or function) is returned from the IIFE, exposing only the methods and properties you want to make public.

#### Example of the Module Pattern
```javascript
const Module = (function () {
  // Private variables and methods
  let privateVar = "I'm private";

  function privateMethod() {
    console.log("Accessing private method:", privateVar);
  }

  // Public API
  return {
    publicMethod: function () {
      console.log("Accessing public method");
      privateMethod();
    },
    setPrivateVar: function (value) {
      privateVar = value;
    },
  };
})();

Module.publicMethod(); // Accessing public method
                       // Accessing private method: I'm private
Module.setPrivateVar("New private value");
Module.publicMethod(); // Accessing public method
                       // Accessing private method: New private value

// Cannot directly access privateVar or privateMethod
// console.log(Module.privateVar); // undefined
// Module.privateMethod(); // TypeError
```

### ES6 Modules
With ES6, JavaScript introduced a standardized module system built into the language. It provides a cleaner and more formal way to define modules and manage dependencies.

1. **File-Based Modules**: Each JavaScript file can be treated as a module. Code inside a module is private by default and only accessible if explicitly exported.
2. **Export/Import Syntax**: You use `export` to expose variables, functions, or classes, and `import` to bring them into other files.
3. **Static Imports**: ES6 modules are statically analyzed at compile time, which allows for optimizations like tree-shaking.

#### Example of ES6 Modules
**math.js (module)**
```javascript
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

// Private (not exported, can't be accessed outside)
const secret = "This is private";
```

**main.js**
```javascript
import { add, subtract } from "./math.js";

console.log(add(2, 3)); // 5
console.log(subtract(5, 3)); // 2

// Cannot access `secret` because it's not exported
// console.log(secret); // ReferenceError
```

### Key Differences Between Module Pattern and ES6 Modules

| **Feature**                | **Module Pattern**                                       | **ES6 Modules**                                             |
|----------------------------|----------------------------------------------------------|------------------------------------------------------------|
| **Encapsulation**          | Achieved via closures and IIFEs.                         | Achieved via file-based scoping; code is private by default.|
| **Syntax**                 | Uses custom implementation for public/private separation.| Uses standardized `export` and `import` keywords.          |
| **Dependency Management**  | No native dependency management; requires manual handling.| Built-in dependency management.                           |
| **Private Members**        | Explicitly hidden using closures.                        | Private by default unless exported.                       |
| **Runtime vs Static**      | Modules are created and configured at runtime.           | Modules are statically analyzed at compile time.          |
| **Performance**            | May require additional effort to optimize.               | Optimized by modern bundlers and browsers (e.g., tree-shaking).|

### When to Use Which?
- **Module Pattern**: Useful in older environments or for quick modularization in non-ES6-compatible browsers.
- **ES6 Modules**: Preferred for modern JavaScript development due to its cleaner syntax, better tooling support, and native browser implementation.