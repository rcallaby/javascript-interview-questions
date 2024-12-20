# Question 07 - Design Patterns

## What is the Decorator Pattern, and how can it be applied in JavaScript to add functionality to objects or classes?

The **Decorator Pattern** is a structural design pattern that allows you to dynamically add behavior or responsibilities to objects without modifying their code. This is achieved by "wrapping" the original object in a new object or function that provides the additional functionality. 

In JavaScript, this can be applied in multiple ways: to objects, functions, or classes. Below is a comprehensive breakdown of how to use the Decorator Pattern in JavaScript.

---

## **Concept**
1. **Core Component**: The original object or class whose behavior you want to extend.
2. **Decorator**: A wrapper that adds new functionality while delegating the original behavior to the core component.

---

## **Using the Decorator Pattern with Objects**

In JavaScript, you can create a decorator for objects by wrapping an object in another object.

### Example:

```javascript
function createCar() {
    return {
        drive() {
            return "The car is driving";
        }
    };
}

function addGPS(car) {
    return {
        ...car, // Delegates original methods
        navigate() {
            return "The GPS is navigating";
        }
    };
}

// Usage:
const car = createCar();
const carWithGPS = addGPS(car);

console.log(car.drive()); // "The car is driving"
console.log(carWithGPS.navigate()); // "The GPS is navigating"
console.log(carWithGPS.drive()); // "The car is driving"
```

In this example:
- The `addGPS` decorator adds a `navigate` method to the original `car` object without modifying it.

---

## **Using the Decorator Pattern with Functions**

Functions can be decorated to add functionality before or after executing the original behavior.

### Example:

```javascript
function logExecution(fn) {
    return function (...args) {
        console.log(`Executing function with arguments: ${args}`);
        const result = fn(...args);
        console.log(`Function executed. Result: ${result}`);
        return result;
    };
}

function add(a, b) {
    return a + b;
}

// Usage:
const loggedAdd = logExecution(add);

loggedAdd(2, 3);
// Output:
// Executing function with arguments: 2,3
// Function executed. Result: 5
```

In this example:
- The `logExecution` function decorates `add` by logging details before and after execution.

---

## **Using the Decorator Pattern with Classes**

In JavaScript, the ES7 decorator syntax (`@decorator`) is often used with classes, but this requires a transpiler like Babel or TypeScript because it's not yet part of the official ECMAScript specification. Alternatively, you can manually apply decorators to classes or their methods.

### Using ES7 Decorator Syntax:

```javascript
function LogMethod(target, propertyKey, descriptor) {
    const originalMethod = descriptor.value;

    descriptor.value = function (...args) {
        console.log(`Method ${propertyKey} called with args: ${args}`);
        const result = originalMethod.apply(this, args);
        console.log(`Method ${propertyKey} returned: ${result}`);
        return result;
    };

    return descriptor;
}

class Calculator {
    @LogMethod
    add(a, b) {
        return a + b;
    }
}

// Usage:
const calc = new Calculator();
calc.add(2, 3);
// Output:
// Method add called with args: 2,3
// Method add returned: 5
```

### Manually Applying Decorators:

```javascript
class Calculator {
    add(a, b) {
        return a + b;
    }
}

function logMethod(originalMethod) {
    return function (...args) {
        console.log(`Calling method with arguments: ${args}`);
        const result = originalMethod.apply(this, args);
        console.log(`Result: ${result}`);
        return result;
    };
}

// Apply decorator manually:
const calc = new Calculator();
calc.add = logMethod(calc.add);

calc.add(2, 3);
// Output:
// Calling method with arguments: 2,3
// Result: 5
```

---

## **Benefits of the Decorator Pattern**
1. **Flexible Extension**: Behavior can be extended at runtime.
2. **Open/Closed Principle**: The core object or class is open for extension but closed for modification.
3. **Composable**: Multiple decorators can be combined to create a chain of behaviors.

---

## **Key Considerations**
- Decorators can add overhead due to function wrapping, so use them judiciously.
- In ES6 and beyond, the spread operator (`...`) is helpful for object decorators, but care must be taken to ensure references to original methods are preserved.
- For class decorators, compatibility with modern JavaScript requires understanding the current stage of decorator proposals.

By leveraging the Decorator Pattern in JavaScript, you can build extensible, reusable, and clean code for adding functionality dynamically.