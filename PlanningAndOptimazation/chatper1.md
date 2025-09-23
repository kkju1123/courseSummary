# Logical consequence vs Logical equivalence

This note explains the difference between **logical consequence** (semantic entailment) and **logical equivalence**, with definitions, examples, and quick methods to check each using truth tables.

---

## 1. Definitions

- **Logical consequence (semantic entailment)**

  \(\varphi \models \psi\) means: *for every interpretation* \(I\), if \(I \models \varphi\) then \(I \models \psi\).*

  Intuitively: **whenever \(\varphi\) is true, \(\psi\) must also be true**.

- **Logical equivalence**

  \(\varphi \equiv \psi\) means: \(\varphi \models \psi\) *and* \(\psi \models \varphi\). Equivalently, **for every interpretation \(I\), \(I \models \varphi\) iff \(I \models \psi\)**.

  Intuitively: \(\varphi\) and \(\psi\) **always have the same truth value**; they are interchangeable in every context.

---

## 2. Key differences (summary table)

| Feature | Logical consequence (\(\varphi \models \psi\)) | Logical equivalence (\(\varphi \equiv \psi\)) |
|---|---:|---:|
| Direction | One-way: truth of \(\varphi\) forces truth of \(\psi\) | Two-way: both force each other |
| Requirement | For every model \(I\), if \(I\models\varphi\) then \(I\models\psi\) | For every model \(I\), \(I\models\varphi\) iff \(I\models\psi\) |
| Strength | Weaker relation (many \(\psi\) may follow from a given \(\varphi\)) | Stronger; only formulas with identical truth behavior |
| Example (true) | \(p \wedge q \models p\) | \(p \wedge q \equiv q \wedge p\) |
| Example (false) | \(p \models q\) (not generally true) | \(p \equiv q\) (not true unless they are identical in truth table) |

---

## 3. How to check (practical methods)

### Using truth tables
- To test \(\varphi \models \psi\): build a truth table for both \(\varphi\) and \(\psi\). Check that **every row where \(\varphi\) is T also has \(\psi\) as T**.
- To test \(\varphi \equiv \psi\): build a truth table for both. Check that **in every row they have the same truth value**.

### Examples
1. **Consequence:**  
   \(\varphi = p \wedge q, \ \psi = p\).  
   Truth table rows where \(p \wedge q = T\) are exactly those where both \(p\) and \(q\) are T, so \(p = T\). Hence \(\varphi \models \psi\).

2. **Equivalence:**  
   \(\varphi = \neg(p \wedge q), \ \psi = (\neg p \vee \neg q)\).  
   Their truth tables match in all rows, so \(\varphi \equiv \psi\) (De Morgan’s law).

---

## 4. Analogy
- **Logical consequence:** like saying *“Whenever it’s raining (\(\varphi\)), the ground is wet (\(\psi\)).”*  
- **Logical equivalence:** like saying *“It’s raining (\(\varphi\)) if and only if water is falling from the sky (\(\psi\)).”*

---
