# Question 06 - Design Patterns

## What is the difference between Structural, Behavioral, and Creational Design Patterns? Can you give a JavaScript example for each category?

Design patterns are categorized into **Structural**, **Behavioral**, and **Creational** patterns based on their purpose and implementation.

---

### 1. **Structural Design Patterns**
These patterns deal with object composition and simplify the design by identifying relationships between entities.

#### Example: **Adapter Pattern**
The Adapter pattern allows incompatible interfaces to work together. It acts as a bridge between two incompatible interfaces.

**JavaScript Example:**
```javascript
// Old interface
class OldAPI {
  request() {
    return "Old API response";
  }
}

// New interface
class NewAPI {
  specificRequest() {
    return "New API response";
  }
}

// Adapter to make NewAPI compatible with OldAPI
class Adapter {
  constructor(newAPI) {
    this.newAPI = newAPI;
  }

  request() {
    return this.newAPI.specificRequest();
  }
}

// Usage
const oldApi = new OldAPI();
console.log(oldApi.request()); // Old API response

const newApi = new NewAPI();
const adaptedApi = new Adapter(newApi);
console.log(adaptedApi.request()); // New API response
```

---

### 2. **Behavioral Design Patterns**
These patterns are concerned with communication between objects and the delegation of responsibilities.

#### Example: **Observer Pattern**
The Observer pattern defines a one-to-many dependency, so when one object (subject) changes state, all its dependents (observers) are notified.

**JavaScript Example:**
```javascript
// Subject
class Subject {
  constructor() {
    this.observers = [];
  }

  attach(observer) {
    this.observers.push(observer);
  }

  detach(observer) {
    this.observers = this.observers.filter((obs) => obs !== observer);
  }

  notify(data) {
    this.observers.forEach((observer) => observer.update(data));
  }
}

// Observer
class Observer {
  constructor(name) {
    this.name = name;
  }

  update(data) {
    console.log(`${this.name} received data: ${data}`);
  }
}

// Usage
const subject = new Subject();

const observer1 = new Observer("Observer 1");
const observer2 = new Observer("Observer 2");

subject.attach(observer1);
subject.attach(observer2);

subject.notify("Hello Observers!");

// Output:
// Observer 1 received data: Hello Observers!
// Observer 2 received data: Hello Observers!
```

---

### 3. **Creational Design Patterns**
These patterns focus on object creation mechanisms, ensuring flexibility and reusability.

#### Example: **Factory Pattern**
The Factory pattern provides a way to create objects without specifying their exact class.

**JavaScript Example:**
```javascript
// Factory
class ShapeFactory {
  createShape(type) {
    switch (type) {
      case "circle":
        return new Circle();
      case "square":
        return new Square();
      default:
        throw new Error("Invalid shape type");
    }
  }
}

// Circle class
class Circle {
  draw() {
    console.log("Drawing a Circle");
  }
}

// Square class
class Square {
  draw() {
    console.log("Drawing a Square");
  }
}

// Usage
const factory = new ShapeFactory();

const circle = factory.createShape("circle");
circle.draw(); // Drawing a Circle

const square = factory.createShape("square");
square.draw(); // Drawing a Square
```

---

### Summary
- **Structural Patterns:** Focus on composition (e.g., Adapter pattern).
- **Behavioral Patterns:** Focus on communication between objects (e.g., Observer pattern).
- **Creational Patterns:** Focus on object creation (e.g., Factory pattern). 

Each example demonstrates a practical use case for JavaScript implementations of these design patterns.