# Bevy ECS Schedule Graph Utils Module Documentation

This document provides a comprehensive overview of the public API available in the `schedule/graph_utils` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to manage dependencies and graph structures in a Bevy application or game.

## Overview

- **Purpose**: This module provides utilities for managing the scheduling graph of systems in Bevy ECS. It includes functionality for handling dependencies, detecting ambiguities, and analyzing the graph structure.

## Enums

### `NodeId`
- **Description**: A unique identifier for a system or system set stored in a `ScheduleGraph`.
- **Variants**:
  - `System(usize)`: Identifier for a system.
  - `Set(usize)`: Identifier for a system set.
- **Key Points**:
  - Provides methods to check if the identifier corresponds to a system or a set.
  - Can be used to manage and reference nodes in the scheduling graph.

### `DependencyKind`
- **Description**: Specifies what kind of edge should be added to the dependency graph.
- **Variants**:
  - `Before`: A node that should be preceded.
  - `After`: A node that should be succeeded.
  - `BeforeNoSync`: A node that should be preceded without automatic synchronization.
  - `AfterNoSync`: A node that should be succeeded without automatic synchronization.

### `Ambiguity`
- **Description**: Configures ambiguity detection for a single system.
- **Variants**:
  - `Check`: Default state to check for ambiguities.
  - `IgnoreWithSet(Vec<InternedSystemSet>)`: Ignore warnings with systems in specified sets.
  - `IgnoreAll`: Ignore all warnings.

## Structs

### `Dependency`
- **Description**: Represents an edge to be added to the dependency graph.
- **Fields**:
  - `kind`: The type of dependency (before or after).
  - `set`: The system set associated with the dependency.
- **Usage**: Used to manage dependencies between systems in the scheduling graph.

### `GraphInfo`
- **Description**: Metadata about how a node fits into the schedule graph.
- **Fields**:
  - `hierarchy`: The sets that the node belongs to.
  - `dependencies`: The sets that the node depends on.
  - `ambiguous_with`: Information about ambiguity detection for the node.
- **Usage**: Used to store and manage information about nodes in the scheduling graph.

### `CheckGraphResults<V>`
- **Description**: Stores the results of graph analysis.
- **Fields**:
  - `reachable`: A boolean reachability matrix for the graph.
  - `connected`: Pairs of nodes that have a path connecting them.
  - `disconnected`: Pairs of nodes that don't have a path connecting them.
  - `transitive_edges`: Edges that are redundant because a longer path exists.
  - `transitive_reduction`: A variant of the graph with no transitive edges.
  - `transitive_closure`: A variant of the graph with all possible transitive edges.
- **Usage**: Used to store results from analyzing the scheduling graph.

## Functions

### `check_graph<V>(graph: &DiGraphMap<V, ()>, topological_order: &[V]) -> CheckGraphResults<V>`
- **Description**: Processes a directed acyclic graph (DAG) and computes its transitive reduction, transitive closure, reachability matrix, and connected/disconnected pairs of nodes.
- **Parameters**:
  - `graph`: The directed graph to analyze.
  - `topological_order`: The order of nodes in the graph.
- **Returns**: A `CheckGraphResults<V>` containing the results of the analysis.
- **Usage**: Call this function to analyze the dependencies and structure of the scheduling graph.

### `simple_cycles_in_component<N>(graph: &DiGraphMap<N, ()>, scc: &[N]) -> Vec<Vec<N>>`
- **Description**: Returns the simple cycles in a strongly-connected component of a directed graph.
- **Parameters**:
  - `graph`: The directed graph to analyze.
  - `scc`: The strongly-connected component to analyze.
- **Returns**: A vector of vectors, where each inner vector represents a cycle.
- **Usage**: Call this function to find cycles within a specific component of the graph.

## Example Usage

### Analyzing a Scheduling Graph
```rust
use bevy_ecs::prelude::*;
use petgraph::graphmap::DiGraphMap;

fn analyze_graph(graph: &DiGraphMap<NodeId, ()>, order: &[NodeId]) -> CheckGraphResults<NodeId> {
    check_graph(graph, order)
}
```

### Finding Cycles in a Component
```rust
fn find_cycles(graph: &DiGraphMap<NodeId, ()>, scc: &[NodeId]) -> Vec<Vec<NodeId>> {
    simple_cycles_in_component(graph, scc)
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `schedule/graph_utils` module of the `bevy_ecs` library to manage scheduling graphs in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.