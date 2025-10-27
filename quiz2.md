# Abstract Syntax Trees (AST) - Complete Learning Guide

## Table of Contents
1. [Introduction](#introduction)
2. [Core Concepts](#core-concepts)
3. [The Two Golden Rules](#the-two-golden-rules)
4. [Step-by-Step Method](#step-by-step-method)
5. [Practice Examples by Difficulty](#practice-examples-by-difficulty)
6. [Common Mistakes](#common-mistakes)
7. [Tips and Tricks](#tips-and-tricks)
8. [Quick Reference](#quick-reference)

---

## Introduction

### What is an Abstract Syntax Tree?

An **Abstract Syntax Tree (AST)** is a tree representation of the structure of source code or mathematical expressions. Each node in the tree represents an operation or value, and the tree structure shows the order in which operations should be performed.

Think of it like this: when you write `2 + 3 * 4`, your brain automatically knows to do the multiplication first, then the addition. An AST is a visual way to represent this understanding. The tree shows:
- **What operations** need to be performed (the nodes)
- **In what order** they should be performed (the tree structure)
- **What values** are involved (the leaves)

### Why Are ASTs Important?

ASTs are fundamental in:
- **Compilers**: Converting source code to machine code
- **Interpreters**: Executing code
- **Code analysis tools**: Understanding program structure
- **Programming language design**: Defining how expressions are evaluated

When you're writing a parser, interpreter, or compiler, you need to convert textual expressions into ASTs so the computer can understand the intended order of operations.

### How to Read an AST

ASTs are evaluated **bottom-up** (from leaves to root):
1. Start at the **leaves** (the numbers/variables at the bottom)
2. Move **upward** through the tree
3. The **root** (top node) is the **last operation** performed

Example:
```
       +          ← Step 3: Add 5 + 6 = 11
      / \
     5   *        ← Step 2: Get result from multiplication
        / \
       2   3      ← Step 1: Multiply 2 * 3 = 6
```

---

## Core Concepts

### Operator Precedence

**Precedence** determines which operations are performed first when there are no parentheses. It's like the "order of operations" you learned in math class (PEMDAS/BODMAS).

**In our examples:**
- **High precedence** (performed first): `*` (multiplication), `/` (division)
- **Low precedence** (performed last): `+` (addition), `-` (subtraction)

**Key insight**: Higher precedence operators go **deeper** in the tree (they're evaluated first).

### Operator Associativity

**Associativity** determines how operators of the **same precedence** are grouped when they appear together.

**Left-associative** (what we use):
- `A + B + C` is evaluated as `(A + B) + C`
- We process from left to right
- The **rightmost** operator of the same precedence becomes the root

**Example of left-associativity:**
```
Expression: 2 + 3 + 4
Grouping:   (2 + 3) + 4
            
Tree:       +          ← The rightmost + is the root
           / \
          +   4        ← The leftmost + is deeper
         / \
        2   3
```

### Tree Terminology

- **Root**: The top node (last operation performed)
- **Leaves**: Bottom nodes with no children (numbers or variables)
- **Subtree**: A portion of the tree under a particular node
- **Left/Right child**: The nodes directly connected below a parent node
- **Depth/Level**: How far down a node is from the root

---

## The Two Golden Rules

### Rule 1: PRECEDENCE (Depth in Tree)

**Higher precedence = Deeper in tree = Happens FIRST**

Think of it as:
- `*` and `/` **dig deep** → go to bottom of tree → executed first
- `+` and `-` **stay high** → go to top of tree → executed last

When you have different operators, the one with **lower precedence** becomes the root.

**Example:**
```
5 + 3 * 2

The * has higher precedence, so it goes deeper:
       +          ← Low precedence = root
      / \
     5   *        ← High precedence = deeper
        / \
       3   2
```

### Rule 2: ASSOCIATIVITY (Left vs Right)

**Left-associative: Rightmost operator of same precedence becomes the root**

When you have multiple operators of the **same precedence**, the **rightmost** one becomes the root (because they're left-associative).

**Example:**
```
2 + 3 + 4

Both are +, so pick the rightmost:
       +          ← The second + (rightmost) is root
      / \
     +   4        ← The first + is deeper
    / \
   2   3
```

**Why?** Because left-associativity means we group from left: `(2 + 3) + 4`
- The operation `(2 + 3)` happens FIRST → goes DEEPER
- The operation `result + 4` happens LAST → goes at ROOT

---

## Step-by-Step Method

Follow this process for **every** expression:

### STEP 1: Find the ROOT (What happens LAST?)

1. Look for the **lowest precedence** operator in the expression
2. If there are **multiple** operators with the same low precedence, pick the **rightmost** one (left-associative)
3. This operator becomes your **ROOT**

**Tips:**
- `+` and `-` have lower precedence than `*` and `/`
- When in doubt, ask: "What's the LAST operation I would perform?"
- The root is always at the TOP of your tree

### STEP 2: Split the Expression at the ROOT

Once you've identified the root, split the expression:

```
[LEFT PART]  ROOT  [RIGHT PART]
     ↓        ↓         ↓
  left      ROOT     right
  child              child
```

**CRITICAL:** Whatever is physically on the **LEFT** of the operator goes to the **left** subtree, and whatever is on the **RIGHT** goes to the **right** subtree. **Never swap them!**

### STEP 3: Repeat for Each Subtree

Now process the left and right parts independently:
- Apply STEP 1 to each part to find its root
- Apply STEP 2 to split it further
- Continue until you reach single numbers or variables

### Example Walkthrough: `2 + 3 * 4 + 5`

**STEP 1:** Find the root
- Operators: two `+` (low precedence), one `*` (high precedence)
- Lowest precedence = `+`
- Multiple `+` operators → pick rightmost
- **Root = second `+`** (between 4 and 5)

**STEP 2:** Split at root
```
(2 + 3 * 4)  +  5
     ↓       ↓   ↓
   LEFT    ROOT RIGHT
```

Current tree:
```
         +
        / \
  [2+3*4]  5
```

**STEP 3:** Process left subtree `2 + 3 * 4`
- Operators: `+` (low), `*` (high)
- Lowest precedence = `+`
- **Root = `+`**

Split:
```
2  +  (3 * 4)
↓  ↓     ↓
LEFT ROOT RIGHT
```

Current tree:
```
         +
        / \
       +   5
      / \
     2  [3*4]
```

**STEP 4:** Process `3 * 4`
- Only one operator: `*`
- **Root = `*`**

Split:
```
3  *  4
↓  ↓  ↓
LEFT ROOT RIGHT
```

**FINAL TREE:**
```
         +          ← Happens 3rd (LAST)
        / \
       +   5        ← Happens 2nd
      / \
     2   *          ← Happens 1st (FIRST)
        / \
       3   4
```

**Evaluation:**
1. `3 * 4 = 12`
2. `2 + 12 = 14`
3. `14 + 5 = 19` ✓

---

## Practice Examples by Difficulty

### DIFFICULTY LEVEL 1: Single Operator (Easiest)
*Master these first - they're the building blocks!*

These are the simplest possible expressions. No precedence rules, no associativity needed. Just one operation.

#### Example 1.1: `5 + 3`
```
    +
   / \
  5   3
```
**Result:** 8

#### Example 1.2: `5 * 3`
```
    *
   / \
  5   3
```
**Result:** 15

#### Example 1.3: `10 - 7`
```
    -
   / \
  10  7
```
**Result:** 3

#### Example 1.4: `8 / 4`
```
    /
   / \
  8   4
```
**Result:** 2

#### Example 1.5: `6 + 2`
```
    +
   / \
  6   2
```
**Result:** 8

**Key Learning:** With only one operator, that operator is always the root. The left operand goes left, right operand goes right.

---

### DIFFICULTY LEVEL 2: Two Operators - Same Precedence
*Learn left-associativity here*

These examples teach you how to handle multiple operators of the same type. The key concept is **left-associativity**: the rightmost operator becomes the root.

#### Example 2.1: `2 + 3 + 4`
```
       +          ← Second + (RIGHTMOST) is root
      / \
     +   4        ← First + is deeper (happens first)
    / \
   2   3
```
**Grouping:** `(2 + 3) + 4`  
**Evaluation:**
- `2 + 3 = 5`
- `5 + 4 = 9`  
**Result:** 9

#### Example 2.2: `2 * 3 * 4`
```
       *          ← Second * (RIGHTMOST) is root
      / \
     *   4        ← First * is deeper
    / \
   2   3
```
**Grouping:** `(2 * 3) * 4`  
**Evaluation:**
- `2 * 3 = 6`
- `6 * 4 = 24`  
**Result:** 24

#### Example 2.3: `10 - 5 - 2`
```
       -          ← Second - (RIGHTMOST) is root
      / \
     -   2        ← First - is deeper
    / \
   10  5
```
**Grouping:** `(10 - 5) - 2`  
**Evaluation:**
- `10 - 5 = 5`
- `5 - 2 = 3`  
**Result:** 3

**Important:** Not `10 - (5 - 2) = 10 - 3 = 7`!

#### Example 2.4: `1 + 2 + 3 + 4`
```
           +          ← Third + (RIGHTMOST) is root
          / \
         +   4        ← Second +
        / \
       +   3          ← First +
      / \
     1   2
```
**Grouping:** `((1 + 2) + 3) + 4`  
**Evaluation:**
- `1 + 2 = 3`
- `3 + 3 = 6`
- `6 + 4 = 10`  
**Result:** 10

#### Example 2.5: `20 / 4 / 2`
```
       /          ← Second / (RIGHTMOST) is root
      / \
     /   2        ← First / is deeper
    / \
   20  4
```
**Grouping:** `(20 / 4) / 2`  
**Evaluation:**
- `20 / 4 = 5`
- `5 / 2 = 2.5`  
**Result:** 2.5

**Important:** Not `20 / (4 / 2) = 20 / 2 = 10`!

**Key Learning:** When you have the same operator multiple times, the rightmost one is the root. The tree "chains" to the left, with the leftmost operation at the bottom (executed first).

---

### DIFFICULTY LEVEL 3: Two Operators - Different Precedence
*Learn precedence rules here - this is crucial!*

These examples teach you the most important concept: **operators with lower precedence become the root**. The high-precedence operators (multiplication/division) go deeper in the tree because they're evaluated first.

#### Example 3.1: `1 + 2 * 3`
```
       +          ← Addition is root (LOW precedence)
      / \
     1   *        ← Multiplication is deeper (HIGH precedence)
        / \
       2   3
```
**Why:** `*` has higher precedence, so we do it first, but `+` is the root because it's performed last.  
**Grouping:** `1 + (2 * 3)`  
**Evaluation:**
- `2 * 3 = 6`
- `1 + 6 = 7`  
**Result:** 7

#### Example 3.2: `5 + 3 * 2`
```
       +          ← Addition is root
      / \
     5   *        ← Multiplication on RIGHT side
        / \
       3   2
```
**Grouping:** `5 + (3 * 2)`  
**Evaluation:**
- `3 * 2 = 6`
- `5 + 6 = 11`  
**Result:** 11

#### Example 3.3: `1 * 2 + 3`
```
       +          ← Addition is root
      / \
     *   3        ← Multiplication on LEFT side
    / \
   1   2
```
**Why:** Even though multiplication comes first in the expression, addition is still the root because it has lower precedence.  
**Grouping:** `(1 * 2) + 3`  
**Evaluation:**
- `1 * 2 = 2`
- `2 + 3 = 5`  
**Result:** 5

#### Example 3.4: `5 * 3 + 2`
```
       +          ← Addition is root
      / \
     *   2        ← Multiplication on LEFT side
    / \
   5   3
```
**Grouping:** `(5 * 3) + 2`  
**Evaluation:**
- `5 * 3 = 15`
- `15 + 2 = 17`  
**Result:** 17

#### Example 3.5: `10 - 2 * 3`
```
       -          ← Subtraction is root (LOW precedence)
      / \
     10  *        ← Multiplication on RIGHT side
        / \
       2   3
```
**Grouping:** `10 - (2 * 3)`  
**Evaluation:**
- `2 * 3 = 6`
- `10 - 6 = 4`  
**Result:** 4

#### Example 3.6: `4 * 5 - 6`
```
       -          ← Subtraction is root
      / \
     *   6        ← Multiplication on LEFT side
    / \
   4   5
```
**Grouping:** `(4 * 5) - 6`  
**Evaluation:**
- `4 * 5 = 20`
- `20 - 6 = 14`  
**Result:** 14

#### Example 3.7: `12 / 3 + 2`
```
       +          ← Addition is root
      / \
     /   2        ← Division on LEFT side
    / \
   12  3
```
**Grouping:** `(12 / 3) + 2`  
**Evaluation:**
- `12 / 3 = 4`
- `4 + 2 = 6`  
**Result:** 6

#### Example 3.8: `3 + 15 / 5`
```
       +          ← Addition is root
      / \
     3   /        ← Division on RIGHT side
        / \
       15  5
```
**Grouping:** `3 + (15 / 5)`  
**Evaluation:**
- `15 / 5 = 3`
- `3 + 3 = 6`  
**Result:** 6

#### Example 3.9: `7 - 12 / 4`
```
       -          ← Subtraction is root
      / \
     7   /        ← Division on RIGHT side
        / \
       12  4
```
**Grouping:** `7 - (12 / 4)`  
**Evaluation:**
- `12 / 4 = 3`
- `7 - 3 = 4`  
**Result:** 4

#### Example 3.10: `18 / 6 - 1`
```
       -          ← Subtraction is root
      / \
     /   1        ← Division on LEFT side
    / \
   18  6
```
**Grouping:** `(18 / 6) - 1`  
**Evaluation:**
- `18 / 6 = 3`
- `3 - 1 = 2`  
**Result:** 2

**Key Learning:** 
- **The operator with LOWER precedence is ALWAYS the root**, regardless of position
- Multiplication/division on the LEFT means they happened before the expression continues
- Multiplication/division on the RIGHT means they happen in the middle of the expression
- The tree structure reflects WHERE the high-precedence operation appears in the original expression

---

### DIFFICULTY LEVEL 4: Two Operators - With Subtraction/Division
*Master order dependency - it matters!*

These examples are similar to Level 3, but use subtraction and division where **order matters critically**. Unlike addition and multiplication (which are commutative), subtraction and division give different results if you swap the operands.

#### Example 4.1: `8 - 2 * 3`
```
       -          ← Subtraction is root
      / \
     8   *        ← Multiplication on RIGHT
        / \
       2   3
```
**Grouping:** `8 - (2 * 3)`  
**Evaluation:**
- `2 * 3 = 6`
- `8 - 6 = 2`  
**Result:** 2

**Common mistake:** If you put the `*` on the left side, you'd compute `(2 * 3) - 8 = 6 - 8 = -2` ❌

#### Example 4.2: `8 * 2 - 3`
```
       -          ← Subtraction is root
      / \
     *   3        ← Multiplication on LEFT
    / \
   8   2
```
**Grouping:** `(8 * 2) - 3`  
**Evaluation:**
- `8 * 2 = 16`
- `16 - 3 = 13`  
**Result:** 13

#### Example 4.3: `6 / 2 + 1`
```
       +          ← Addition is root
      / \
     /   1        ← Division on LEFT
    / \
   6   2
```
**Grouping:** `(6 / 2) + 1`  
**Evaluation:**
- `6 / 2 = 3`
- `3 + 1 = 4`  
**Result:** 4

#### Example 4.4: `10 / 5 - 1`
```
       -          ← Subtraction is root
      / \
     /   1        ← Division on LEFT
    / \
   10  5
```
**Grouping:** `(10 / 5) - 1`  
**Evaluation:**
- `10 / 5 = 2`
- `2 - 1 = 1`  
**Result:** 1

**Not:** `10 / (5 - 1) = 10 / 4 = 2.5` ❌

#### Example 4.5: `3 - 12 / 4`
```
       -          ← Subtraction is root
      / \
     3   /        ← Division on RIGHT
        / \
       12  4
```
**Grouping:** `3 - (12 / 4)`  
**Evaluation:**
- `12 / 4 = 3`
- `3 - 3 = 0`  
**Result:** 0

#### Example 4.6: `20 / 4 + 5`
```
       +          ← Addition is root
      / \
     /   5        ← Division on LEFT
    / \
   20  4
```
**Grouping:** `(20 / 4) + 5`  
**Evaluation:**
- `20 / 4 = 5`
- `5 + 5 = 10`  
**Result:** 10

#### Example 4.7: `15 - 3 * 2`
```
       -          ← Subtraction is root
      / \
     15  *        ← Multiplication on RIGHT
        / \
       3   2
```
**Grouping:** `15 - (3 * 2)`  
**Evaluation:**
- `3 * 2 = 6`
- `15 - 6 = 9`  
**Result:** 9

#### Example 4.8: `4 * 3 - 5`
```
       -          ← Subtraction is root
      / \
     *   5        ← Multiplication on LEFT
    / \
   4   3
```
**Grouping:** `(4 * 3) - 5`  
**Evaluation:**
- `4 * 3 = 12`
- `12 - 5 = 7`  
**Result:** 7

**Key Learning:**
- With subtraction and division, **left vs right placement is critical**
- `A - B` is very different from `B - A`
- `A / B` is very different from `B / A`
- Always maintain the left-to-right order from the original expression
- **Never swap the left and right subtrees** - you'll get the wrong answer!

---

### DIFFICULTY LEVEL 5: Three+ Operators - Mixed Precedence
*Combine everything you've learned*

Now we're dealing with multiple operators at different precedence levels. You need to apply both precedence rules AND associativity rules. These require processing the tree in multiple stages.

#### Example 5.1: `9 + 2 * 4 + 1`
```
        +          ← Rightmost + is root (happens 3rd/LAST)
       / \
      +   1        ← First + (happens 2nd)
     / \
    9   *          ← Multiplication (happens 1st/FIRST)
       / \
      2   4
```
**Step-by-step:**
1. Find root: Two `+` (low) and one `*` (high) → pick rightmost `+`
2. Split: `(9 + 2 * 4) + 1`
3. Process left side `9 + 2 * 4`: `+` vs `*` → `+` is root → split: `9 + (2 * 4)`
4. Process `2 * 4`: only operator, so it's the root

**Evaluation:**
- `2 * 4 = 8`
- `9 + 8 = 17`
- `17 + 1 = 18`  
**Result:** 18

#### Example 5.2: `4 + 2 * 3 * 7`
```
        +          ← Addition is root (happens 3rd/LAST)
       / \
      4   *        ← Rightmost * (happens 2nd)
         / \
        *   7      ← Leftmost * (happens 1st/FIRST)
       / \
      2   3
```
**Step-by-step:**
1. Find root: One `+` (low), two `*` (high) → `+` is root
2. Split: `4 + (2 * 3 * 7)`
3. Process right side `2 * 3 * 7`: Two `*` → pick rightmost → split: `(2 * 3) * 7`
4. Process `2 * 3`: only operator

**Evaluation:**
- `2 * 3 = 6`
- `6 * 7 = 42`
- `4 + 42 = 46`  
**Result:** 46

#### Example 5.3: `4 * 2 * 3 + 7`
```
        +          ← Addition is root (happens 3rd/LAST)
       / \
      *   7        ← Rightmost * (happens 2nd)
     / \
    *   3          ← Leftmost * (happens 1st/FIRST)
   / \
  4   2
```
**Step-by-step:**
1. Find root: One `+` (low), two `*` (high) → `+` is root
2. Split: `(4 * 2 * 3) + 7`
3. Process left side `4 * 2 * 3`: Two `*` → pick rightmost → split: `(4 * 2) * 3`
4. Process `4 * 2`: only operator

**Evaluation:**
- `4 * 2 = 8`
- `8 * 3 = 24`
- `24 + 7 = 31`  
**Result:** 31

#### Example 5.4: `2 + 3 * 4 + 5`
```
         +          ← Rightmost + is root (happens 3rd/LAST)
        / \
       +   5        ← First + (happens 2nd)
      / \
     2   *          ← Multiplication (happens 1st/FIRST)
        / \
       3   4
```
**Step-by-step:**
1. Find root: Two `+` (low), one `*` (high) → pick rightmost `+`
2. Split: `(2 + 3 * 4) + 5`
3. Process left side `2 + 3 * 4`: `+` vs `*` → `+` is root → split: `2 + (3 * 4)`
4. Process `3 * 4`: only operator

**Evaluation:**
- `3 * 4 = 12`
- `2 + 12 = 14`
- `14 + 5 = 19`  
**Result:** 19

#### Example 5.5: `1 + 2 + 3 * 4`
```
         +          ← Rightmost + is root (happens 3rd/LAST)
        / \
       +   *        ← First + (happens 2nd), Multiplication (also happens 2nd)
      / \ / \
     1  2 3  4
```
**Step-by-step:**
1. Find root: Two `+` (low), one `*` (high) → pick rightmost `+`
2. Split: `(1 + 2) + (3 * 4)`
3. Process both subtrees independently

**Evaluation:**
- `1 + 2 = 3`
- `3 * 4 = 12`
- `3 + 12 = 15`  
**Result:** 15

#### Example 5.6: `10 - 2 * 3 + 4`
```
         +          ← Addition is root (happens 3rd/LAST)
        / \
       -   4        ← Subtraction (happens 2nd)
      / \
     10  *          ← Multiplication (happens 1st/FIRST)
        / \
       2   3
```
**Step-by-step:**
1. Find root: One `-`, one `+` (both low), one `*` (high) → pick rightmost low-precedence → `+`
2. Split: `(10 - 2 * 3) + 4`
3. Process left side `10 - 2 * 3`: `-` vs `*` → `-` is root → split: `10 - (2 * 3)`

**Evaluation:**
- `2 * 3 = 6`
- `10 - 6 = 4`
- `4 + 4 = 8`  
**Result:** 8

#### Example 5.7: `5 * 2 + 3 * 4`
```
         +          ← Addition is root (happens 3rd/LAST)
        / \
       *   *        ← Both multiplications (happen 1st and 2nd)
      / \ / \
     5  2 3  4
```
**Step-by-step:**
1. Find root: One `+` (low), two `*` (high) → `+` is root
2. Split: `(5 * 2) + (3 * 4)`
3. Process both subtrees independently

**Evaluation:**
- `5 * 2 = 10`
- `3 * 4 = 12`
- `10 + 12 = 22`  
**Result:** 22

#### Example 5.8: `8 / 2 + 3 * 2`
```
         +          ← Addition is root (happens 3rd/LAST)
        / \
       /   *        ← Division and multiplication (happen 1st and 2nd)
      / \ / \
     8  2 3  2
```
**Step-by-step:**
1. Find root: One `+` (low), one `/` and one `*` (both high) → `+` is root
2. Split: `(8 / 2) + (3 * 2)`
3. Process both subtrees

**Evaluation:**
- `8 / 2 = 4`
- `3 * 2 = 6`
- `4 + 6 = 10`  
**Result:** 10

#### Example 5.9: `12 - 4 / 2 + 1`
```
         +          ← Addition is root (happens 3rd/LAST)
        / \
       -   1        ← Subtraction (happens 2nd)
      / \
     12  /          ← Division (happens 1st/FIRST)
        / \
       4   2
```
**Step-by-step:**
1. Find root: One `-`, one `+` (both low), one `/` (high) → pick rightmost low → `+`
2. Split: `(12 - 4 / 2) + 1`
3. Process left side `12 - 4 / 2`: `-` vs `/` → `-` is root → split: `12 - (4 / 2)`

**Evaluation:**
- `4 / 2 = 2`
- `12 - 2 = 10`
- `10 + 1 = 11`  
**Result:** 11

#### Example 5.10: `3 + 4 + 5 * 6`
```
         +          ← Rightmost + is root (happens 3rd/LAST)
        / \
       +   *        ← First + and multiplication (happen 1st and 2nd)
      / \ / \
     3  4 5  6
```
**Step-by-step:**
1. Find root: Two `+` (low), one `*` (high) → pick rightmost `+`
2. Split: `(3 + 4) + (5 * 6)`
3. Process both subtrees

**Evaluation:**
- `3 + 4 = 7`
- `5 * 6 = 30`
- `7 + 30 = 37`  
**Result:** 37

**Key Learning:**
- With three or more operators, you need to **work in stages**
- Always find the **rightmost** operator with the **lowest precedence** first
- Process each subtree independently using the same rules
- High-precedence operators (multiplication/division) always end up deeper, even if there are multiple of them
- The tree can have multiple "levels" of operations

---

### DIFFICULTY LEVEL 6: Complex Expressions with Subtraction/Division
*The most challenging - everything combined*

These are the hardest because they combine:
- Multiple operators at different precedence levels
- Subtraction and/or division (where order is critical)
- Associativity rules
- Negative results (easy to mess up)

#### Example 6.1: `2 - 4 - 1 * 3`
```
         -          ← Rightmost - is root (happens 3rd/LAST)
        / \
       -   *        ← First - and multiplication (happen 1st and 2nd)
      / \ / \
     2  4 1  3
```
**This is TRICKY!**

**Step-by-step:**
1. Find root: Two `-` (low), one `*` (high) → pick rightmost `-`
2. Split: `(2 - 4) - (1 * 3)`
3. Process both subtrees

**Evaluation:**
- `2 - 4 = -2` (negative!)
- `1 * 3 = 3`
- `(-2) - 3 = -5`  
**Result:** -5

**Common mistake:** Picking the first `-` as root would give: `2 - (4 - 1 * 3) = 2 - (4 - 3) = 2 - 1 = 1` ❌

#### Example 6.2: `10 - 6 / 2 - 1`
```
         -          ← Rightmost - is root (happens 3rd/LAST)
        / \
       -   1        ← First - (happens 2nd)
      / \
     10  /          ← Division (happens 1st/FIRST)
        / \
       6   2
```
**Step-by-step:**
1. Find root: Two `-` (low), one `/` (high) → pick rightmost `-`
2. Split: `(10 - 6 / 2) - 1`
3. Process left side `10 - 6 / 2`: `-` vs `/` → `-` is root → split: `10 - (6 / 2)`

**Evaluation:**
- `6 / 2 = 3`
- `10 - 3 = 7`
- `7 - 1 = 6`  
**Result:** 6

#### Example 6.3: `8 / 4 / 2 + 1`
```
         +          ← Addition is root (happens 3rd/LAST)
        / \
       /   1        ← Rightmost / (happens 2nd)
      / \
     /   2          ← Leftmost / (happens 1st/FIRST)
    / \
   8   4
```
**Step-by-step:**
1. Find root: One `+` (low), two `/` (high) → `+` is root
2. Split: `(8 / 4 / 2) + 1`
3. Process left side `8 / 4 / 2`: Two `/` → pick rightmost → split: `(8 / 4) / 2`

**Evaluation:**
- `8 / 4 = 2`
- `2 / 2 = 1`
- `1 + 1 = 2`  
**Result:** 2

**Not:** `8 / (4 / 2) = 8 / 2 = 4` then `4 + 1 = 5` ❌

#### Example 6.4: `15 - 3 - 2 * 4`
```
         -          ← Rightmost - is root (happens 3rd/LAST)
        / \
       -   *        ← First - and multiplication (happen 1st and 2nd)
      / \ / \
     15 3 2  4
```
**Step-by-step:**
1. Find root: Two `-` (low), one `*` (high) → pick rightmost `-`
2. Split: `(15 - 3) - (2 * 4)`

**Evaluation:**
- `15 - 3 = 12`
- `2 * 4 = 8`
- `12 - 8 = 4`  
**Result:** 4

#### Example 6.5: `20 / 5 - 2 * 2`
```
         -          ← Subtraction is root (happens 3rd/LAST)
        / \
       /   *        ← Division and multiplication (happen 1st and 2nd)
      / \ / \
     20 5 2  2
```
**Step-by-step:**
1. Find root: One `-` (low), one `/` and one `*` (both high) → `-` is root
2. Split: `(20 / 5) - (2 * 2)`

**Evaluation:**
- `20 / 5 = 4`
- `2 * 2 = 4`
- `4 - 4 = 0`  
**Result:** 0

#### Example 6.6: `9 - 3 * 2 + 5`
```
         +          ← Addition is root (happens 3rd/LAST)
        / \
       -   5        ← Subtraction (happens 2nd)
      / \
     9   *          ← Multiplication (happens 1st/FIRST)
        / \
       3   2
```
**Step-by-step:**
1. Find root: One `-`, one `+` (both low), one `*` (high) → pick rightmost low → `+`
2. Split: `(9 - 3 * 2) + 5`
3. Process left side `9 - 3 * 2`: `-` vs `*` → `-` is root → split: `9 - (3 * 2)`

**Evaluation:**
- `3 * 2 = 6`
- `9 - 6 = 3`
- `3 + 5 = 8`  
**Result:** 8

#### Example 6.7: `12 / 3 - 4 / 2`
```
         -          ← Subtraction is root (happens 3rd/LAST)
        / \
       /   /        ← Both divisions (happen 1st and 2nd)
      / \ / \
     12 3 4  2
```
**Step-by-step:**
1. Find root: One `-` (low), two `/` (high) → `-` is root
2. Split: `(12 / 3) - (4 / 2)`

**Evaluation:**
- `12 / 3 = 4`
- `4 / 2 = 2`
- `4 - 2 = 2`  
**Result:** 2

#### Example 6.8: `16 - 8 / 4 + 2`
```
         +          ← Addition is root (happens 3rd/LAST)
        / \
       -   2        ← Subtraction (happens 2nd)
      / \
     16  /          ← Division (happens 1st/FIRST)
        / \
       8   4
```
**Step-by-step:**
1. Find root: One `-`, one `+` (both low), one `/` (high) → pick rightmost low → `+`
2. Split: `(16 - 8 / 4) + 2`
3. Process left side: split at `-` → `16 - (8 / 4)`

**Evaluation:**
- `8 / 4 = 2`
- `16 - 2 = 14`
- `14 + 2 = 16`  
**Result:** 16

#### Example 6.9: `5 - 2 - 1 - 1`
```
           -          ← Rightmost - (happens 4th/LAST)
          / \
         -   1        ← Third - (happens 3rd)
        / \
       -   1          ← Second - (happens 2nd)
      / \
     5   2            ← (happens 1st/FIRST)
```
**Grouping:** `((5 - 2) - 1) - 1`

**Evaluation:**
- `5 - 2 = 3`
- `3 - 1 = 2`
- `2 - 1 = 1`  
**Result:** 1

**Not:** `5 - (2 - (1 - 1)) = 5 - (2 - 0) = 5 - 2 = 3` ❌

#### Example 6.10: `24 / 6 / 2 - 1`
```
         -          ← Subtraction is root (happens 3rd/LAST)
        / \
       /   1        ← Rightmost / (happens 2nd)
      / \
     /   2          ← Leftmost / (happens 1st/FIRST)
    / \
   24  6
```
**Grouping:** `((24 / 6) / 2) - 1`

**Evaluation:**
- `24 / 6 = 4`
- `4 / 2 = 2`
- `2 - 1 = 1`  
**Result:** 1

**Key Learning:**
- These are the HARDEST because you need everything:
  - Correct precedence identification
  - Correct associativity (rightmost low-precedence operator)
  - Careful attention to left/right order (can't swap!)
  - Handling negative results
- Common mistakes:
  - Picking the wrong operator as root
  - Swapping left and right subtrees
  - Confusing the order of operations
- **Master the easier levels first** before attempting these!

---

## Common Mistakes

### Mistake 1: Picking the Wrong Root

**Wrong:** Picking a high-precedence operator as the root

**Example:** For `2 + 3 * 4`, picking `*` as root:
```
    *          ← WRONG! High precedence shouldn't be root
   / \
  +   4
 / \
2   3
```
This would mean: `(2 + 3) * 4 = 5 * 4 = 20` ❌

**Correct:** Pick the `+` as root:
```
    +          ← CORRECT! Low precedence is root
   / \
  2   *
     / \
    3   4
```
This means: `2 + (3 * 4) = 2 + 12 = 14` ✓

**How to avoid:** Always look for the **lowest precedence** operator first!

---

### Mistake 2: Swapping Left and Right Subtrees

**Wrong:** Putting the right operand on the left side

**Example:** For `10 - 3`, swapping them:
```
    -          ← The operator is correct
   / \
  3   10      ← WRONG! They're swapped!
```
This would compute: `3 - 10 = -7` ❌

**Correct:**
```
    -
   / \
  10  3       ← CORRECT! Maintains order
```
This computes: `10 - 3 = 7` ✓

**How to avoid:** Always maintain the **left-to-right order** from the original expression. What's on the left goes left, what's on the right goes right!

---

### Mistake 3: Wrong Associativity Choice

**Wrong:** Picking the leftmost operator of the same precedence as root

**Example:** For `2 + 3 + 4`, picking the first `+`:
```
    +          ← WRONG! Picked leftmost +
   / \
  2   +
     / \
    3   4
```
This would represent: `2 + (3 + 4)` which equals 9, but the grouping is wrong!

**Correct:** Pick the **rightmost** `+`:
```
       +       ← CORRECT! Picked rightmost +
      / \
     +   4
    / \
   2   3
```
This represents: `(2 + 3) + 4` ✓

**How to avoid:** With left-associativity, always pick the **rightmost** operator of the same precedence level!

---

### Mistake 4: Ignoring Precedence in Complex Expressions

**Wrong:** Treating all operators as equal

**Example:** For `2 + 3 * 4 + 5`, picking the middle `*`:
```
         *          ← WRONG! High precedence shouldn't be root
        / \
       +   +
      / \ / \
     2  3 4  5
```

**Correct:** Pick the rightmost `+`:
```
         +          ← CORRECT!
        / \
       +   5
      / \
     2   *
        / \
       3   4
```

**How to avoid:** Always identify **all** operators and their precedences before starting!

---

### Mistake 5: Confusing Evaluation Order with Tree Structure

**Confusion:** "Multiplication happens first, so it should be at the top"

**Wrong thinking:** 
- "In `2 + 3 * 4`, I do multiplication first, so `*` should be the root"

**Correct thinking:**
- "Operations performed FIRST go DEEPER in the tree"
- "Operations performed LAST are at the ROOT"
- "The root is the LAST operation, so low-precedence operators are roots"

**Remember:** The tree is built **bottom-up** for evaluation. The **deepest** operations are performed **first**.

---

## Tips and Tricks

### Tip 1: The "Last Operation" Trick

Always ask yourself: **"What is the LAST operation I would perform when calculating this by hand?"**

That operation is your **root**.

**Example:** `5 + 3 * 2`
- By hand: "First I'd do 3 * 2 = 6, then I'd add 5 to get 11"
- The **last** thing I do is the addition
- So `+` is the root!

---

### Tip 2: The "Dividing Line" Visualization

Imagine the root operator is a **vertical line** dividing the expression:

```
For: 4 + 2 * 3 * 7

Think:  4  |+|  2 * 3 * 7
       LEFT|+| RIGHT
```

Everything on the **left of the line** goes to the **left subtree**.  
Everything on the **right of the line** goes to the **right subtree**.

---

### Tip 3: Work in Stages for Complex Expressions

Don't try to build the whole tree at once!

**Process:**
1. Find the root (rightmost, lowest precedence)
2. Draw the root with two empty subtrees
3. Fill in one subtree at a time
4. For each subtree, repeat steps 1-3

**Example:** `2 + 3 * 4 + 5`

Stage 1:
```
    ?
```

Stage 2: (Find root = rightmost `+`)
```
         +
        / \
    [2+3*4] 5
```

Stage 3: (Process left subtree)
```
         +
        / \
       +   5
      / \
     2  [3*4]
```

Stage 4: (Process remaining subtree)
```
         +
        / \
       +   5
      / \
     2   *
        / \
       3   4
```

---

### Tip 4: Verify with Evaluation

After building your tree, **evaluate it** to check:

1. Start at the leaves (bottom numbers)
2. Work your way up
3. Compare with the original expression

If you get a different result, you made a mistake in the tree!

---

### Tip 5: Practice with Parentheses

If you're unsure about your tree, try adding parentheses to the original expression based on your tree structure:

**Tree:**
```
    +
   / \
  2   *
     / \
    3   4
```

**This represents:** `2 + (3 * 4)`

Now evaluate both:
- Tree: `3 * 4 = 12`, then `2 + 12 = 14`
- Expression: `2 + (3 * 4) = 2 + 12 = 14`

They match! ✓

---

### Tip 6: Draw It Out

Don't try to do this all in your head!

**Use paper:**
1. Write the expression
2. Circle the operators and label their precedence
3. Find and mark the root
4. Draw lines showing the split
5. Build the tree step by step

**Example:**
```
Original: 4 + 2 * 3 * 7

Step 1: Label precedence
        4 + 2 * 3 * 7
          ↓   ↓   ↓
         LOW HI  HI

Step 2: Mark root (lowest precedence)
        4 [+] 2 * 3 * 7
          ↑
         ROOT

Step 3: Split
        4  |+|  2 * 3 * 7
```

---

### Tip 7: Remember the Precedence Rhyme

**"Please Excuse My Dear Aunt Sally"** (PEMDAS)
- **P**arentheses
- **E**xponents
- **M**ultiplication
- **D**ivision
- **A**ddition
- **S**ubtraction

Or create your own mnemonic: **"High Math, Low Add"**
- **High** precedence: **Multiplication** and division
- **Low** precedence: **Addition** and subtraction

---

### Tip 8: The "Chain to the Left" Pattern

When you have **same operators** repeated (like `2 + 3 + 4 + 5`), the tree always "chains to the left":

```
           +          ← Rightmost is root
          / \
         +   5        ← Next goes here
        / \
       +   4          ← Next goes here
      / \
     2   3            ← Leftmost is deepest
```

This pattern is consistent for all left-associative operators!

---

## Quick Reference

### Precedence Table

| Operator | Precedence | Associativity | Goes in Tree |
|----------|------------|---------------|--------------|
| `*` `/`  | High (1)   | Left-to-right | Deep (bottom) |
| `+` `-`  | Low (2)    | Left-to-right | High (top/root) |

Lower number = higher precedence = performed first = deeper in tree

### Decision Tree for Finding Root

```
START
  ↓
Do I have operators at DIFFERENT precedence levels?
  ↓                                    ↓
 YES                                  NO
  ↓                                    ↓
Pick the one with                Pick the RIGHTMOST
LOWEST precedence                operator
  ↓                                    ↓
That's the ROOT! ← ← ← ← ← ← ← That's the ROOT!
```

### Checklist for Building Trees

- [ ] Identify all operators in the expression
- [ ] Determine precedence of each operator
- [ ] Find the operator with lowest precedence
- [ ] If multiple operators have same low precedence, pick rightmost
- [ ] That operator is your root
- [ ] Split expression at root: left part → left subtree, right part → right subtree
- [ ] Repeat for each subtree until you reach single numbers/variables
- [ ] Verify by evaluating the tree

### Common Patterns

**Pattern 1: A + B * C**
```
   +
  / \
 A   *
    / \
   B   C
```
Result: A + (B * C)

**Pattern 2: A * B + C**
```
   +
  / \
 *   C
/ \
A  B
```
Result: (A * B) + C

**Pattern 3: A + B + C**
```
     +
    / \
   +   C
  / \
 A   B
```
Result: (A + B) + C

**Pattern 4: A * B * C**
```
     *
    / \
   *   C
  / \
 A   B
```
Result: (A * B) * C

---

## Practice Strategy

### Stage 1: Foundation (Spend 1-2 days here)
1. Master all Level 1 examples (single operator)
2. Understand why left = left, right = right
3. Practice drawing simple trees

### Stage 2: Associativity (Spend 2-3 days here)
1. Work through all Level 2 examples (same precedence)
2. Practice identifying the rightmost operator
3. Understand the "chain to the left" pattern

### Stage 3: Precedence (Spend 3-4 days here)
1. Complete all Level 3 examples (different precedence)
2. Practice identifying lowest precedence operator
3. Understand why low precedence = root

### Stage 4: Order Matters (Spend 2-3 days here)
1. Work through Level 4 examples (subtraction/division)
2. Practice maintaining left-to-right order
3. Verify with evaluation

### Stage 5: Integration (Spend 4-5 days here)
1. Tackle Level 5 examples (three+ operators)
2. Practice the step-by-step method
3. Build complex trees in stages

### Stage 6: Mastery (Ongoing practice)
1. Challenge yourself with Level 6 examples
2. Create your own expressions and solve them
3. Teach someone else (best way to solidify understanding!)

### Daily Practice Routine

**15-Minute Daily Practice:**
1. Pick 3 examples from your current level
2. Build the trees on paper
3. Evaluate to verify
4. If you get any wrong, review that level's explanation
5. Move to next level when you get 10 in a row correct

---

## Final Thoughts

Building abstract syntax trees is a **fundamental skill** in computer science. It might seem tricky at first, but with practice, it becomes second nature!

**Remember:**
- **Precedence** determines depth (high = deep, low = shallow/root)
- **Associativity** handles same-precedence operators (pick rightmost for left-associative)
- **Order matters** for subtraction and division (never swap!)
- **Practice** is key - you'll get faster and more confident

**You've got this!** 🎯

Keep this guide handy, work through the examples systematically, and you'll master ASTs in no time!

---

## Additional Resources

### For More Practice
- Try creating your own expressions and building trees
- Work backward: draw a tree and figure out what expression it represents
- Challenge friends to tree-building contests!

### Common Exam Question Types
1. **Given expression, draw tree** (most common)
2. **Given tree, write expression**
3. **Evaluate tree bottom-up**
4. **Identify errors in trees**
5. **Convert expression to ML/SML datatype**

### Need Help?
- Review the step-by-step method
- Go back to easier levels
- Draw everything out on paper
- Verify with evaluation
- Practice, practice, practice!

**Good luck with your studies!** 🚀
