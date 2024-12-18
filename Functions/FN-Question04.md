# Question 04 - Functions

## How does JavaScript handle default function parameters? Provide an example.

In JavaScript, **default function parameters** allow you to specify default values for function arguments if they are not provided when the function is called, or if they are explicitly passed as `undefined`.

This feature was introduced in ES6 (ECMAScript 2015).

### Syntax
The default parameter is specified directly in the function declaration by assigning a value to the parameter in the parameter list.

### Example
```javascript
function greet(name = "Guest") {
    return `Hello, ${name}!`;
}

console.log(greet("Alice")); // Output: "Hello, Alice!"
console.log(greet());        // Output: "Hello, Guest!"
console.log(greet(undefined)); // Output: "Hello, Guest!"
```

### Key Points:
1. **Default values only apply when the argument is `undefined`**:
   - If you pass `null`, the default value is **not** applied. `null` is treated as a valid value.
   - Example:
     ```javascript
     console.log(greet(null)); // Output: "Hello, null!"
     ```
   
2. **Expressions as Default Values**:
   - Default values can be the result of any valid expression, including function calls.
   - Example:
     ```javascript
     function calculatePrice(price, tax = price * 0.1) {
         return price + tax;
     }
     
     console.log(calculatePrice(100));   // Output: 110
     console.log(calculatePrice(100, 20)); // Output: 120
     ```

3. **Default values are evaluated lazily**:
   - The default expression is only executed if the parameter is `undefined`.

4. **Parameter Order Matters**:
   - Default parameters rely on their position in the parameter list. If a preceding parameter is not provided, but a subsequent one is, you must pass `undefined` to skip the earlier parameter(s).
   - Example:
     ```javascript
     function greet(greeting = "Hello", name = "Guest") {
         return `${greeting}, ${name}!`;
     }

     console.log(greet());                  // Output: "Hello, Guest!"
     console.log(greet("Hi"));              // Output: "Hi, Guest!"
     console.log(greet(undefined, "Bob"));  // Output: "Hello, Bob!"
     ```

5. **Interaction with `arguments` Object**:
   - The `arguments` object does not reflect default values if arguments are omitted.
   - Example:
     ```javascript
     function logArgs(a = 1, b = 2) {
         console.log(arguments[0]); // Reflects the actual passed value or `undefined`
         console.log(arguments[1]);
     }

     logArgs(10); // Output: 10, undefined
     ```

Default function parameters enhance function flexibility, improve code readability, and help avoid errors when working with optional parameters.