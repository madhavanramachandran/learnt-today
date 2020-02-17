# Java Memory Management

#### How Memory Works:
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
#### Values and Reference:

### Passing Variables by Value:
- Passing parameters in to methods.
- Whenever we pass arguments, we always pass the copy of the value into the method being called.
