# Question 01 - Design Patterns

## Can you explain the Singleton Pattern and its potential pitfalls in JavaScript? How can you implement a thread-safe Singleton?

The **Singleton Pattern** is a design pattern that ensures a class has only one instance and provides a global point of access to that instance. In JavaScript, this can be implemented using closures or ES6 modules since they naturally encapsulate their code.

### **How to Implement a Singleton in JavaScript**

Here's a typical implementation of a Singleton in JavaScript:

```javascript
class Singleton {
  constructor() {
    if (Singleton.instance) {
      return Singleton.instance;
    }
    Singleton.instance = this;
    this.data = "I am the single instance";
  }

  getData() {
    return this.data;
  }
}

const instance1 = new Singleton();
const instance2 = new Singleton();

console.log(instance1 === instance2); // true
```

In this example:
- The `Singleton` class has a static property `instance` to store the single instance.
- The `constructor` checks if an instance already exists and returns it if it does, ensuring only one instance is created.

### **Potential Pitfalls**
1. **Global State Sharing**: Since a Singleton provides a single global instance, changes in one part of the code can inadvertently affect another part of the code.
2. **Testing Issues**: Singletons can make unit testing harder because they introduce hidden dependencies.
3. **Thread-Safety**: In multi-threaded environments, race conditions could lead to multiple instances being created.

While JavaScript is single-threaded in nature (due to its event loop), certain environments, like server-side Node.js, can involve asynchronous operations that might create concurrency issues.

---

### **Thread-Safe Singleton in JavaScript**

Though JavaScript is not inherently multi-threaded, we can implement a thread-safe Singleton to simulate better control over the initialization process, especially when used in asynchronous contexts.

Here’s an example:

```javascript
class ThreadSafeSingleton {
  constructor() {
    if (ThreadSafeSingleton.instance) {
      return ThreadSafeSingleton.instance;
    }

    // Initialize the instance only once
    this.data = "I am the thread-safe single instance";
    ThreadSafeSingleton.instance = this;
  }

  static getInstance() {
    if (!ThreadSafeSingleton.instance) {
      // Use a lock or Promise to handle asynchronous initialization safely
      ThreadSafeSingleton.instance = new ThreadSafeSingleton();
    }
    return ThreadSafeSingleton.instance;
  }

  getData() {
    return this.data;
  }
}

// Usage
const instance1 = ThreadSafeSingleton.getInstance();
const instance2 = ThreadSafeSingleton.getInstance();

console.log(instance1 === instance2); // true
```

In this case:
- The `getInstance` static method ensures that the instance is created only once.
- The logic could be enhanced with promises or locking mechanisms for more complex asynchronous setups.

---

### **Asynchronous Initialization Example**

If you need to handle initialization that involves asynchronous operations:

```javascript
class AsyncSingleton {
  constructor() {
    if (AsyncSingleton.instance) {
      return AsyncSingleton.instance;
    }

    this.data = null; // Placeholder for data
    AsyncSingleton.instance = this;
  }

  static async getInstance() {
    if (!AsyncSingleton.instance) {
      AsyncSingleton.instance = new AsyncSingleton();
      await AsyncSingleton.instance.init(); // Async initialization
    }
    return AsyncSingleton.instance;
  }

  async init() {
    // Simulating an async operation (e.g., fetching configuration)
    this.data = await new Promise((resolve) =>
      setTimeout(() => resolve("I am the async single instance"), 1000)
    );
  }

  getData() {
    return this.data;
  }
}

// Usage
(async () => {
  const instance1 = await AsyncSingleton.getInstance();
  const instance2 = await AsyncSingleton.getInstance();

  console.log(instance1 === instance2); // true
  console.log(instance1.getData());    // "I am the async single instance"
})();
```

### Key Takeaways
- JavaScript's natural single-threaded environment reduces the likelihood of thread safety issues, but asynchronous code can introduce race conditions.
- The Singleton Pattern is straightforward to implement in JavaScript, but careful consideration should be given to its pitfalls, particularly around testing and shared state.
- If thread safety is critical (e.g., in async contexts), use a static method or locking mechanism to control the instance creation process.