# Java Memory Management

## How Memory Works:
### The Role of Stack:

- When ever memory is used, it is either in stack or heap.
- Each thread will have its own stack.
- Stack if LIFO data structure.
- Each value is added to the top of the stack (stack.add)
- For primitives, copy of the variable is passed on to the method.
- When the value is done, that will be removed from the stack (Stack.pop)
- When the program is completed (code block), the stack will be pop the values corresponding to the block variables.
- Data is stack will have short life.

### The Role of Heap:

- Allows to store data that have longer life time than just the code block. Ex. Object creation, where it will be used by multiple code blocks.
- In an application there will be only one heap in general.
- All objects are stored on the heap. (including string)
- Local variable reference will be in the stack, but the actual object will be in the heap. A pointer is created to the address of object in the stack for the variable.

---
## Values and Reference:

### Passing Variables by Value:
- Passing parameters in to methods.
> Whenever we pass arguments, we always pass the copy of the value into the method being called.
- Java uses, passing variables by value.
- Other languages will support `byref`[not supported in java].
- When we pass the object (instead of primitive), copy of the variable will hold the address of the object in the stack and it will be passed. So the copy of address will be created in case of object being passed as the method arguments.

### The final Keyword:
- The keyword says that the object reference for the variable cannot be changes.
- There is no `const` keyword in java.
- The `final` keyword does not guarantee the object changes.
- It is advisable to make the arguments `final` to avoid unnecessary modifying objects.

---
## Escaping Reference:

- Encapsulation is one of idea of OOP. Reference to the data in the class is encapsulated.
- The references escaped outside the class which is not the ideal.

```
public class Customers {
    private Map<String, Customer> records;

    public Customers() {
        this.records = new HashMap<>();
    }

    public void addCustomer(Customer c) {
        this.records.put(c.getName(), c);
    }

    //Here the by having access to getCustomers(), the entire records can be manipulated outside the class basically the reference in escaped from the scope of this class.
    //This returns the pointer of the actual records object.
    public Map<String, Customer> getCustomers() {
        return this.records;
    }
    
}
```
- So instead of returning the records directly, return the copy of the records. This way the user will not know what is happening until he go through the source code[not elegant solution].  Return a unmodifiable collection instead.
- For general object type, we should send only a read only copy of the object. By using `interfaces` we will give only the read only methods and properties to the user. In the above case, we can send the read_only class.

---
## Garbage Collection:

