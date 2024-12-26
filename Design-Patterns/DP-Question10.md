# Question 10 - Design Patterns

## Describe the Command Pattern in JavaScript. How would you implement it for undo/redo functionality?


The **Command Pattern** is a behavioral design pattern that encapsulates a request as an object, allowing you to parameterize methods with different requests, queue requests, and support undoable operations. In JavaScript, this pattern is especially useful for implementing undo/redo functionality because it enables you to keep track of executed commands and reverse their effects.

### Key Concepts of the Command Pattern
1. **Command**: An interface or abstract class defining the operation(s) to execute.
2. **Concrete Command**: Implements the `Command` interface and encapsulates the receiver and an action (or a series of actions).
3. **Invoker**: Calls the command's `execute` method.
4. **Receiver**: The actual object that performs the work.
5. **Client**: Configures the command and passes it to the invoker.

---

### Example: Undo/Redo Functionality with the Command Pattern

Here’s how you can implement undo/redo functionality using the Command Pattern in JavaScript:

#### Step 1: Define a `Command` Interface
```javascript
class Command {
  execute() {}
  undo() {}
}
```

#### Step 2: Create Concrete Commands
For this example, we'll simulate an editor where you can add and remove text.

```javascript
class AddTextCommand extends Command {
  constructor(receiver, text) {
    super();
    this.receiver = receiver;
    this.text = text;
  }

  execute() {
    this.receiver.addText(this.text);
  }

  undo() {
    this.receiver.removeText(this.text);
  }
}

class RemoveTextCommand extends Command {
  constructor(receiver, text) {
    super();
    this.receiver = receiver;
    this.text = text;
  }

  execute() {
    this.receiver.removeText(this.text);
  }

  undo() {
    this.receiver.addText(this.text);
  }
}
```

#### Step 3: Create the Receiver
The receiver performs the actual work of adding and removing text.

```javascript
class TextEditor {
  constructor() {
    this.content = "";
  }

  addText(text) {
    this.content += text;
    console.log(`Added: "${text}" | Current Content: "${this.content}"`);
  }

  removeText(text) {
    this.content = this.content.replace(new RegExp(`${text}$`), "");
    console.log(`Removed: "${text}" | Current Content: "${this.content}"`);
  }
}
```

#### Step 4: Create an Invoker
The invoker manages the command history and provides undo/redo functionality.

```javascript
class CommandManager {
  constructor() {
    this.history = [];
    this.undoStack = [];
  }

  executeCommand(command) {
    command.execute();
    this.history.push(command);
    this.undoStack = []; // Clear the redo stack on new command
  }

  undo() {
    if (this.history.length > 0) {
      const command = this.history.pop();
      command.undo();
      this.undoStack.push(command);
    } else {
      console.log("Nothing to undo.");
    }
  }

  redo() {
    if (this.undoStack.length > 0) {
      const command = this.undoStack.pop();
      command.execute();
      this.history.push(command);
    } else {
      console.log("Nothing to redo.");
    }
  }
}
```

#### Step 5: Client Code
```javascript
const editor = new TextEditor();
const manager = new CommandManager();

const command1 = new AddTextCommand(editor, "Hello ");
const command2 = new AddTextCommand(editor, "World!");
const command3 = new RemoveTextCommand(editor, "World!");

manager.executeCommand(command1); // Adds "Hello "
manager.executeCommand(command2); // Adds "World!"
manager.undo();                  // Removes "World!"
manager.redo();                  // Adds "World!" again
manager.executeCommand(command3); // Removes "World!"
manager.undo();                  // Re-adds "World!"
```

---

### Output Example
```plaintext
Added: "Hello " | Current Content: "Hello "
Added: "World!" | Current Content: "Hello World!"
Removed: "World!" | Current Content: "Hello "
Added: "World!" | Current Content: "Hello World!"
Removed: "World!" | Current Content: "Hello "
Added: "World!" | Current Content: "Hello World!"
```

---

### Benefits of the Command Pattern
- **Encapsulation**: Commands encapsulate the details of an action, making the system more modular.
- **Undo/Redo Support**: By storing command history, you can easily implement undo/redo functionality.
- **Extensibility**: Adding new commands (e.g., `ReplaceTextCommand`) doesn’t require modifying existing code.

This pattern is especially useful in scenarios like text editors, drawing applications, or any system requiring reversible actions.