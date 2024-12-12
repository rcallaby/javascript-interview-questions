# Question 03 - Advanced Async Programming

## Describe how the event loop works in JavaScript. Specifically, explain the difference between the microtask queue and the macrotask queue, and provide examples of operations that would be placed in each queue. 

The **event loop** in JavaScript is a mechanism that ensures non-blocking behavior, allowing JavaScript to handle asynchronous operations. It works by managing a queue of tasks and continuously checking for tasks that need to be executed.

### Key Concepts

1. **Call Stack**: This is where function execution contexts are placed and removed as functions are called and returned.
2. **Task Queues**:
   - **Macrotask Queue**: Includes tasks like `setTimeout`, `setInterval`, `setImmediate` (Node.js), and I/O operations. These are scheduled to run after the current script and all microtasks in the microtask queue are completed.
   - **Microtask Queue**: Includes tasks like `Promise` callbacks (`.then`, `.catch`, `.finally`), `MutationObserver`, and `queueMicrotask`. These are prioritized over macrotasks and are executed before any macrotask in the queue.

### The Event Loop Process
1. Executes code in the **call stack**.
2. Processes all tasks in the **microtask queue**.
3. Executes the next task in the **macrotask queue**.
4. Repeats the process.

### Example and Explanation

Let's illustrate the differences between the microtask queue and the macrotask queue using code:

```javascript
console.log('Script start'); // Executes first, sync code

setTimeout(() => {
  console.log('Macrotask: setTimeout'); // Macrotask
}, 0);

Promise.resolve()
  .then(() => {
    console.log('Microtask: Promise 1'); // Microtask
  })
  .then(() => {
    console.log('Microtask: Promise 2'); // Microtask
  });

queueMicrotask(() => {
  console.log('Microtask: queueMicrotask'); // Microtask
});

console.log('Script end'); // Executes last in sync code
```

#### Step-by-Step Breakdown

1. **Synchronous Code**:  
   The main script (`console.log('Script start')` and `console.log('Script end')`) is executed first.

   Output so far:  
   ```
   Script start
   Script end
   ```

2. **Microtask Queue**:  
   After the synchronous code, all microtasks are processed before any macrotasks.  
   - The first `.then` callback (`Promise 1`) is executed.
   - The second `.then` callback (`Promise 2`) is executed.
   - The `queueMicrotask` callback runs.

   Output so far:  
   ```
   Script start
   Script end
   Microtask: Promise 1
   Microtask: Promise 2
   Microtask: queueMicrotask
   ```

3. **Macrotask Queue**:  
   Finally, the `setTimeout` callback (macrotask) is executed.

   Final Output:  
   ```
   Script start
   Script end
   Microtask: Promise 1
   Microtask: Promise 2
   Microtask: queueMicrotask
   Macrotask: setTimeout
   ```

---

### Additional Examples

#### Interaction Between Microtasks and Macrotasks

```javascript
setTimeout(() => {
  console.log('Macrotask: setTimeout 1');
  
  Promise.resolve().then(() => {
    console.log('Microtask: inside setTimeout');
  });
}, 0);

Promise.resolve().then(() => {
  console.log('Microtask: standalone Promise');
});

setTimeout(() => {
  console.log('Macrotask: setTimeout 2');
}, 0);
```

**Output**:  
```
Microtask: standalone Promise
Macrotask: setTimeout 1
Microtask: inside setTimeout
Macrotask: setTimeout 2
```

#### Explanation:
1. The standalone `Promise` runs its `.then` callback first because it is a microtask.
2. The first `setTimeout` callback runs next as a macrotask.
3. The `Promise` inside the first `setTimeout` is added to the microtask queue and is executed before the second `setTimeout`.

---

### Summary Table of Task Types

| **Operation**                     | **Queue Type** | **Example**                                    |
|------------------------------------|----------------|------------------------------------------------|
| `setTimeout`, `setInterval`        | Macrotask      | `setTimeout(() => { console.log('Task'); });` |
| `Promise.then`, `Promise.catch`    | Microtask      | `Promise.resolve().then(() => {});`           |
| `queueMicrotask`                   | Microtask      | `queueMicrotask(() => {});`                   |
| DOM mutations (`MutationObserver`) | Microtask      | Changes to the DOM observed asynchronously.   |
| `setImmediate` (Node.js)           | Macrotask      | Runs after I/O events but before `setTimeout`.|

Understanding these concepts helps in writing efficient, non-blocking asynchronous JavaScript code.