# Bevy ECS Schedule Module Documentation

This document provides a comprehensive overview of the public API available in the `schedule` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to manage and execute systems in a Bevy application or game.

## Overview

- **Purpose**: This module contains APIs for ordering systems and executing them on a `World`. It provides functionality for managing system execution order, dependencies, and conditions under which systems should run.

## Enums

### `TestSet`
- **Description**: An enumeration representing different system sets for organizing systems.
- **Variants**:
  - `A`
  - `B`
  - `C`
  - `D`
  - `X`
- **Usage**: Used to categorize systems into different sets, allowing for more granular control over their execution order and conditions.

## Structs

### `SystemOrder`
- **Description**: A resource that holds the order of system execution.
- **Fields**:
  - `0`: A vector of `u32` representing the order in which systems were executed.
- **Usage**: Used to track the order of system execution for testing and debugging purposes.

### `RunConditionBool`
- **Description**: A resource that holds a boolean value to control whether certain systems should run.
- **Fields**:
  - `0`: A boolean indicating the condition for running systems.
- **Usage**: Used to conditionally execute systems based on the value of this boolean.

### `Counter`
- **Description**: A resource that holds a counter value.
- **Fields**:
  - `0`: An `AtomicU32` representing the count.
- **Usage**: Used to count the number of times a system has been executed.

## Functions

### `make_exclusive_system`
- **Description**: Creates a system that pushes a tag to the `SystemOrder` resource.
- **Parameters**:
  - `tag`: A `u32` tag to identify the system.
- **Usage**: Used to create systems that modify the `SystemOrder` resource in an exclusive manner.

### `make_function_system`
- **Description**: Creates a function-based system that modifies the `SystemOrder` resource.
- **Parameters**:
  - `tag`: A `u32` tag to identify the system.
- **Usage**: Used to create systems that can be executed within the Bevy ECS framework.

### `named_system`
- **Description**: A named system that pushes a maximum value to the `SystemOrder` resource.
- **Parameters**:
  - `resource`: A mutable reference to `SystemOrder`.
- **Usage**: Used to demonstrate named systems in the scheduling context.

### `named_exclusive_system`
- **Description**: An exclusive system that pushes a maximum value to the `SystemOrder` resource.
- **Parameters**:
  - `world`: A mutable reference to the `World`.
- **Usage**: Used to demonstrate exclusive systems in the scheduling context.

### `counting_system`
- **Description**: A system that increments a counter resource.
- **Parameters**:
  - `counter`: A reference to the `Counter` resource.
- **Usage**: Used to count the number of times this system is executed.

## Tests

### `system_execution`
- **Description**: Tests for executing systems.
- **Tests**:
  - `run_system`: Tests running a simple system.
  - `run_exclusive_system`: Tests running an exclusive system.
  - `parallel_execution`: Tests parallel execution of systems.

### `system_ordering`
- **Description**: Tests for ordering systems.
- **Tests**:
  - `order_systems`: Tests the order of systems execution.
  - `order_exclusive_systems`: Tests the order of exclusive systems.
  - `add_systems_correct_order`: Tests adding systems in the correct order.
  - `add_systems_correct_order_nested`: Tests adding nested systems in the correct order.

### `conditions`
- **Description**: Tests for systems with conditions.
- **Tests**:
  - `system_with_condition`: Tests a system that runs based on a condition.
  - `systems_with_distributive_condition`: Tests systems with distributive conditions.
  - `run_exclusive_system_with_condition`: Tests an exclusive system with a condition.
  - `multiple_conditions_on_system`: Tests systems with multiple conditions.

### `schedule_build_errors`
- **Description**: Tests for errors during schedule building.
- **Tests**:
  - `dependency_loop`: Tests for dependency loops.
  - `dependency_cycle`: Tests for dependency cycles.
  - `hierarchy_loop`: Tests for hierarchy loops.
  - `hierarchy_cycle`: Tests for hierarchy cycles.
  - `system_type_set_ambiguity`: Tests for ambiguity in system type sets.

### `system_ambiguity`
- **Description**: Tests for system ambiguity.
- **Tests**:
  - `one_of_everything`: Tests various systems for conflicts.
  - `read_only`: Tests read-only systems for conflicts.
  - `read_world`: Tests reading the world for conflicts.
  - `resources`: Tests resource conflicts.
  - `nonsend`: Tests non-send conflicts.
  - `components`: Tests component conflicts.
  - `events`: Tests event conflicts.

## Example Usage

### Creating and Running a Schedule
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::schedule::{Schedule, SystemOrder};

fn main() {
    let mut world = World::new();
    let mut schedule = Schedule::default();

    // Initialize resources
    world.init_resource::<SystemOrder>();

    // Add systems to the schedule
    schedule.add_systems(make_function_system(0));
    schedule.run(&mut world);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `schedule` module of the `bevy_ecs` library to manage system execution in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.