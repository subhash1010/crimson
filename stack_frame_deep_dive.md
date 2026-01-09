# Stack Frame Deep Dive

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
