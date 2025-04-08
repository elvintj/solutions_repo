# Problem 1: 
# Equivalent Resistance Calculation Using Graph Theory

## Algorithm Description

In this solution, we will use a graph-based approach to calculate equivalent resistance by iteratively simplifying the circuit through series and parallel reductions. To implementat, we will use Python with the NetworkX library for graph manipulation.

### Key Concepts
1. **Graph Representation**:
   - Nodes represent junction points
   - Edges represent resistors with weights as resistance values

2. **Simplification Rules**:
   - Series: R_eq = R1 + R2 + ... + Rn (for resistors in a linear path)
   - Parallel: 1/R_eq = 1/R1 + 1/R2 + ... + 1/Rn (for resistors between same nodes)

3. **Steps of the Algorithm**:
   - Identifying series connections (nodes with degree 2)
   - Identifying parallel connections (multiple edges between same nodes)
   - Iteratively simplifying until only two nodes remain
   - Returning the final resistance value

## Implementation

```python
# Starts here
import networkx as nx
import matplotlib.pyplot as plt

class CircuitAnalyzer:
    def __init__(self):
        self.G = nx.MultiGraph()  # This allows multiple edges between nodes

    def add_resistor(self, node1, node2, resistance):
        self.G.add_edge(node1, node2, weight=resistance)
    
    def reduce_series(self):
        """Reduce series connections (nodes with degree 2)"""
        while True:
            series_nodes = [n for n in self.G.nodes() if self.G.degree(n) == 2]
            if not series_nodes:
                break
                
            node = series_nodes[0]
            neighbors = list(self.G.neighbors(node))
            if len(neighbors) != 2:
                continue
                
            n1, n2 = neighbors
            r1 = self.G[n1][node][0]['weight']
            r2 = self.G[node][n2][0]['weight']
            
            # Creating new edge with combined resistance
            self.G.add_edge(n1, n2, weight=r1 + r2)
            self.G.remove_node(node)
    
    def reduce_parallel(self):
        """Reduce parallel connections (multiple edges between same nodes)"""
        while True:
            parallel_found = False
            for n1 in self.G.nodes():
                for n2 in self.G.nodes():
                    if n1 >= n2:
                        continue
                    edges = self.G.get_edge_data(n1, n2)
                    if edges and len(edges) > 1:
                        parallel_found = True
                        # Calculating parallel resistance
                        total_inv = sum(1/e['weight'] for e in edges.values())
                        r_eq = 1/total_inv if total_inv != 0 else 0
                        
                        # Removing all edges and add equivalent
                        self.G.remove_edges_from([(n1, n2, k) for k in edges.keys()])
                        self.G.add_edge(n1, n2, weight=r_eq)
            if not parallel_found:
                break
    
    def calculate_equivalent_resistance(self, start_node, end_node):
        """Calculate equivalent resistance between two nodes"""
        while len(self.G.nodes()) > 2:
            self.reduce_series()
            self.reduce_parallel()
        
        if len(self.G.edges()) == 1:
            return list(self.G.edges(data=True))[0][2]['weight']
        return None

# This is the Function to visualize graph
def draw_circuit(G, title):
    pos = nx.spring_layout(G)
    nx.draw(G, pos, with_labels=True)
    edge_labels = nx.get_edge_attributes(G, 'weight')
    nx.draw_networkx_edge_labels(G, pos, edge_labels)
    plt.title(title)
    plt.show()
```


