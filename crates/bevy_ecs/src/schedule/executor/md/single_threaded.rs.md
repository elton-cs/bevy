# Bevy ECS Single-Threaded Executor Module Documentation

This document provides a comprehensive overview of the public API available in the `executor/single_threaded` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to run systems in a single-threaded manner in a Bevy application or game.

## Overview

- **Purpose**: This module provides functionality for executing systems sequentially in a single-threaded environment. It is useful for scenarios where multi-threading is not required, allowing for simpler execution of systems.

## Structs

### `SingleThreadedExecutor`
- **Description**: Executes systems in a single-threaded manner, applying deferred changes immediately after each system runs.
- **Fields**:
  - `evaluated_sets`: A bitset indicating which system sets have had their conditions evaluated.
  - `completed_systems`: A bitset indicating which systems have run or been skipped.
  - `unapplied_systems`: A bitset indicating which systems have run but not had their buffers applied.
  - `apply_final_deferred`: A boolean indicating whether to apply deferred changes after all systems have run.
- **Usage**: Used to manage and execute systems in a simple, sequential manner.

## Functions

### `SingleThreadedExecutor::new`
- **Description**: Creates a new instance of `SingleThreadedExecutor`.
- **Returns**: A new `SingleThreadedExecutor` with initialized fields.
- **Usage**: Call this method to create a new executor for use in a `Schedule`.

### `SingleThreadedExecutor::init`
- **Description**: Initializes the executor with the given system schedule.
- **Parameters**:
  - `schedule`: A mutable reference to the `SystemSchedule` to initialize.
- **Usage**: Call this method to prepare the executor for running the specified schedule.

### `SingleThreadedExecutor::run`
- **Description**: Executes the systems in the schedule.
- **Parameters**:
  - `schedule`: A mutable reference to the `SystemSchedule` to run.
  - `world`: A mutable reference to the `World` in which the systems operate.
  - `_skip_systems`: An optional bitset of systems to skip.
- **Usage**: Call this method to run the systems in the schedule sequentially.

### `SingleThreadedExecutor::set_apply_final_deferred`
- **Description**: Sets whether to apply deferred changes after all systems have run.
- **Parameters**:
  - `apply_final_deferred`: A boolean indicating whether to apply final deferred changes.
- **Usage**: Call this method to configure the executor's behavior regarding deferred changes.

### `apply_deferred`
- **Description**: Applies deferred changes for systems that have run but not had their buffers applied.
- **Parameters**:
  - `schedule`: A mutable reference to the `SystemSchedule`.
  - `world`: A mutable reference to the `World`.
- **Usage**: Used to finalize the execution of systems by applying any deferred changes.

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

### Creating and Running a Single-Threaded Executor
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::schedule::{Schedule, SingleThreadedExecutor};

fn main() {
    let mut world = World::new();
    let mut schedule = Schedule::default();
    let mut executor = SingleThreadedExecutor::new();

    // Add systems to the schedule
    schedule.add_systems(|| {
        // System logic here
    });

    // Run the schedule using the single-threaded executor
    executor.run(&mut schedule, &mut world, None);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `executor/single_threaded` module of the `bevy_ecs` library to manage system execution in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.