# Problem 1
# Equivalent Resistance Using Graph Theory

## Motivation:
Calculating equivalent resistance is a cornerstone of electrical circuit analysis, critical for designing efficient systems in electronics and engineering. Traditional methods, such as iteratively applying series and parallel resistor rules, can become tedious for complex circuits. Graph theory offers an elegant alternative by modeling circuits as graphs—nodes represent junctions, and edges represent resistors with weights equal to their resistance values. This approach enables systematic simplification of intricate networks and supports automated analysis, making it invaluable for circuit simulation, optimization, and network design. Beyond its practical utility, it bridges electrical engineering with mathematical concepts, showcasing the power of interdisciplinary methods.

## Task:
- **Implement an algorithm** in Python to calculate the equivalent resistance of a circuit using graph theory.
- **Ensure the implementation:**
  - Accepts a circuit graph as input.
  - Handles arbitrary resistor configurations (series, parallel, and nested combinations).
  - Outputs the final equivalent resistance.
- **Test the implementation** with examples:
  - Simple series and parallel combinations.
  - Nested configurations.
  - Complex graphs with multiple cycles.
- **Deliverables:**
  - A detailed explanation of the algorithm and its implementation.
  - Results for three example circuits.
  - A brief analysis of efficiency and potential improvements.

## Theory:

### Graph Representation:
- **Nodes:** Represent junctions or connection points in the circuit.
- **Edges:** Represent resistors, with weights equal to their resistance values (in ohms, Ω).
- **Series Connection:** Two resistors are in series if they share a node with a degree of 2 (only connected to two edges).
- **Parallel Connection:** Two resistors are in parallel if they share the same start and end nodes.

### Key Concepts:
- **Series Reduction:** For resistors \( R_1 \) and \( R_2 \) in series, the equivalent resistance is:
  \[
  R_{eq} = R_1 + R_2
  \]
- **Parallel Reduction:** For resistors \( R_1 \) and \( R_2 \) in parallel, the equivalent resistance is:
  \[
  R_{eq} = \frac{R_1 \cdot R_2}{R_1 + R_2}
  \]
- **Graph Simplification:** Iteratively identify and reduce series and parallel configurations until the graph collapses to a single edge between the source and sink nodes.

## Algorithm Description:
1. **Input:** A graph \( G \) with nodes, edges (resistors), and weights (resistance values), plus source and sink nodes.
2. **Steps:**
   - **Identify Series Connections:** Find nodes with degree 2, combine the adjacent edges, and remove the intermediate node.
   - **Identify Parallel Connections:** Find multiple edges between the same pair of nodes, compute their parallel equivalent, and replace profile_id_1them with a single edge.
   - **Iterate:** Repeat until only one edge remains between the source and sink.
3. **Output:** The weight of the final edge, representing the equivalent resistance.

## Python Implementation:

```python
import networkx as nx

def reduce_series(G):
    """Reduce series connections in the graph."""
    while True:
        series_node = None
        for node in G.nodes():
            if G.degree(node) == 2:  # Node connects exactly two edges
                series_node = node
                break
        if not series_node:
            break
        neighbors = list(G.neighbors(series_node))
        edge1 = G[neighbors[0]][series_node]
        edge2 = G[series_node][neighbors[1]]
        r1 = edge1['weight']
        r2 = edge2['weight']
        new_resistance = r1 + r2
        G.add_edge(neighbors[0], neighbors[1], weight=new_resistance)
        G.remove_node(series_node)

def reduce_parallel(G):
    """Reduce parallel connections in the graph."""
    while True:
        parallel_edges = None
        for u in G.nodes():
            for v in G.nodes():
                if u < v and G.number_of_edges(u, v) > 1:  # Multiple edges between u and v
                    parallel_edges = (u, v)
                    break
            if parallel_edges:
                break
        if not parallel_edges:
            break
        u, v = parallel_edges
        edges = G[u][v]
        resistances = [data['weight'] for data in edges.values()]
        parallel_res = 1 / sum(1/r for r in resistances)  # Parallel formula
        G.remove_edges_from([(u, v)] * len(resistances))
        G.add_edge(u, v, weight=parallel_res)

def equivalent_resistance(G, source, sink):
    """Calculate equivalent resistance between source and sink."""
    G = G.copy()  # Avoid modifying the original graph
    while G.number_of_nodes() > 2 or G.number_of_edges() > 1:
        reduce_series(G)
        reduce_parallel(G)
    return G[source][sink][0]['weight']

# Example Circuits
def test_circuits():
    # Example 1: Simple Series (2Ω + 3Ω)
    G1 = nx.MultiGraph()
    G1.add_edge('A', 'B', weight=2)
    G1.add_edge('B', 'C', weight=3)
    print("Example 1 - Series (2Ω + 3Ω):", equivalent_resistance(G1, 'A', 'C'), "Ω")

    # Example 2: Simple Parallel (4Ω || 4Ω)
    G2 = nx.MultiGraph()
    G2.add_edge('A', 'B', weight=4)
    G2.add_edge('A', 'B', weight=4)
    print("Example 2 - Parallel (4Ω || 4Ω):", equivalent_resistance(G2, 'A', 'B'), "Ω")

    # Example 3: Nested (2Ω in series with (3Ω || 6Ω))
    G3 = nx.MultiGraph()
    G3.add_edge('A', 'B', weight=2)
    G3.add_edge('B', 'C', weight=3)
    G3.add_edge('B', 'C', weight=6)
    print("Example 3 - Nested (2Ω + (3Ω || 6Ω)):", equivalent_resistance(G3, 'A', 'C'), "Ω")

# Run the tests
test_circuits()
```
- **Efficiency Analysis:** Iteratively identify and reduce series and parallel configurations until the graph collapses to a single edge between the source and sink nodes.
