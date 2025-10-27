+
   / \
  5   3
```

### **2. `5 * 3`**
```
    *
   / \
  5   3
```

### **3. `4 + 7`**
```
    +
   / \
  4   7
```

### **4. `2 * 4`**
```
    *
   / \
  2   4
```

---

## **DIFFICULTY LEVEL 2: Two Operators - Same Precedence**
*Need to apply left-associativity, but no precedence confusion*

### **5. `2 + 3 + 4`**
```
       +
      / \
     +   4
    / \
   2   3
```
**Key concept:** Rightmost `+` is root (left-associative)

### **6. `2 * 3 * 4`**
```
       *
      / \
     *   4
    / \
   2   3
```
**Key concept:** Rightmost `*` is root (left-associative)

---

## **DIFFICULTY LEVEL 3: Two Operators - Different Precedence (Simple Position)**
*Need to understand precedence, but structure is straightforward*

### **7. `1 + 2 * 3`**
```
       +
      / \
     1   *
        / \
       3   2
```
**Key concept:** `+` is root (lower precedence), multiplication on RIGHT

### **8. `5 + 3 * 2`**
```
       +
      / \
     5   *
        / \
       3   2
```
**Key concept:** Same as above - `+` is root, multiplication on RIGHT

### **9. `1 * 2 + 3`**
```
       +
      / \
     *   3
    / \
   1   2
```
**Key concept:** `+` is root (lower precedence), multiplication on LEFT

### **10. `5 * 3 + 2`**
```
       +
      / \
     *   2
    / \
   5   3
```
**Key concept:** Same pattern - `+` is root, multiplication on LEFT

---

## **DIFFICULTY LEVEL 4: Two Operators with Subtraction/Division**
*Same as Level 3, but subtraction/division makes order more critical*

### **11. `8 - 2 * 3`**
```
       -
      / \
     8   *
        / \
       2   3
```
**Key concept:** `-` is root, multiplication on RIGHT

### **12. `8 * 2 - 3`**
```
       -
      / \
     *   3
    / \
   8   2
```
**Key concept:** `-` is root, multiplication on LEFT

### **13. `6 / 2 + 1`**
```
       +
      / \
     /   1
    / \
   6   2
```
**Key concept:** `+` is root, division on LEFT

---

## **DIFFICULTY LEVEL 5: Three Operators - Mixed Precedence**
*Multiple operators with precedence rules AND associativity*

### **14. `9 + 2 * 4 + 1`** (from your Quiz!)
```
        +
       / \
      +   1
     / \
    9   *
       / \
      2   4
```
**Key concepts:** 
- Rightmost `+` is root
- First `+` is next level
- Multiplication is deepest

### **15. `4 + 2 * 3 * 7`**
```
        +
       / \
      4   *
         / \
        *   7
       / \
      2   3
```
**Key concepts:**
- `+` is root (lowest precedence)
- Rightmost `*` is next
- Leftmost `*` is deepest

### **16. `4 * 2 * 3 + 7`**
```
        +
       / \
      *   7
     / \
    *   3
   / \
  4   2
```
**Key concepts:**
- `+` is root
- Chain of multiplications on LEFT side
- Left-associative chaining

### **17. `2 + 3 * 4 + 5`**
```
         +
        / \
       +   5
      / \
     2   *
        / \
       3   4
```
**Key concepts:**
- Rightmost `+` is root
- First `+` is next level
- Multiplication embedded in middle

---

## **DIFFICULTY LEVEL 6: Three Operators with Subtraction (Hardest)**
*Most complex: multiple operators, precedence, AND order matters for subtraction*

### **18. `2 - 4 - 1 * 3`** (The tricky one!)
```
         -
        / \
       -   *
      / \ / \
     2  4 1  3