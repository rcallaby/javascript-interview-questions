# Question 05 - Design Patterns

## Explain the Module Pattern in JavaScript and compare it with ES6 Modules. What are the advantages and limitations of each? 

### The Module Pattern in JavaScript

The **Module Pattern** is a design pattern that provides a way to encapsulate private and public methods and variables. It allows for the creation of a self-contained module that exposes only what is necessary, using closures for encapsulation. It was widely used before ES6 introduced native modules, enabling developers to simulate modularity in JavaScript.

#### Features
- Uses **IIFE** (Immediately Invoked Function Expression) to create a private scope.
- Exposes specific parts of the module via a returned object.
- Helps avoid global namespace pollution.

#### Example

```javascript
const Module = (function () {
    // Private variables and methods
    let privateVar = "I am private";

    function privateMethod() {
        console.log(privateVar);
    }

    // Public variables and methods
    return {
        publicVar: "I am public",

        publicMethod: function () {
            console.log("Accessing private method:");
            privateMethod();
        }
    };
})();

console.log(Module.publicVar); // Output: I am public
Module.publicMethod();
// Output: 
// Accessing private method:
// I am private

// Trying to access privateVar or privateMethod will fail
// console.log(Module.privateVar); // undefined
// Module.privateMethod(); // Error: Module.privateMethod is not a function
```

#### Advantages
1. Encapsulation: Keeps private data hidden.
2. Global Namespace Safety: Avoids polluting the global scope.
3. Flexibility: Allows for custom implementations.

#### Limitations
1. Harder to Test: Private variables and methods cannot be directly accessed for unit tests.
2. Static Structure: Cannot dynamically load or lazy-load parts of the module.
3. Dependency Management: Does not natively handle imports/exports; requires manual handling.

---

### ES6 Modules

ES6 introduced **native modules** to JavaScript. These are syntactically and semantically defined ways to manage modularity in a straightforward and standard manner. They work at the language level and leverage `import` and `export` statements.

#### Example

**`math.js`**
```javascript
// Named exports
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

// Default export
const multiply = (a, b) => a * b;
export default multiply;
```

**`main.js`**
```javascript
import multiply, { add, subtract } from './math.js';

console.log(add(2, 3));        // Output: 5
console.log(subtract(5, 3));   // Output: 2
console.log(multiply(4, 5));   // Output: 20
```

#### Advantages
1. Native Support: Built into modern JavaScript engines, no extra workarounds required.
2. Static Structure: Imports and exports are statically analyzed at compile time, improving performance and tooling support (e.g., tree shaking).
3. Dependency Management: Automatically resolves dependencies, avoiding manual linkage.
4. Better Readability: Uses explicit syntax for exporting and importing.

#### Limitations
1. Browser Compatibility: Requires modern browsers or tools like Babel to transpile for older environments.
2. File-Based Modules: Tied to files, so each module is a file by design.
3. Not Private: All exported members are accessible, offering no built-in mechanism for private members.

---

### Comparison

| **Feature**             | **Module Pattern**                                | **ES6 Modules**                                |
|--------------------------|--------------------------------------------------|------------------------------------------------|
| **Encapsulation**        | Achieved via closures, supports private members  | No direct private member support               |
| **Syntax**               | Uses IIFE and manual object return               | Uses `export` and `import` keywords            |
| **Dependency Management**| Manual                                          | Automatic                                      |
| **Dynamic Loading**      | Possible but not native                          | Native with `import()`                         |
| **Performance**          | Runtime analysis (less efficient)               | Static analysis (more efficient)              |
| **Browser Compatibility**| Works with all browsers                          | Requires modern browsers or transpilation      |

---

### When to Use Which?

1. **Module Pattern**:
   - Use it in legacy projects where ES6 is not available.
   - When you need private members for encapsulation.
   - Lightweight solutions without dependency on build tools.

2. **ES6 Modules**:
   - Use it for modern projects with ES6+ support.
   - When you need static analysis for performance and better tooling.
   - To integrate with other modern JavaScript libraries or frameworks.

Both patterns solve the problem of modularity but are suited for different contexts. The **Module Pattern** is great for encapsulation, while **ES6 Modules** offer better scalability and integration in modern workflows.