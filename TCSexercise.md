# 1. exercise 1
## 1.1 set, partial functions
 ![alt text](image-32.png)
## 1.2 Structural Induction

## 2.1 Regular Grammar
### 1. how to elimite start variable from the right side of the rule:
1. add new rules which replacing all occurences of S in r with S′ 
2. add new rule: for S -> w replace S with S' in w.
### 2. how to eliminate forbidden A -> ε
1. check S not occur in right side
2. find variable can derived to ε -> Vε
3. eliminate forbidden ε rule: 
4. If a variable from Vε occurs in the right-hand side, add another rule that directly emulates a subsequent replacement with the empty word: X → a and Y → b
![alt text](image-37.png)
### 3. Transform grammar to a NFA
![alt text](image-40.png)
![alt text](image-41.png)
![alt text](image-42.png)
### 4. Convert a NFA to DFA
![alt text](image-43.png)
![alt text](image-44.png)
![alt text](image-39.png)
### 5. Specify a regular grammar that generates the language recognized by the DFA
![alt text](image-45.png)
![alt text](image-46.png)
![alt text](image-47.png)
### 6. Product Automaton
A Product Automaton is a **finite automaton constructed by combining two or more automata**, typically to model the concurrent behavior of the systems they represent. 
# Exercise 3
## 1. Product Automaton
![alt text](image-54.png)
## 2. NFAs for Regular Expressions
![alt text](image-55.png)
![alt text](image-56.png)
![alt text](image-60.png)
![alt text](image-61.png)
## 3. DFA, GNFA and Regular Expressions
![alt text](image-57.png)
![alt text](image-58.png)
![alt text](image-62.png)
![alt text](image-63.png)
![alt text](image-64.png)
## 4. Regular Expressions
![alt text](image-59.png)
# Exercise 4
Chomsky Normal Form

