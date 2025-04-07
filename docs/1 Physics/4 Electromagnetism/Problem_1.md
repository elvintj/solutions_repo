# Problem 1: 
#Equivalent Resistance Using Graph Theory

## Motivation

Calculating equivalent resistance is essential in understanding electrical circuits, especially when circuits become too complex for traditional methods. Graph theory offers a clean and algorithmic approach for modeling, simplifying, and computing resistances. By treating junctions as nodes and resistors as weighted edges, we can use graph reduction techniques to evaluate the total resistance.

This task demonstrates how graph theory can be applied to simplify complex resistor networks, both through code and visual step-by-step simplifications.

---

## Task

- Implement an algorithm that reduces a resistor network using graph theory.
- Visualize each step of the simplification.
- Demonstrate on a sample graph with both series and parallel connections.

---

## Theory

### Series Connection

Two resistors in series:
\[
R_{eq} = R_1 + R_2
\]

### Parallel Connection

Two resistors in parallel:
\[
\frac{1}{R_{eq}} = \frac{1}{R_1} + \frac{1}{R_2}
\quad \Rightarrow \quad
R_{eq} = \frac{R_1 \cdot R_2}{R_1 + R_2}
\]

---

## Circuit Example

We will use a graph that has the following structure:

- `A` — `B` with 4Ω
- `B` — `C` with 6Ω
- `C` — `D` with 12Ω
- `B` — `D` with 12Ω

This graph has:
- A **series path**: B → C → D
- A **parallel connection**: (B → C → D) in parallel with (B → D)

---

## Step-by-Step Simplification

### 🧩 Original Circuit

![Original Circuit](../../_pics/original_graph.png)

---

### 🔧 Step 1: Combine B-C-D into Series (6Ω + 12Ω = 18Ω), then find parallel with 12Ω

\[
R_{parallel} = \frac{12 \cdot 18}{12 + 18} = 7.2Ω
\]

![Step 1](../../_pics/step1_graph.png)

---

### 🔧 Step 2: Replace B-D with single equivalent 7.2Ω

Then combine the parallel B-D branches:

\[
R_{BD} = \frac{12 \cdot 7.2}{12 + 7.2} = 4.571Ω
\]

![Step 2](../../_pics/step2_graph.png)

---

### 🧮 Final Step: Combine A-B-D in series

\[
R_{eq} = 4Ω + 4.571Ω = 8.571Ω
\]

![Final Graph](../../_pics/final_graph.png)

---

## Final Result

**Total Equivalent Resistance**:  
\[
R_{eq} = \boxed{8.571\, \Omega}
\]

---

## Python Code

```python
import networkx as nx
import matplotlib.pyplot as plt

# Create Graph
G = nx.Graph()
G.add_edge('A', 'B', resistance=4)
G.add_edge('B', 'C', resistance=6)
G.add_edge('C', 'D', resistance=12)
G.add_edge('B', 'D', resistance=12)

# Series: B-C-D = 6 + 12 = 18
# Then parallel with B-D (12Ω)
R1 = 18
R2 = 12
R_parallel = (R1 * R2) / (R1 + R2)

# Combine A-B and B-D in series
R_eq = 4 + ((12 * R_parallel) / (12 + R_parallel))
print(f"Equivalent resistance: {R_eq:.3f} Ω")
```
## Test