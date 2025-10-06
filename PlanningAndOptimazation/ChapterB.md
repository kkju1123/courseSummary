# B5. Positive Normal Form and STRIPS
## Positive Normal Form
### 1. Positive Formula
1. A logical formula φ is positive if **no negation symbols** appear in φ.
2. Just ∧, ∨, ⊤, ⊥, and **atomic** propositions (like P, Q, R...)
### 2. Positive opertor
1. An operator o is positive if **pre(o)** and all effect conditions in **eff(o)** are positive.
### 3. Positive Propositional Planning Task
1. A propositional planning task ⟨V , I , O, γ⟩ is positive if all **operators** in O and the **goal** γ are positive.
### 4. Positive Normal Form
1. A propositional planning task is in positive normal form if it is **positive** and all **operator effects are flat**.
2. An operator’s effect is **flat** if it is a **conjunction of literals** — **not nested or conditional**.

That means:
No nested expressions like (A ∧ (B ∨ C))
No conditional effects like if A then B
No disjunctions inside effects