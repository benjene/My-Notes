
Things to know
1. Decorators have the same supertype as the objects they decorate
2. b


Ch 6 - Command
1. Client creates command obj, a set of actions on a receiver
    - Command object has method execute(), called to invoke the actions on the receiver
2. Client calls setCommand() on Invoker objecct, passes Invoker obj to command obj
3. Client gives go ahead for execution.
4. The invoker calls the command object's execute() 
5. Receiver has actions invoked

## Common interface
- This Command interface implemented by all methods
- consists of one method execute()

#### Commands that implement Command
e.g. LightsOnCommand:

```
public class LightOnCommand implements Command {
    Light light;
   public LightOnCommand(Light light) {
       this.light = light;
}
   public void execute() {
       light.on();
    }
}
```
