# Graph-Based Text Classification with GAT and BERT

This project studies whether graph-based document representations can improve text classification. The main idea is to represent each document as a graph, where tokens are nodes and edges represent relationships between tokens.

The project compares different graph construction methods and later evaluates a hybrid architecture combining BERT embeddings with Graph Attention Networks (GAT).

## Project Overview

Text is usually processed as a sequence of tokens. In this project, we instead represent each document as an independent token-level graph.

Each document is converted into a graph:

- Nodes: token occurrences in the document
- Edges: relationships between tokens
- Node features: trainable word embeddings or BERT embeddings
- Model: Graph Attention Network (GAT)
- Output: document class prediction

## Experiments

### Experiment 1: Influence of Graph Topology

The goal of Experiment 1 is to isolate the effect of graph topology.

Two graph construction methods are compared:

1. **Co-occurrence graph**
   - Connects tokens that appear close to each other.
   - Uses a symmetric window radius of 3.

2. **Dependency graph**
   - Connects tokens according to syntactic dependency relations.
   - Dependency relations are extracted using spaCy.

In this experiment, BERT is not used. Instead, the model uses trainable word embeddings initialized from scratch. This allows the comparison to focus only on the graph structure.

### Experiment 2: Hybrid BERT-GAT Model

The second experiment evaluates a hybrid architecture:

- BERT provides contextual token embeddings.
- The selected graph topology provides explicit structural information.
- GAT propagates information between connected token nodes.

This experiment compares standalone BERT against BERT-GAT.

### Experiment 3: Data Efficiency

The third experiment studies how the models behave with different amounts of training data.

## Datasets

The project uses two datasets:

1. **Scientific Text Classification**
   - Source: Hugging Face
   - Dataset: `knowledgator/Scientific-text-classification`
   - Task: multi-class scientific topic classification
   - Classes used: 8

2. **IMDb Movie Reviews**
   - Source: Kaggle
   - Dataset: `lakshmi25npathi/imdb-dataset-of-50k-movie-reviews`
   - Task: binary sentiment classification
   - Classes: positive, negative

## Model Architecture

The main graph model is a Graph Attention Network:

```text
Token IDs
→ Embedding layer
→ GAT layer 1
→ GAT layer 2
→ Global mean pooling
→ Linear classifier
→ Class prediction
