# Command Pattern

### Description
the command pattern is a behavioral pattern that encapsulates all of the logic to complete a task inside of an object.  This is a very common pattern and can help to abstract more complex operations away from client code.  The command pattern is usually mixed with the Unit of Work pattern for database operations.  Multiple commands can be grouped together to execute them in order. (This is especially helpful in designing systems that support multi-level undo.) The Invoker and the actions can be decoupled. The invoker doesn’t need to know how the operations are implemented.

#### Practical uses
Anytime you are interacting with a database, consider a command pattern to group operations logically and in order.  Anytime there is an operation that could require an undo, use the command pattern.

#### Example

```javascript
interface ICommand {
  exectute(): void;
}

/* 
 *Impliment your business logic here.
 */
class CommandReceiver {
  public operation1() {
    // Some business logic
  }
  public operation2() {
    // Some additional business logic
  }
}


/* 
 *This class can be implimented to take more than one reciver.
 */
class Command impliments IComand {
  private commandReciever: CommandReciever;
  
  constructor(receiver: CommandReceiver) {
    this.commandReceiver = reciever;

  public execute() {
    this.commandReceiver.operation1();
    this.commandReceiver.operation2();
    etc...
  }
}
    
    
/* 
 *Create a class to invoke your commands
 */
class CommandInvoker {
  
  public invoke(command: Command) {
      command.execute();
  }
}

    
const invoker = new CommandInvoker();
const receiver = new CommandReceiver();
invoker.execute(new Command(receiver));
```