# Call Stack: 

The **Call Stack** is a fundamental mechanism utilized by the JavaScript engine to keep track of its place in a script that calls multiple functions. Since JavaScript is a **single-threaded** programming language, it has exactly one Call Stack. This means it can only process one command or function at a time.

> **Core Concept:** The Call Stack manages **Execution Contexts** using the **LIFO** (Last In, First Out) principle.

---

## The LIFO Principle (Last In, First Out)

In the Call Stack, the **last** function pushed into the stack is the **first** one to be popped out and executed. The engine always evaluates the function residing at the very top of the stack.

### The Wedding Plates
Imagine a physical stack of plates at a buffet:
1. You place **Plate 1** on the table.
2. You place **Plate 2** on top of Plate 1.
3. You place **Plate 3** on top of Plate 2.

When a guest takes a plate, they must take the one from the top:
- **Plate 3** is picked up first.
- **Plate 2** is picked up next.
- **Plate 1** is picked up last.

The JavaScript Call Stack operates the exact same way concerning function executions!

---

##  How the Call Stack Operates Internally

Whenever a JavaScript file runs, the following lifecycle occurs:
1. **Initialization:** The JS engine creates the **Global Execution Context (GEC)** and pushes it to the bottom of the Call Stack.
2. **Function Invocation:** Whenever a function is called, the engine creates a new **Function Execution Context (FEC)** for that specific function and pushes it to the *top* of the Call Stack.
3. **Execution:** The engine executes the function at the top of the stack.
4. **Completion (Pop):** Once the function finishes (or returns a value), its execution context is popped off the Call Stack, and the engine resumes executing the previously paused function just below it.

---

## Deep Dive: Tracking the Stack Step-by-Step

Let's analyze a script with nested function calls to see the Call Stack in full action:

```javascript
function firstFunction() {
    console.log("Entering firstFunction");
    secondFunction();
    console.log("Exiting firstFunction");
}

function secondFunction() {
    console.log("Entering secondFunction");
    thirdFunction();
    console.log("Exiting secondFunction");
}

function thirdFunction() {
    console.log("Inside thirdFunction: Hello!");
}

// Kick off the whole process
firstFunction();
```

### Visualizing the Call Stack 

Here is how the Call Stack evolves over time during the execution of the code above:

**Step 1: Script Loads**
The Global Execution Context is created and pushed.
```text
|                  |
|       GEC        |
--------------------
```

**Step 2: `firstFunction()` is called**
```text
| firstFunction()  |
|       GEC        |
--------------------
```

**Step 3: `firstFunction` calls `secondFunction()`**
Execution of `firstFunction` is paused while `secondFunction` runs.
```text
| secondFunction() |
| firstFunction()  |
|       GEC        |
--------------------
```

**Step 4: `secondFunction` calls `thirdFunction()`**
Execution of `secondFunction` is paused while `thirdFunction` runs.
```text
| thirdFunction()  |
| secondFunction() |
| firstFunction()  |
|       GEC        |
--------------------
```

**Step 5: `thirdFunction()` completes**
It logs "Inside thirdFunction: Hello!" and is popped off the stack.
```text
| secondFunction() |
| firstFunction()  |
|       GEC        |
--------------------
```

**Step 6: `secondFunction()` completes**
It resumes, logs "Exiting secondFunction", and is popped off the stack.
```text
| firstFunction()  |
|       GEC        |
--------------------
```

**Step 7: `firstFunction()` completes**
It resumes, logs "Exiting firstFunction", and is popped off the stack. The stack is now back to just the Global Execution Context, ready for the next task!
```text
|                  |
|       GEC        |
--------------------
```

---

## Stack Overflow: Breaking the Call Stack

The Call Stack has a limited amount of memory allocation. If a function calls itself infinitely without an exit condition (base case), the stack will eventually run out of space. This triggers a fatal error known as a **Stack Overflow**.

### The Infinite Recursion Trap
```javascript
function inception() {
    // Calling itself endlessly
    inception(); 
}

inception();
```

**What happens to the stack?**
```text
|   inception()    |
|   inception()    |
|   inception()    |
|   inception()    |
|       GEC        |
--------------------
```

Eventually, the browser's JS engine intervenes and throws an error:
> `RangeError: Maximum call stack size exceeded`

---

## Important Rules to Remember

1. **Synchronous Nature:** JavaScript executes Call Stack tasks synchronously. Code execution stops and waits until the top function is popped off.
2. **Context Creation:** Every single function invocation generates a brand new Execution Context.
3. **Memory Limits:** The Call Stack is not infinite. Poorly written recursive functions will quickly crash the runtime environment.

---

## Common Interview Questions on Call Stack

**Q1: What is the Call Stack in JavaScript?**
> **A:** The Call Stack is a fundamental data structure holding Execution Contexts. It tracks function invocations and manages execution order via the LIFO (Last In, First Out) principle.

**Q2: What happens internally when a function invokes another function?**
> **A:** The calling function's execution is paused, a new Execution Context is created for the called function, and it is pushed to the top of the Call Stack. Once the called function finishes, it is popped off, and the original function resumes execution.

**Q3: What causes a Stack Overflow error?**
> **A:** A Stack Overflow occurs when the Call Stack exceeds its maximum size limit, almost always caused by an infinite loop or a recursive function without a proper base case.

**Q4: How does the Call Stack relate to Execution Contexts?**
> **A:** Execution Contexts are the actual "environments" created when code blocks run (containing variables, `this`, lexical data). The Call Stack is merely the manager that stacks these contexts together and orchestrates the order in which they resolve.

---

**Next Concept to Explore (VERY IMPORTANT):**  [Hoisting](./Hoisting.md)