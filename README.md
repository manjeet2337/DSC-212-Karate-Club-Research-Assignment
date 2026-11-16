# Karate Club Modularity Analysis

## Overview

This assignment studies community structure in **Zachary's Karate Club network** using Newman's spectral modularity method. The notebook builds the modularity matrix, performs a spectral split using its leading eigenvector, and then recursively splits communities to identify finer groups.

It also examines how the network's structure changes during partitioning by recalculating four node-level measures at every stage: degree centrality, betweenness centrality, closeness centrality, and clustering coefficient.

## Objectives

1. Implement recursive spectral modularity partitioning.
2. Visualise the network after every split, using a different colour for each community.
3. Calculate degree, betweenness, closeness, and clustering coefficient for every node after each split.
4. Plot the evolution of each metric for every node throughout the partitioning process.
5. Discuss the results, including the role of bridge nodes and the limitations of the method.

## Dataset

The analysis uses the Karate Club graph included in **NetworkX**, so no separate dataset file is required.

- **Nodes:** 34 club members
- **Edges:** 78 social connections
- **Graph:** undirected and connected
- **Leaders:** node 0 represents the instructor (Mr. Hi), and node 33 represents the club president.

The graph is converted to an unweighted binary adjacency matrix for the analysis. The recorded `club` attribute is used only to compare the detected communities with the club's historical split; it is not used by the partitioning algorithm.

## Methodology

### 1. Modularity matrix

The notebook constructs the modularity matrix using

\[
B_{ij}=A_{ij}-\frac{k_i k_j}{2m},
\]

where \(A\) is the adjacency matrix, \(k_i\) and \(k_j\) are node degrees, and \(m\) is the total number of edges.

The matrix is checked for symmetry, near-zero row sums, and a positive leading eigenvalue.

### 2. Spectral community split

For a community, the notebook takes the corresponding block of the modularity matrix and calculates its leading eigenvector. Nodes are divided into two groups according to the sign of their eigenvector entries.

### 3. Recursive partitioning

The detected groups are placed in a queue and considered for further splitting. A group is not split when its leading eigenvalue is non-positive or when the leading eigenvector does not produce two non-empty sign groups. The process continues until no further valid split is available.

Modularity \(Q\) is evaluated for the complete partition at each stage.

### 4. Visualisation

The notebook uses a fixed spring layout so node positions remain consistent across figures. Nodes are coloured by community. Solid edges connect nodes in the same community, while dashed grey edges indicate connections between different communities.

### 5. Node metrics

The following measures are recalculated after every split:

| Metric | What it measures |
|---|---|
| Degree centrality | The number of direct connections of a node, normalised by the graph size. |
| Betweenness centrality | How often a node lies on shortest paths between other nodes. |
| Closeness centrality | How close a node is to the other reachable nodes in the graph. |
| Clustering coefficient | How strongly a node's neighbours are connected to one another. |

For this part of the analysis, edges between different communities are removed before recalculating the metrics. Therefore, the values describe each node's position **within its own community**, rather than repeatedly measuring the unchanged original graph.

## Requirements

- Python 3.9 or later
- NumPy
- pandas
- NetworkX
- Matplotlib
- SciPy
- Jupyter Notebook or JupyterLab

Install the required packages with:

```bash
pip install numpy pandas networkx matplotlib scipy jupyter
```

## How to Run

1. Place `Karate_Club_Modularity_Report.ipynb` and this `README.md` in the same project directory.
2. Install the required packages.
3. Open a terminal in the project directory and start Jupyter:

   ```bash
   jupyter notebook
   ```

   Alternatively, open the notebook in JupyterLab or another compatible notebook environment.
4. Open `Karate_Club_Modularity_Report.ipynb`.
5. Run all cells from top to bottom.

The notebook creates an `outputs/` directory automatically and saves its figures and node-metric CSV file there.

## Main Results

- The first spectral split produces two groups of 16 and 18 members.
- The first split correctly places **33 out of 34 members** with their historical faction; node 8 is the one misclassified member.
- The modularity after the first split is **\(Q=0.3715\)**.
- Recursive partitioning produces **five communities after four splits**, with final modularity **\(Q=0.4094\)**.
- The final community sizes are 10, 13, 5, 5, and 1 members.
- The notebook's metric analysis shows substantial decreases in betweenness and closeness as cross-community links are removed. Clustering increases slightly on average, while the leaders retain importance within their respective groups.

## Generated Outputs

Running the notebook creates the following files in `outputs/`:

| File | Description |
|---|---|
| `01_real_split.png` | Historical division of the Karate Club network. |
| `02_first_split_vs_truth.png` | Comparison of the first spectral split with the historical factions. |
| `03_leading_eigenvector.png` | Leading eigenvector values used for the first split. |
| `04_splits_grid.png` | Network visualisation after each recursive split. |
| `05_final_communities.png` | Larger visualisation of the final five communities. |
| `06_split_tree.png` | Recursive split tree and the leading eigenvalue used at each split. |
| `07_modularity_progress.png` | Modularity across partitioning stages. |
| `08_block_structure.png` | Adjacency and modularity matrices ordered by final community. |
| `09_graph_and_mean_metrics.png` | Graph changes and mean node metrics across stages. |
| `10_metric_evolution.png` | Evolution of the four metrics for individual nodes. |
| `11_metric_heatmaps.png` | Heatmaps showing each node's metric values at every stage. |
| `12_stable_central_nodes.png` | Nodes that repeatedly appear among the most central nodes. |
| `node_metrics_by_stage.csv` | Node-level metric values for every stage. |

## Discussion and Limitations

The first spectral split captures the main historical division of the club with only one member placed on the opposite side. Further recursive splits reveal smaller subgroups and increase the modularity score.

Bridge nodes are particularly affected when connections between communities are removed: their betweenness and closeness centrality can decrease substantially. The leaders remain relatively important within their respective communities. The average clustering coefficient rises slightly because removing inter-community edges can leave more internally cohesive groups.

The implementation uses a stopping rule based on a non-positive leading eigenvalue or a trivial sign split. It also uses an unweighted graph, so potentially useful information in the original edge weights is not retained. These choices should be considered when interpreting the detected communities.

## References

1. Newman, M. E. J. (2006). *Modularity and community structure in networks*. Proceedings of the National Academy of Sciences, 103(23), 8577–8582.
2. Zachary, W. W. (1977). *An information flow model for conflict and fission in small groups*. Journal of Anthropological Research, 33(4), 452–473.
3. Hagberg, A. A., Schult, D. A., & Swart, P. J. (2008). *Exploring network structure, dynamics, and function using NetworkX*. Proceedings of the 7th Python in Science Conference (SciPy 2008).


# Submission

Submitted to - Dr. Saptarshi Bej {Course Instructor of DSC212}

Submitted by - Manjeet Jajoriya {Roll NO. - IMS25122}