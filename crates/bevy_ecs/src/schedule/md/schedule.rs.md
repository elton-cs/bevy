# Bevy ECS Schedule Module Documentation

This document provides a comprehensive overview of the public API available in the `schedule` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to manage scheduling of systems in a Bevy application or game.

## Overview

- **Purpose**: This module provides types and functions that enable the scheduling of systems, managing their execution order, dependencies, and conditions under which they should run.

## Structs

### `Schedules`
- **Description**: A resource that stores `Schedule`s mapped to `ScheduleLabel`s, excluding the currently running `Schedule`.
- **Key Points**:
  - Allows for the management of multiple schedules within a Bevy application.
  - Provides methods to insert, remove, and retrieve schedules.

### `Schedule`
- **Description**: A collection of systems, and the metadata and executor needed to run them in a certain order under certain conditions.
- **Key Points**:
  - Contains a directed acyclic graph structure for managing system dependencies.
  - Provides methods for adding systems, configuring sets, and running the schedule.

### `NodeConfig<T>`
- **Description**: Stores configuration for a single generic node (a system or a system set).
- **Key Points**:
  - Includes the node itself, scheduling metadata (hierarchy and dependencies), and run conditions associated with the node.

### `SystemConfig`
- **Description**: Stores configuration for a single system.
- **Key Points**:
  - A type alias for `NodeConfig<BoxedSystem>`.

### `NodeConfigs<T>`
- **Description**: A collection of generic `NodeConfig`s.
- **Key Points**:
  - Can represent either a single node configuration or a tuple of nested configurations.

### `SystemSetConfig`
- **Description**: A `SystemSet` with scheduling metadata.
- **Key Points**:
  - Used to manage sets of systems and their execution order.

### `SystemSetConfigs`
- **Description**: A collection of `SystemSetConfig`.
- **Key Points**:
  - Allows for the management of multiple system sets and their configurations.

## Enums

### `Chain`
- **Description**: Specifies how systems should be chained together in the schedule.
- **Variants**:
  - `Yes`: Run nodes in order, adding `apply_deferred` if necessary.
  - `YesIgnoreDeferred`: Run nodes in order without adding `apply_deferred`.
  - `No`: Nodes are allowed to run in any order.

## Functions

### `new_condition<M>(condition: impl Condition<M>) -> BoxedCondition`
- **Description**: Creates a new boxed condition from a given condition.
- **Parameters**:
  - `condition`: The condition to be boxed.
- **Returns**: A `BoxedCondition` that can be used to determine if a system should run.

### `ambiguous_with(graph_info: &mut GraphInfo, set: InternedSystemSet)`
- **Description**: Marks a system as ambiguous with another system set.
- **Parameters**:
  - `graph_info`: The graph information to update.
  - `set`: The system set to mark as ambiguous.
- **Usage**: Used to manage ambiguities in system execution order.

### `check_graph<V>(graph: &DiGraphMap<V, ()>, topological_order: &[V]) -> CheckGraphResults<V>`
- **Description**: Analyzes a directed acyclic graph (DAG) and computes its properties.
- **Parameters**:
  - `graph`: The directed graph to analyze.
  - `topological_order`: The order of nodes in the graph.
- **Returns**: A `CheckGraphResults<V>` containing the results of the analysis.

## Example Usage

### Creating a Schedule
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::schedule::Schedule;

fn my_system() {
    // System logic here
}

fn main() {
    let mut world = World::new();
    let mut schedule = Schedule::default();
    schedule.add_systems(my_system);
    schedule.run(&mut world);
}
```

### Adding Systems to a Schedule
```rust
fn another_system() {
    // Additional system logic here
}

fn setup_schedule() {
    let mut schedule = Schedule::default();
    schedule.add_systems((my_system, another_system));
}
```

### Configuring System Sets
```rust
fn configure_sets(schedule: &mut Schedule) {
    schedule.configure_sets(SystemSet::new());
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `schedule` module of the `bevy_ecs` library to manage system scheduling in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.