# Lesson: From Python to C — Programming Logic Gates

---

## Table of Contents

1. [Lesson Overview](#lesson-overview)
2. [Part 1 — What Changed and Why](#part-1--what-changed-and-why)
3. [Part 2 — What is a Header File?](#part-2--what-is-a-header-file)
4. [Part 3 — printf Warm-Ups](#part-3--printf-warm-ups)
5. [Part 4 — Building Gates Together](#part-4--building-gates-together)
6. [Part 5 — Reference: C Operators](#part-5--reference-c-operators)
7. [Part 6 — Your Challenge](#part-6--your-challenge)
8. [Bonus: Truth Table Checker](#bonus-truth-table-checker)

---

## Lesson Overview

**Goal:** Take the logic gate functions you already wrote in Python and recreate them in C.

**What you already know:**
- What AND, OR, NOT, XOR, NOR, and XNOR gates do
- How to write a function in Python
- How to call a function and check its output

**What you will learn today:**
- Why C requires more structure than Python
- What `#include` does and why we need `<stdbool.h>`
- How to print values to the screen using `printf`
- How to write a function in C
- How to use C's symbolic operators: `!`, `&&`, `||`, `^`

**Where you will work:** [OnlineGDB](https://www.onlinegdb.com/) — set the language to **C** in the top-right dropdown.

---

## Part 1 — What Changed and Why

You already know how to write an AND gate in Python:

```python
def and_gate(x, y):
    return (x and y)
```

Here is the same gate written in C:

```c
#include <stdbool.h>

bool and_gate(bool x, bool y) {
    return x && y;
}
```

These two functions do the exact same thing. But C looks different. Let's go through every difference, one at a time.

---

### 1.1 — Python lets you write English. C uses symbols.

This is the biggest shift. Python uses reserved words that read like sentences:

| What it means | Python (English) | C (Symbolic) |
|---|---|---|
| NOT | `not x` | `!x` |
| AND | `x and y` | `x && y` |
| OR | `x or y` | `x \|\| y` |
| XOR | `x ^ y` | `x ^ y` ← same! |

> **Notice:** XOR is not in this table yet — we will come back to it. You built XOR yourself out of AND, OR, and NOT, and we are going to do the same thing in C before we introduce any new symbols.

---

### 1.2 — C requires you to declare types

In Python, you can write:

```python
x = True
```

Python figures out on its own that `x` is a boolean. C does not do this. In C, you must tell the compiler what type every variable and function is going to use:

```c
bool x = true;   // you must say "bool" before the variable name
```

This applies to functions too. In Python, `def` starts every function. In C, you write the **return type** where `def` used to be:

```python
# Python -- "def" starts the function, return type is implied
def and_gate(x, y):
    return (x and y)
```

```c
// C -- return type comes first, then function name, then parameters with their types
bool and_gate(bool x, bool y) {
    return x && y;
}
```

---

### 1.3 — Every statement ends with a semicolon

In Python, a newline ends a statement. In C, you must put a `;` at the end of every statement. If you forget it, the compiler will give you an error.

```c
return x && y;   // semicolon is required
```

---

### 1.4 — true and false are lowercase in C

Python uses `True` and `False` (capital T, capital F).  
C uses `true` and `false` (all lowercase).

```python
and_gate(True, False)   # Python
```

```c
and_gate(true, false)   // C
```

---

## Part 2 — What is a Header File?

At the very top of your C program, you will see this line:

```c
#include <stdbool.h>
```

### What does `#include` mean?

Think of `#include` like an import statement. It tells the compiler:

> "Before you compile my code, go find this file and pull its contents in."

The compiler does not know what `bool`, `true`, or `false` mean by default. Those words are defined in a file called `stdbool.h`. When you write `#include <stdbool.h>`, you are loading those definitions so your program can use them.

The angle brackets `< >` mean: "look for this file in the standard library" — a collection of official files that come with every C installation.

### What is a `.h` file?

The `.h` stands for **header**. A header file is a file that contains definitions and declarations — essentially, it tells the compiler "here are some tools you can use." You don't write header files yourself in this lesson; you just include the ones you need.

### What would happen without it?

Try removing the `#include <stdbool.h>` line and running your code. You will see an error like:

```
error: unknown type name 'bool'
```

The compiler has no idea what `bool` means until you include the file that defines it.

### The two includes you need for this lesson

```c
#include <stdbool.h>   // gives us: bool, true, false
#include <stdio.h>     // gives us: printf (for printing to the screen)
```

---

## Part 3 — printf Warm-Ups

In Python, displaying a value is simple:

```python
print(True)
print(and_gate(True, False))
```

In C, the equivalent is `printf`. It is more powerful, but more syntax to learn. Let's warm up before we write any gates.

---

### 3.1 — Printing a plain message

```c
#include <stdio.h>

int main() {
    printf("Hello, world!\n");
    return 0;
}
```

**What is `\n`?**  
It is the **newline character** — it moves the cursor to the next line, like pressing Enter. Without it, your next output would appear on the same line.

**What is `main()`?**  
Every C program must have a function called `main`. This is where the program starts running. For now, just know that your code goes inside it.

**What is `return 0;`?**  
When `main` finishes, it returns `0` to the operating system. By convention, `0` means "everything went fine." You will always write this at the end of `main`.

---

### 3.2 — Printing an integer

```c
#include <stdio.h>

int main() {
    printf("The answer is: %d\n", 42);
    return 0;
}
```

**Output:**
```
The answer is: 42
```

`printf` uses **format specifiers** — placeholder codes inside the string that get replaced by actual values:

| Format specifier | What it prints |
|---|---|
| `%d` | An integer (digit) |
| `%f` | A decimal number (float) |
| `%s` | A string of text |
| `%c` | A single character |

The value you want to print goes **after** the string, separated by a comma.

---

### 3.3 — Printing a boolean

Here is something surprising: C does not have a `%bool` format specifier. Booleans in C are just integers under the hood — `true` is stored as `1`, and `false` is stored as `0`. So you print them with `%d`:

```c
#include <stdbool.h>
#include <stdio.h>

int main() {
    bool result = true;
    printf("Result: %d\n", result);   // prints: Result: 1
    return 0;
}
```

This is actually a useful reminder: at the hardware level, `true` and `false` are just `1` and `0`. The word `bool` is a layer of meaning on top of that.

---

### 3.4 — Warm-Up Exercises

Try these in OnlineGDB before moving on. Just put them inside `main()`.

**Exercise 1:** Print the number `7` using `printf`.

**Exercise 2:** Print the result of `3 + 4` using `printf` with a `%d` format specifier.  
*(Hint: the value after the comma can be an expression, not just a variable.)*

**Exercise 3:** Print the value of `true` and the value of `false` on two separate lines.

**Exercise 4:** Print the following formatted output using two `printf` calls:
```
Input A: 1
Input B: 0
```

---

## Part 4 — Building Gates Together

Now that you can print values, let's build the first two gates together.

---

### 4.1 — The NOT Gate

**Python version:**
```python
def not_gate(x):
    return not x
```

**C version:**

```c
// NOT gate
// Takes one boolean input x.
// Returns the logical complement -- true becomes false, false becomes true.
// Operator: ! (replaces Python's "not")
bool not_gate(bool x) {
    return !x;
}
```

**Test it inside main():**

```c
int main() {
    printf("NOT(1) = %d\n", not_gate(true));    // expect: 0
    printf("NOT(0) = %d\n", not_gate(false));   // expect: 1
    return 0;
}
```

**The full program so far:**

```c
#include <stdbool.h>
#include <stdio.h>

bool not_gate(bool x) {
    return !x;
}

int main() {
    printf("NOT(1) = %d\n", not_gate(true));
    printf("NOT(0) = %d\n", not_gate(false));
    return 0;
}
```

Paste this into OnlineGDB and run it. Make sure you see:
```
NOT(1) = 0
NOT(0) = 1
```

---

### 4.2 — The AND Gate

**Python version:**
```python
def and_gate(x, y):
    return (x and y)
```

**C version:**

```c
// AND gate
// Takes two boolean inputs x and y.
// Returns true only when both inputs are true.
// Operator: && (replaces Python's "and")
bool and_gate(bool x, bool y) {
    return x && y;
}
```

**Add these test lines to main():**

```c
printf("AND(1,1) = %d\n", and_gate(true, true));    // expect: 1
printf("AND(1,0) = %d\n", and_gate(true, false));   // expect: 0
printf("AND(0,0) = %d\n", and_gate(false, false));  // expect: 0
```

Run it and verify all three outputs match the AND gate truth table.

---

### 4.3 — The Pattern

You now have the pattern. Every gate follows the same structure:

```c
bool gate_name(bool x, bool y) {
    return [expression using !, &&, ||, ^];
}
```

The only thing that changes is the expression in the return statement.

---

## Part 5 — Reference: C Operators

Use this table when building the remaining gates.

### Logical Operators

| Operator | Meaning | Example | Result |
|---|---|---|---|
| `!` | NOT | `!true` | `false` |
| `&&` | AND | `true && false` | `false` |
| `\|\|` | OR | `true \|\| false` | `true` |

### Building XOR From What You Already Know

You built XOR in Python like this:

```python
def xor_gate(x, y):
    both  = x and y
    either = x or y
    return (either and not both)
```

The logic was: **either one is true, but not both.** That same reasoning translates directly to C using the operators you already know:

```c
bool xor_gate(bool x, bool y) {
    bool both   = x && y;
    bool either = x || y;
    return (either && !both);
}
```

Every line maps exactly to your Python version — only the symbols changed.

Once you have written and tested this version, there is a shorter way to write XOR that C provides as a built-in operator:

```c
bool xor_gate(bool x, bool y) {
    return x ^ y;   // ^ means XOR -- C has a symbol for "either but not both"
}
```

Both versions are correct. The `^` version is just shorthand. It is worth knowing `^` exists because you will see it in other people's code — but understanding *why* it works is more important, and you already do.

### Compound Gates

NAND, NOR, and XNOR are all built by wrapping a simpler gate in `!( )`.

| Gate | Logic | C Expression |
|---|---|---|
| NAND | NOT of AND | `!(x && y)` |
| NOR | NOT of OR | `!(x \|\| y)` |
| XNOR | NOT of XOR | `!(x ^ y)` |

### Thinking About XNOR

XNOR returns `true` when both inputs are **the same value**. Think of it as asking: "Are x and y equal?"

You can also derive it from your XOR — just wrap it in NOT:

```c
bool xnor_gate(bool x, bool y) {
    bool both   = x && y;
    bool either = x || y;
    bool xor_result = (either && !both);
    return !xor_result;   // flip XOR to get XNOR
}
```

Or using `^` once you are comfortable with it:

```c
bool xnor_gate(bool x, bool y) {
    return !(x ^ y);
}
```

---

## Part 6 — Your Challenge

You have the pattern. You have the operator reference table. Now build the rest.

### Your Task

Complete the following program in OnlineGDB. The `not_gate` and `and_gate` are done for you. Write the remaining five gates and test each one.

```c
#include <stdbool.h>
#include <stdio.h>

// --- NOT gate (done) ---
bool not_gate(bool x) {
    return !x;
}

// --- AND gate (done) ---
bool and_gate(bool x, bool y) {
    return x && y;
}

// --- OR gate ---
// Takes two boolean inputs x and y.
// Returns true when at least one input is true.
// Operator: ||
bool or_gate(bool x, bool y) {
    // your code here
}

// --- XOR gate ---
// Takes two boolean inputs x and y.
// Returns true when inputs are different -- "either but not both."
// Hint: you built this in Python already. Translate your three lines using &&, ||, and !
bool xor_gate(bool x, bool y) {
    // your code here
}

// --- NAND gate ---
// Takes two boolean inputs x and y.
// Returns the complement of AND -- false only when both inputs are true.
// Hint: wrap the AND expression in !( )
bool nand_gate(bool x, bool y) {
    // your code here
}

// --- NOR gate ---
// Takes two boolean inputs x and y.
// Returns the complement of OR -- true only when both inputs are false.
// Hint: wrap the OR expression in !( )
bool nor_gate(bool x, bool y) {
    // your code here
}

// --- XNOR gate ---
// Takes two boolean inputs x and y.
// Returns true when both inputs are equal -- the opposite of XOR.
// Hint: build your XOR result first, then flip it with !
bool xnor_gate(bool x, bool y) {
    // your code here
}

int main() {

    // NOT
    printf("NOT(1)      = %d\n", not_gate(true));      // expect: 0

    // AND
    printf("AND(1,0)    = %d\n", and_gate(true, false));  // expect: 0
    printf("AND(1,1)    = %d\n", and_gate(true, true));   // expect: 1

    // OR
    printf("OR(1,0)     = %d\n", or_gate(true, false));   // expect: 1
    printf("OR(0,0)     = %d\n", or_gate(false, false));  // expect: 0

    // XOR
    printf("XOR(1,0)    = %d\n", xor_gate(true, false));  // expect: 1
    printf("XOR(1,1)    = %d\n", xor_gate(true, true));   // expect: 0

    // NAND
    printf("NAND(1,1)   = %d\n", nand_gate(true, true));  // expect: 0
    printf("NAND(1,0)   = %d\n", nand_gate(true, false)); // expect: 1

    // NOR
    printf("NOR(0,0)    = %d\n", nor_gate(false, false)); // expect: 1
    printf("NOR(1,0)    = %d\n", nor_gate(true, false));  // expect: 0

    // XNOR
    printf("XNOR(1,1)   = %d\n", xnor_gate(true, true));  // expect: 1
    printf("XNOR(1,0)   = %d\n", xnor_gate(true, false)); // expect: 0

    return 0;
}
```

### Expected Output

When all gates are correct, your program should print:

```
NOT(1)      = 0
AND(1,0)    = 0
AND(1,1)    = 1
OR(1,0)     = 1
OR(0,0)     = 0
XOR(1,0)    = 1
XOR(1,1)    = 0
NAND(1,1)   = 0
NAND(1,0)   = 1
NOR(0,0)    = 1
NOR(1,0)    = 0
XNOR(1,1)   = 1
XNOR(1,0)   = 0
```

---

## Bonus: Truth Table Checker

If you finish early, extend `main()` to print a **full truth table** for one gate of your choice. A full truth table tests all four possible input combinations: (0,0), (0,1), (1,0), (1,1).

Here is what the output might look like for XOR:

```
XOR Truth Table
x | y | result
0 | 0 | 0
0 | 1 | 1
1 | 0 | 1
1 | 1 | 0
```

You already know how to use `printf` with `%d`. Can you figure out how to print all four rows?

---

*Next lesson: using variables, loops, and arrays to generate truth tables automatically.*
