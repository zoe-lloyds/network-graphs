To create a network graph using a **BFS (Breadth-First Search) layout** and highlight a **Person of Interest (POI)** in the graph, we can use NetworkX’s BFS traversal to emphasize the relationships radiating outward from the POI.

---

### 1. **Setup the Data**
Assume we have a DataFrame `df` with columns `party_id`, `relation_id`, and `relationship_type`. The `Person of Interest (POI)` is one of the `party_id`s.

```python
import pandas as pd
import networkx as nx
import matplotlib.pyplot as plt

# Example data
data = {
    "party_id": [1, 2, 3, 1, 4, 5],
    "relation_id": ["A", "A", "B", "C", "C", "D"],
    "relationship_type": ["POA", "POA", "Guardian", "Guardian", "POA", "Guardian"]
}
df = pd.DataFrame(data)

# Define the person of interest (POI)
person_of_interest = 1
```

---

### 2. **Build the Graph**
Create a graph where nodes represent `party_id` and `relation_id`, and edges represent relationships.

```python
# Create a graph
G = nx.Graph()

# Add edges to the graph
for _, row in df.iterrows():
    G.add_edge(row["party_id"], row["relation_id"], relationship_type=row["relationship_type"])
```

---

### 3. **Perform BFS Traversal**
Use NetworkX to compute a **BFS tree** starting from the POI.

```python
# Generate a BFS tree from the Person of Interest
bfs_edges = list(nx.bfs_edges(G, source=person_of_interest))
bfs_nodes = [person_of_interest] + [v for _, v in bfs_edges]  # Include source in BFS

print("BFS Nodes (from POI):", bfs_nodes)
```

---

### 4. **Visualize with BFS Layout**
Use a **bfs_layout** to arrange the graph, with nodes closer to the POI appearing nearer.

```python
# Create BFS layout
bfs_pos = nx.bfs_layout(G, source=person_of_interest)

# Highlight POI and BFS nodes
node_colors = ["red" if node == person_of_interest else "lightblue" for node in G.nodes]
node_sizes = [800 if node == person_of_interest else 300 for node in G.nodes]

# Draw the graph
plt.figure(figsize=(12, 10))
nx.draw(
    G,
    pos=bfs_pos,  # Use BFS layout
    with_labels=True,
    node_color=node_colors,
    edge_color="gray",
    node_size=node_sizes,
    font_size=10,
)
plt.title("Network Graph with BFS Layout from Person of Interest")
plt.show()
```

---

### 5. **Enhanced Analysis**
#### Add Edge Labels
To show relationship types on edges:

```python
# Add edge labels
edge_labels = nx.get_edge_attributes(G, "relationship_type")

# Draw the graph with edge labels
plt.figure(figsize=(12, 10))
nx.draw(
    G,
    pos=bfs_pos,
    with_labels=True,
    node_color=node_colors,
    edge_color="gray",
    node_size=node_sizes,
    font_size=10,
)
nx.draw_networkx_edge_labels(
    G, pos=bfs_pos, edge_labels=edge_labels, font_size=8, font_color="blue"
)
plt.title("Network Graph with BFS Layout and Edge Labels")
plt.show()
```

---

### 6. **Interpretation**
- **Centrality of POI**: The POI is at the center, with nodes radiating outward.
- **Proximity and Relationships**: Nodes directly connected to the POI are neighbors in the BFS layout, emphasizing their relationship.
- **Edge Labels**: Display relationship types (e.g., `POA`, `Guardian`) for clarity.

---

### Extensions
- Use node colors to represent clusters or relationship types.
- Filter BFS traversal to a certain depth (e.g., only immediate connections).
- Export the BFS subtree for further analysis.

Let me know if you'd like to expand this further!
