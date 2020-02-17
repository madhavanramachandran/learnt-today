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

- Understand to avoid memory leaks.
- In case of stack, it is very easy to remove elements from memory (after the code block is executed.)
- If the JVM identifier that an object is not going to be used outside the scope of the block, it will create the object in stack(optimization).

### String Pools:
- Since `string` are immutable, if two variables are created with same data, java will create one object and other variable will be pointing to the same object created (in the heap). Consider the following. `internalize the string`

```
String one = "test"
String two = "test"

System.out.println(one == two); //return true
System.out.println(one.equals(two)); //return true

String three = new String("test");
System.out.println(one == three); //return false
System.out.println(one.equals(three)); //return true
```
> `==` is used to check if both object have same reference
and `equals` is used to check if two objects are same value.

### Garbage Eligibility:
- Java workouts garbage collection automatically unlike C and C++.
- Memory leaks is hard to find.
- JVM is actually a computer program written in C.
- Java Garbage collection will automatically analyze the size of the heap, detects the objects that are not being used in the heap and removes it.
- An object in the heap which cannot be reached through a reference from stack is eligible for garbage collection.
- Java also have methods to trigger the GC.
- `gc()` method suggests the JVM to run the GC. But there is no guarantee. Not recommended to run this method.

### Garbage Collection:
- Rather than searching for all the objects to be removed, GC will search for reachable objects and rescues them.
- General algorithm used in `Mark and Sweep`. Its is a two stage process [Marking and Sweeping.] GC will look for all the live references. Those that are not marked will be removed. Then the marked objects will be fragmented over time. 
- Threads will be stopped while running GC.

### Generational GC:
- If one object lives for `8` GC, it is most likely to retained.
- Way to organize the heap. Heap will be divided in to `young generation` (typically smaller) and `old generation`.
- New objects will be created in `young generation`.
- GC on young generation is called `minor collection`. Contains `eden`, `S0,` and `S1`.
- GC on old generation is `major collection`.

### Permgen:
- Old generation GC is also associated with space called `permgen` which is permanent generation. The objects in this memory will never be garbage collected. 
- All meta data information regarding classes will be stored in permgen. In servers like tomcat, if we are doing minor changes and redeploying the application, the older metadata information is never removed. For this case, when ever the new code is deployed, it is always good to stop and restart the server.
- For Java 7, all string interns are also stored in permgen.
- From Java 8, permgen is completely removed and introduced new space called `metaspace`. String interns are now in Heap memory and available for GC. Here if the meta information is invalid, the data will be removed from metaspace.


## Tuning JVM.
- `xmx` - max heap size.
- `Xms` - starting heap size
    > `-Xmx512m -Xms150m`

- `-verbose:gc` print to console when GC is happening.
- `-XX:HeapDumpOnOutOfMemory` - create a heat dump file when out of memory exception occurs.