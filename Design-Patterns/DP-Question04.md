# Question 04 - Design Patterns

## How does the Proxy pattern work in JavaScript? Can you provide a real-world use case where it enhances performance or usability? 

The Proxy pattern in JavaScript involves creating a `Proxy` object that wraps around another object (referred to as the target) and intercepts operations performed on that target. This is accomplished using **traps**, which are functions defined on the `Proxy` handler. These traps allow you to customize or modify the behavior of fundamental operations like property access, assignment, function invocation, and more.

The Proxy pattern enhances performance or usability in various scenarios. Here's a breakdown of how it works and a real-world use case.

---

### How the Proxy Pattern Works in JavaScript

1. **Creating a Proxy**: A `Proxy` is instantiated with two arguments:
   - The target object (the one being wrapped).
   - A handler object that defines traps for various operations.

2. **Common Traps**:
   - `get(target, property, receiver)`: Intercepts property access.
   - `set(target, property, value, receiver)`: Intercepts property assignment.
   - `has(target, property)`: Intercepts the `in` operator.
   - `apply(target, thisArg, argumentsList)`: Intercepts function calls (for callable targets).
   - `construct(target, argumentsList, newTarget)`: Intercepts `new` operations (for constructor targets).

3. **Example Syntax**:
   ```javascript
   const target = { name: "John" };
   const handler = {
       get(target, property) {
           return property in target ? target[property] : `Property "${property}" does not exist.`;
       }
   };

   const proxy = new Proxy(target, handler);
   console.log(proxy.name); // John
   console.log(proxy.age);  // Property "age" does not exist.
   ```

---

### Real-World Use Case: Lazy Loading (Performance Optimization)

#### Scenario:
Imagine a scenario where you have an object containing expensive-to-compute or fetch properties, such as large datasets or API responses. Using the Proxy pattern, you can implement **lazy loading**, where the expensive computation or fetching is deferred until the specific property is accessed.

#### Implementation:
```javascript
const expensiveResource = {
    get data() {
        console.log("Fetching data...");
        // Simulate a heavy operation or API call
        return Array.from({ length: 100000 }, (_, i) => i);
    }
};

const handler = {
    get(target, property) {
        if (!(property in target)) {
            console.log(`Property "${property}" does not exist.`);
            return undefined;
        }

        // Lazily fetch or compute the property
        if (property === "data" && !target._data) {
            console.log("Initializing lazy-loaded data...");
            target._data = target.data; // Triggers the expensive operation
        }

        return target._data;
    }
};

const proxy = new Proxy(expensiveResource, handler);

// The expensive operation only happens when 'data' is accessed
console.log("Proxy created. No data fetched yet.");
console.log(proxy.data); // Fetching data...
console.log(proxy.data); // Uses cached result; no fetching
```

#### Benefits:
- **Performance**: Resources are only loaded or computed when needed, reducing initial overhead.
- **Usability**: The API remains simple; the user doesn’t have to worry about manually triggering the computation or caching.

---

### Summary

The Proxy pattern in JavaScript is a versatile tool for customizing object behavior. In the example of lazy loading, it improves performance by deferring expensive operations until absolutely necessary, while also simplifying the user interface. Other use cases include input validation, access control, logging, and reactive programming (e.g., Vue.js's reactivity system).