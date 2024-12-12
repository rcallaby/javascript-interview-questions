# Question 03 - Design Pattern

## How would you implement the Strategy Pattern in JavaScript to handle different payment methods (e.g., credit card, PayPal, cryptocurrency)?

The **Strategy Pattern** is a behavioral design pattern that enables selecting an algorithm's behavior at runtime. In JavaScript, you can implement it by defining a common interface (often via classes or objects with the same method signature) and then providing specific implementations for each "strategy." Here's an example of how you might implement the Strategy Pattern to handle different payment methods like credit card, PayPal, and cryptocurrency.

### Code Example

```javascript
// Step 1: Define the Strategy Interface
class PaymentStrategy {
  pay(amount) {
    throw new Error("This method should be overridden");
  }
}

// Step 2: Implement Concrete Strategies
class CreditCardPayment extends PaymentStrategy {
  constructor(cardNumber, cardHolder, expiryDate) {
    super();
    this.cardNumber = cardNumber;
    this.cardHolder = cardHolder;
    this.expiryDate = expiryDate;
  }

  pay(amount) {
    console.log(`Processing credit card payment of $${amount} for card: ${this.cardNumber}`);
    // Simulate credit card payment logic here
  }
}

class PayPalPayment extends PaymentStrategy {
  constructor(email) {
    super();
    this.email = email;
  }

  pay(amount) {
    console.log(`Processing PayPal payment of $${amount} for account: ${this.email}`);
    // Simulate PayPal payment logic here
  }
}

class CryptoPayment extends PaymentStrategy {
  constructor(walletAddress) {
    super();
    this.walletAddress = walletAddress;
  }

  pay(amount) {
    console.log(`Processing cryptocurrency payment of $${amount} to wallet: ${this.walletAddress}`);
    // Simulate cryptocurrency payment logic here
  }
}

// Step 3: Context Class to Use Strategies
class PaymentProcessor {
  constructor(paymentStrategy) {
    this.paymentStrategy = paymentStrategy;
  }

  setPaymentStrategy(paymentStrategy) {
    this.paymentStrategy = paymentStrategy;
  }

  processPayment(amount) {
    if (!this.paymentStrategy) {
      throw new Error("Payment strategy not set");
    }
    this.paymentStrategy.pay(amount);
  }
}

// Step 4: Usage Example
const creditCard = new CreditCardPayment("4111111111111111", "John Doe", "12/25");
const paypal = new PayPalPayment("john.doe@example.com");
const crypto = new CryptoPayment("0x123abc456def789ghi");

// Use the context class with different strategies
const paymentProcessor = new PaymentProcessor();

paymentProcessor.setPaymentStrategy(creditCard);
paymentProcessor.processPayment(100); // Processing credit card payment of $100

paymentProcessor.setPaymentStrategy(paypal);
paymentProcessor.processPayment(200); // Processing PayPal payment of $200

paymentProcessor.setPaymentStrategy(crypto);
paymentProcessor.processPayment(300); // Processing cryptocurrency payment of $300
```

### Key Concepts in This Implementation
1. **`PaymentStrategy` Interface**: Ensures that all strategies provide a `pay` method.
2. **Concrete Strategy Classes**:
   - `CreditCardPayment`
   - `PayPalPayment`
   - `CryptoPayment`
   Each implements its own version of the `pay` method.
3. **Context (`PaymentProcessor`)**:
   - Maintains a reference to a `PaymentStrategy`.
   - Allows runtime selection of a strategy with the `setPaymentStrategy` method.

This approach follows the **Open/Closed Principle**: you can easily add new payment methods by creating new strategy classes without modifying the existing ones.