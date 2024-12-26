# Question 12 - Design Patterns

## Discuss scenarios where a Singleton pattern might be useful or harmful.

The **Singleton pattern** is a design pattern that restricts the instantiation of a class to a single instance and provides a global point of access to that instance. In JavaScript, this is often implemented using closures or ES6 modules. Below are scenarios where using a Singleton pattern can be **useful** or **harmful**:

---

### **Useful Scenarios**
1. **Shared Resource Management:**
   - When managing a resource that is expensive to instantiate (e.g., database connections, in-memory cache, or file system handlers).
   - Example: A single database connection pool shared across an application to avoid multiple expensive connections.

   ```javascript
   const DatabaseConnection = (function () {
       let instance;

       function createConnection() {
           return { id: Math.random(), status: "connected" }; // Mock database connection
       }

       return {
           getInstance: function () {
               if (!instance) {
                   instance = createConnection();
               }
               return instance;
           },
       };
   })();

   const connection1 = DatabaseConnection.getInstance();
   const connection2 = DatabaseConnection.getInstance();
   console.log(connection1 === connection2); // true
   ```

2. **Global State Management:**
   - For applications needing a global state, like managing a configuration or user settings across modules.
   - Example: A global configuration object for app-wide settings.

   ```javascript
   const Config = (function () {
       let instance;

       function createConfig() {
           return { apiBaseUrl: "https://api.example.com", debugMode: true };
       }

       return {
           getInstance: function () {
               if (!instance) {
                   instance = createConfig();
               }
               return instance;
           },
       };
   })();
   ```

3. **Logging Service:**
   - To ensure all parts of an application write logs to the same service or destination.
   - Example: A logging service that outputs to the console or a file.

   ```javascript
   class Logger {
       static instance;

       constructor() {
           if (Logger.instance) return Logger.instance;
           this.logs = [];
           Logger.instance = this;
       }

       log(message) {
           this.logs.push(message);
           console.log(message);
       }

       getLogs() {
           return this.logs;
       }
   }

   const logger1 = new Logger();
   const logger2 = new Logger();
   console.log(logger1 === logger2); // true
   ```

4. **Browser-Based Scenarios:**
   - In browser-based apps, a Singleton can manage shared objects like a Service Worker instance or WebSocket connection.

---

### **Harmful Scenarios**
1. **Hidden Dependencies:**
   - Since a Singleton provides global access, it introduces implicit dependencies, making the code harder to understand and maintain.
   - This can violate the Dependency Injection principle, making unit testing difficult.

   ```javascript
   const instance = MySingleton.getInstance(); // Implicit dependency
   ```

   **Why harmful?** Modules using the Singleton are now tightly coupled to it, reducing reusability.

2. **Scalability Issues in Multi-Threaded Environments:**
   - In server-side JavaScript (e.g., Node.js), using Singletons for stateful objects can create bottlenecks when handling concurrent requests.
   - Example: If a Singleton is used for in-memory storage, it might cause data corruption or race conditions when accessed by multiple threads.

   **Why harmful?** Singletons can introduce shared mutable state, leading to bugs in concurrent environments.

3. **Testing Challenges:**
   - Singleton instances persist across tests, which can lead to unexpected test results due to leftover state.
   - Example: If a Singleton logs test data, its logs might persist into unrelated tests, creating flaky or failing tests.

   **Why harmful?** Makes mocking or resetting state for unit tests cumbersome.

4. **Global State Anti-Pattern:**
   - Overuse of Singletons can lead to global state management issues, where debugging becomes difficult due to unpredictable changes in shared state.
   - Example: Multiple modules modifying a Singleton’s state in ways that are hard to trace.

   ```javascript
   const GlobalState = (function () {
       let state = { counter: 0 };
       return {
           getState: () => state,
           updateCounter: (value) => (state.counter += value),
       };
   })();

   // Difficult to track state changes when accessed from multiple places
   GlobalState.updateCounter(1);
   GlobalState.updateCounter(2);
   ```

5. **Alternatives Ignored:**
   - Using a Singleton might prevent considering better alternatives like Dependency Injection, which promotes flexibility and testability.

---

### **Summary**
#### **When Singleton is useful:**
- When a single shared instance makes sense conceptually (e.g., a logger or configuration manager).
- When controlling resource access (e.g., database connection pools).
- When enforcing a single instance globally simplifies the architecture.

#### **When Singleton is harmful:**
- When it introduces tight coupling and hidden dependencies.
- When shared state management creates concurrency or scalability issues.
- When testing becomes cumbersome due to persistent state.
- When it is used inappropriately, leading to over-reliance on global state.

**Best Practice:** Use Singletons judiciously, ensuring they solve a real problem without introducing unnecessary complexity or tight coupling.