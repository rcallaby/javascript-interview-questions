# Question 09 - Design Patterns

## How would you use the Strategy Pattern in JavaScript to implement multiple algorithms for a single problem (e.g., sorting)?

The **Strategy Pattern** is a behavioral design pattern that allows you to define a family of algorithms, encapsulate each one, and make them interchangeable. It enables the client code to dynamically choose the algorithm at runtime without altering its structure.

Here’s how you can implement the **Strategy Pattern** in JavaScript to handle multiple sorting algorithms:

---

### Example: Implementing Sorting Algorithms with Strategy Pattern

#### Step 1: Define the Strategy Interface
The strategy interface is a contract that all sorting algorithms must adhere to. In JavaScript, we don’t have strict interfaces, so we can document the expected method (`sort` in this case).

#### Step 2: Create Concrete Strategies
Each sorting algorithm (e.g., Bubble Sort, Quick Sort) will implement the `sort` method.

#### Step 3: Create the Context Class
The context class is responsible for using a sorting strategy. It delegates the sorting operation to the selected strategy.

---

#### Implementation in JavaScript

```javascript
// Step 1: Define the Strategy Interface
// All strategies must implement a `sort` method.

class BubbleSortStrategy {
  sort(data) {
    const arr = [...data]; // Clone the array to avoid mutating the original
    for (let i = 0; i < arr.length; i++) {
      for (let j = 0; j < arr.length - i - 1; j++) {
        if (arr[j] > arr[j + 1]) {
          [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]]; // Swap elements
        }
      }
    }
    return arr;
  }
}

class QuickSortStrategy {
  sort(data) {
    const arr = [...data]; // Clone the array to avoid mutating the original

    const quickSort = (arr) => {
      if (arr.length <= 1) return arr;
      const pivot = arr[arr.length - 1];
      const left = arr.filter((el) => el < pivot);
      const right = arr.filter((el) => el > pivot);
      return [...quickSort(left), pivot, ...quickSort(right)];
    };

    return quickSort(arr);
  }
}

// Step 2: Create the Context Class
class SortContext {
  constructor(strategy) {
    this.strategy = strategy; // Strategy instance
  }

  setStrategy(strategy) {
    this.strategy = strategy; // Dynamically set a new strategy
  }

  sort(data) {
    if (!this.strategy || typeof this.strategy.sort !== "function") {
      throw new Error("Invalid sorting strategy");
    }
    return this.strategy.sort(data); // Delegate to the strategy
  }
}

// Step 3: Using the Strategy Pattern
const data = [5, 3, 8, 4, 2];

const bubbleSort = new BubbleSortStrategy();
const quickSort = new QuickSortStrategy();

const context = new SortContext(bubbleSort); // Use Bubble Sort
console.log("Bubble Sort:", context.sort(data));

context.setStrategy(quickSort); // Switch to Quick Sort
console.log("Quick Sort:", context.sort(data));
```

---

### Key Points:
1. **Encapsulation of Algorithms**:
   - Each sorting algorithm is encapsulated in its own class (`BubbleSortStrategy`, `QuickSortStrategy`).
2. **Interchangeability**:
   - The `SortContext` class allows you to switch between sorting strategies at runtime using the `setStrategy` method.
3. **Separation of Concerns**:
   - The client code (the part calling `context.sort`) doesn’t care about the details of the sorting algorithms. It just uses the `SortContext`.

---

### Benefits of the Strategy Pattern:
- **Open/Closed Principle**: You can add new sorting algorithms without modifying existing code.
- **Flexibility**: The algorithm can be chosen dynamically based on user input or other factors.

### Real-World Use Case:
This pattern is useful in scenarios like:
- Implementing different compression algorithms (e.g., Zip, Gzip).
- Applying different payment methods (e.g., PayPal, Credit Card).
- Choosing between various data validation techniques.