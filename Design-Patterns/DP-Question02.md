# Question 02 - Design Patterns

## What is the Observer Pattern, and how does it differ from the Publish-Subscribe Pattern? Can you provide a code example of each?

The **Observer Pattern** and the **Publish-Subscribe Pattern** are both design patterns that deal with communication between components in a system. While they have similarities, they differ in their implementation and use cases. Here's an explanation of each, along with code examples:

---

### **Observer Pattern**
The **Observer Pattern** is a behavioral design pattern in which an object (called the **subject**) maintains a list of its dependents (called **observers**) and notifies them of any state changes, typically by calling one of their methods.

- **Key Characteristics**:
  - Tight coupling between subject and observers (the subject knows its observers).
  - Observers directly register themselves with the subject.

#### **Code Example (Observer Pattern in C#)**

```csharp
using System;
using System.Collections.Generic;

// Subject interface
interface ISubject
{
    void Attach(IObserver observer);
    void Detach(IObserver observer);
    void Notify();
}

// Concrete Subject
class WeatherStation : ISubject
{
    private List<IObserver> _observers = new List<IObserver>();
    private int _temperature;

    public int Temperature
    {
        get => _temperature;
        set
        {
            _temperature = value;
            Notify(); // Notify all observers when state changes
        }
    }

    public void Attach(IObserver observer) => _observers.Add(observer);
    public void Detach(IObserver observer) => _observers.Remove(observer);
    public void Notify()
    {
        foreach (var observer in _observers)
        {
            observer.Update(_temperature);
        }
    }
}

// Observer interface
interface IObserver
{
    void Update(int temperature);
}

// Concrete Observer
class PhoneDisplay : IObserver
{
    public void Update(int temperature)
    {
        Console.WriteLine($"Phone Display: Temperature is now {temperature}°C");
    }
}

// Main
class Program
{
    static void Main()
    {
        WeatherStation station = new WeatherStation();
        PhoneDisplay phoneDisplay = new PhoneDisplay();

        station.Attach(phoneDisplay);

        station.Temperature = 25; // Output: Phone Display: Temperature is now 25°C
        station.Temperature = 30; // Output: Phone Display: Temperature is now 30°C
    }
}
```

---

### **Publish-Subscribe Pattern**
The **Publish-Subscribe Pattern** is a messaging pattern where senders (**publishers**) and receivers (**subscribers**) of messages are decoupled through a **message broker** (or **event bus**). Subscribers register their interest in certain types of messages, and publishers send messages to the broker without knowing the recipients.

- **Key Characteristics**:
  - Loose coupling between publishers and subscribers (they don't know about each other).
  - Typically implemented with an intermediary (message broker or event bus).

#### **Code Example (Publish-Subscribe Pattern in C#)**

```csharp
using System;
using System.Collections.Generic;

// Event Broker
class EventBus
{
    private readonly Dictionary<string, List<Action<string>>> _subscribers = new();

    public void Subscribe(string eventType, Action<string> handler)
    {
        if (!_subscribers.ContainsKey(eventType))
        {
            _subscribers[eventType] = new List<Action<string>>();
        }
        _subscribers[eventType].Add(handler);
    }

    public void Publish(string eventType, string message)
    {
        if (_subscribers.ContainsKey(eventType))
        {
            foreach (var handler in _subscribers[eventType])
            {
                handler(message);
            }
        }
    }
}

// Main
class Program
{
    static void Main()
    {
        EventBus bus = new EventBus();

        // Subscriber 1
        bus.Subscribe("TemperatureChange", message =>
        {
            Console.WriteLine($"Phone Display received: {message}");
        });

        // Subscriber 2
        bus.Subscribe("TemperatureChange", message =>
        {
            Console.WriteLine($"Laptop Display received: {message}");
        });

        // Publisher
        bus.Publish("TemperatureChange", "Temperature is now 25°C");
        // Output:
        // Phone Display received: Temperature is now 25°C
        // Laptop Display received: Temperature is now 25°C
    }
}
```

---

### **Key Differences**
| Aspect                   | Observer Pattern                              | Publish-Subscribe Pattern                         |
|--------------------------|-----------------------------------------------|-------------------------------------------------|
| **Coupling**             | Tight coupling (subject knows observers).    | Loose coupling (mediator decouples participants). |
| **Direct Communication** | Observers register directly with the subject.| Subscribers register with an intermediary.       |
| **Use Case**             | Suitable for simple, tightly coupled systems.| Suitable for decoupled, large-scale systems.     |
| **Intermediary**         | No intermediary is involved.                 | Requires a message broker or event bus.          |

---

Both patterns are useful in different scenarios. The **Observer Pattern** is simpler and more direct, while the **Publish-Subscribe Pattern** is more scalable and suitable for distributed systems.