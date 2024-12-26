# Question 13 - Design Patterns

## Demonstrate how you can use the Object.create() method in implementing the Prototype Pattern.

The `Object.create()` method is an excellent way to implement the Prototype Pattern in JavaScript because it allows you to create a new object that directly inherits from another object (the prototype). Below is a demonstration of how you can use `Object.create()` to implement the Prototype Pattern:

### Example: Prototype Pattern with `Object.create()`

```javascript
// Define a prototype object
const Animal = {
  init(type, sound) {
    this.type = type;
    this.sound = sound;
  },
  makeSound() {
    console.log(`${this.type} says: ${this.sound}`);
  },
};

// Use Object.create() to create a new object inheriting from Animal
const dog = Object.create(Animal);
dog.init("Dog", "Woof");
dog.makeSound(); // Output: Dog says: Woof

// Create another object inheriting from Animal
const cat = Object.create(Animal);
cat.init("Cat", "Meow");
cat.makeSound(); // Output: Cat says: Meow

// Add specific behavior to one object without affecting the prototype
dog.fetch = function () {
  console.log(`${this.type} is fetching!`);
};
dog.fetch(); // Output: Dog is fetching!

// cat doesn't have the fetch method
cat.fetch?.(); // Undefined or an error, depending on strictness
```

### Explanation:
1. **Prototype Object**:
   - The `Animal` object acts as the prototype. It has methods (`init` and `makeSound`) and serves as the base for other objects.
   
2. **Inheritance via `Object.create()`**:
   - The `Object.create()` method creates a new object (`dog` or `cat`) that inherits directly from the `Animal` object.

3. **Shared Behavior**:
   - The methods defined on the prototype (`Animal`) are shared among all objects created with `Object.create(Animal)`.

4. **Customization**:
   - Each instance can have its own properties (e.g., `type` and `sound`) and even additional methods without affecting other instances or the prototype.

### Advantages:
- **Memory Efficiency**: Shared methods are defined once in the prototype, saving memory.
- **Flexibility**: You can easily extend or override the functionality of specific objects while preserving shared behavior.

This approach encapsulates behavior in a reusable way, adhering to the principles of the Prototype Pattern.