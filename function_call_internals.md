# Function Call Internals

## 🕘 Session 1: Function Call Internals (1 hour 15 min)

### What actually happens when a function is called?

Understand these in order:

1. **Parameters passed (value copy)**
2. **Return address stored**
3. **Stack frame created**
4. **Local variables allocated**
5. **Function executes**
6. **Stack frame destroyed on return**

**Key rule:** Each function call = new stack frame.

### Important Concepts

#### Call by value (C has ONLY this)
- C always passes arguments by value.
- For primitives, the value is copied into registers or onto the stack.
- For structs, the bytes are copied.
- “Pass by reference” in C is actually pass-by-value of a pointer.

#### Why changes inside function don’t affect caller
- The callee receives its own copy of the arguments.
- Modifying that copy does not affect the caller’s original values.
- The exception is when you pass a pointer: you can change the memory it points to.

#### Role of return address
- The `call` instruction stores the return address on the stack.
- This tells the CPU where to resume execution after the function returns.

#### Stack growth direction (downwards, typically)
- On most architectures, the stack grows toward lower memory addresses.
- Allocating locals moves the stack pointer downward.

### Typical Stack Snapshot (Conceptual)

```
High addresses
| ...            |
| caller locals  |
| return address |  <-- pushed by CALL
| old RBP        |  <-- saved by callee
| locals         |  <-- allocated by callee (stack grows ↓)
Low addresses
```

---

## 🕙 Session 2: Stack Frame Deep Dive (45 minutes)

### Understand with example

```c
int f(int x) {
    int y = x + 1;
    return y;
}
```

### Know

#### Where `x` is stored
- `x` is passed by value.
- It is typically stored in a **register** when the function begins.
- If the compiler needs it later or runs out of registers, it may be **spilled** into the stack frame.

#### Where `y` is stored
- `y` is a local variable, so it lives in the **stack frame** of `f`.
- The stack space for `y` is reserved when the frame is created.

#### When memory is freed
- When the function returns, its **stack frame is destroyed**.
- The stack pointer is restored, so the memory is reclaimed for future calls.

#### Why accessing local variables after return is UB
- After return, the stack memory that held `x`/`y` can be **reused**.
- The old values are no longer valid and may be overwritten.
- Accessing them is **undefined behavior**.

### ISRO frequently asks

**Question:** “What happens to local variables after function returns?”

**Correct answer:**
- **Memory reclaimed** (stack frame removed).
- **Value becomes garbage**.
- Any **access → undefined behavior**.
