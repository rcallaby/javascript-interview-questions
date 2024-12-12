# Question 02 - Advanced Async Programming

## Explain the differences between Promise.all(), Promise.any(), Promise.allSettled(), and Promise.race(). In what scenarios would you use each, and what happens if one of the promises in each method rejects or resolves?


### **Key Differences Between `Promise.all()`, `Promise.any()`, `Promise.allSettled()`, and `Promise.race()`**

These methods are used to handle multiple promises in JavaScript and offer different ways to aggregate or react to promise outcomes.

---

### **1. `Promise.all()`**

- **Behavior**: Resolves only when all promises in the array resolve. Rejects as soon as one promise rejects.
- **Use Case**: When all promises need to succeed for further processing (e.g., fetching data from multiple APIs and combining results).

#### **Code Example:**
```javascript
const promise1 = Promise.resolve(10);
const promise2 = Promise.resolve(20);
const promise3 = Promise.reject("Error!");

Promise.all([promise1, promise2, promise3])
  .then(results => {
    console.log("All resolved:", results); // Won't execute
  })
  .catch(error => {
    console.log("One promise rejected:", error); // Output: "One promise rejected: Error!"
  });
```

#### **Notes:**
- If one promise rejects, the entire `Promise.all` rejects with the first rejection reason.
- If all promises resolve, the result is an array of their resolved values.

---

### **2. `Promise.any()`**

- **Behavior**: Resolves as soon as **any one promise resolves**. If all promises reject, it rejects with an `AggregateError` containing all rejection reasons.
- **Use Case**: When you only need one promise to succeed (e.g., finding the fastest server or first successful response).

#### **Code Example:**
```javascript
const promise1 = Promise.reject("Error 1");
const promise2 = Promise.resolve(42);
const promise3 = Promise.reject("Error 2");

Promise.any([promise1, promise2, promise3])
  .then(result => {
    console.log("First resolved:", result); // Output: "First resolved: 42"
  })
  .catch(error => {
    console.log("All promises rejected:", error.errors);
  });
```

#### **Notes:**
- If at least one promise resolves, `Promise.any()` resolves with that value.
- If all promises reject, it throws an `AggregateError`.

---

### **3. `Promise.allSettled()`**

- **Behavior**: Waits for all promises to settle (either resolve or reject). Returns an array of objects describing the outcome (`{ status: 'fulfilled' | 'rejected', value | reason }`).
- **Use Case**: When you need the results of all promises, regardless of whether they resolve or reject (e.g., when processing all results but ignoring failures).

#### **Code Example:**
```javascript
const promise1 = Promise.resolve("Success 1");
const promise2 = Promise.reject("Failure");
const promise3 = Promise.resolve("Success 3");

Promise.allSettled([promise1, promise2, promise3])
  .then(results => {
    console.log(results);
    /*
    Output:
    [
      { status: 'fulfilled', value: 'Success 1' },
      { status: 'rejected', reason: 'Failure' },
      { status: 'fulfilled', value: 'Success 3' }
    ]
    */
  });
```

#### **Notes:**
- It **never rejects**, regardless of promise failures.
- Useful for logging or analyzing outcomes without disrupting execution.

---

### **4. `Promise.race()`**

- **Behavior**: Resolves or rejects as soon as the **first promise settles** (resolves or rejects).
- **Use Case**: When you only care about the first result (e.g., timing out operations or getting the fastest response).

#### **Code Example:**
```javascript
const promise1 = new Promise(resolve => setTimeout(resolve, 100, "Slow Success"));
const promise2 = Promise.reject("Fast Failure");

Promise.race([promise1, promise2])
  .then(result => {
    console.log("First settled:", result); // Won't execute in this case
  })
  .catch(error => {
    console.log("First settled:", error); // Output: "First settled: Fast Failure"
  });
```

#### **Notes:**
- It returns the result or error of the first settled promise, no matter whether it resolves or rejects.
- Use for timeout scenarios: reject a promise if a long-running task doesn't complete within a given timeframe.

---

### **Summary Table**

| Method              | Resolves When                              | Rejects When                                           | Result                                               |
|---------------------|--------------------------------------------|-------------------------------------------------------|------------------------------------------------------|
| `Promise.all()`     | All promises resolve                      | Any one promise rejects                               | Array of resolved values (if all succeed).          |
| `Promise.any()`     | Any one promise resolves                  | All promises reject                                   | First resolved value (if at least one succeeds).    |
| `Promise.allSettled()` | All promises settle (resolve/reject)       | Never rejects                                         | Array of `{status, value/reason}` for all promises. |
| `Promise.race()`    | First promise settles (resolve/reject)    | First promise settles with rejection                 | Result/error of the first settled promise.          |

---

### **Choosing the Right Method**

1. **`Promise.all()`**: Use when *all promises must succeed* for the application to proceed.
   - Example: Aggregating multiple API responses.

2. **`Promise.any()`**: Use when *any successful result is sufficient*.
   - Example: Querying multiple endpoints for redundancy and taking the first successful response.

3. **`Promise.allSettled()`**: Use when you need *all results, even failures*.
   - Example: Running diagnostic tasks and logging all outcomes.

4. **`Promise.race()`**: Use when you care about the *first settled promise*, whether success or failure.
   - Example: Implementing a timeout mechanism or finding the fastest response.