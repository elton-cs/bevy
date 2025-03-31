# Bevy ECS Simple Executor Module Documentation

This document provides a comprehensive overview of the public API available in the `executor/simple` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to run systems in a simple, single-threaded manner in a Bevy application or game.

## Overview

- **Purpose**: This module provides functionality for executing systems sequentially in a single-threaded environment. It immediately applies deferred changes after each system runs, making it suitable for simpler use cases where multi-threading is not required.

## Structs

### `SimpleExecutor`
- **Description**: A variant of the `SingleThreadedExecutor` that executes systems in order and applies deferred changes immediately after each system.
- **Fields**:
  - `evaluated_sets`: A bitset indicating which system sets have had their conditions evaluated.
  - `completed_systems`: A bitset indicating which systems have run or been skipped.
- **Usage**: Used to manage and execute systems in a simple, sequential manner.

## Functions

### `SimpleExecutor::new`
- **Description**: Creates a new instance of `SimpleExecutor`.
- **Returns**: A new `SimpleExecutor` with initialized fields.
- **Usage**: Call this method to create a new executor for use in a `Schedule`.

### `SimpleExecutor::init`
- **Description**: Initializes the executor with the given system schedule.
- **Parameters**:
  - `schedule`: A mutable reference to the `SystemSchedule` to initialize.
- **Usage**: Call this method to prepare the executor for running the specified schedule.

### `SimpleExecutor::run`
- **Description**: Executes the systems in the schedule.
- **Parameters**:
  - `schedule`: A mutable reference to the `SystemSchedule` to run.
  - `world`: A mutable reference to the `World` in which the systems operate.
  - `_skip_systems`: An optional bitset of systems to skip.
- **Usage**: Call this method to run the systems in the schedule sequentially.

### `SimpleExecutor::set_apply_final_deferred`
- **Description**: Sets whether to apply deferred changes after all systems have run.
- **Parameters**:
  - `value`: A boolean indicating whether to apply final deferred changes.
- **Usage**: Call this method to configure the executor's behavior regarding deferred changes.

### `evaluate_and_fold_conditions`
- **Description**: Evaluates and folds conditions for systems.
- **Parameters**:
  - `conditions`: A mutable slice of conditions to evaluate.
  - `world`: A mutable reference to the `World`.
- **Returns**: A boolean indicating if all conditions were met.
- **Usage**: Used to determine if a system can run based on its conditions.

## Tests

### `skip_automatic_sync_points`
- **Description**: Tests that automatic sync points (deferred systems) are not executed.
- **Usage**: Ensures that systems inserted as markers do not interfere with the execution of actual systems.

## Example Usage

### Creating and Running a Simple Executor
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::schedule::{Schedule, SimpleExecutor};

fn main() {
    let mut world = World::new();
    let mut schedule = Schedule::default();
    let mut executor = SimpleExecutor::new();

    // Add systems to the schedule
    schedule.add_systems(|| {
        // System logic here
    });

    // Run the schedule using the simple executor
    executor.run(&mut schedule, &mut world, None);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `executor/simple` module of the `bevy_ecs` library to manage system execution in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.