# Bevy ECS Executor Module Documentation

This document provides a comprehensive overview of the public API available in the `executor` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to run systems in a Bevy application or game.

## Overview

- **Purpose**: This module provides various executors for running a `SystemSchedule` on a `World`. It includes single-threaded, multi-threaded, and simple executors, allowing developers to choose the appropriate execution model for their application.

## Enums

### `ExecutorKind`
- **Description**: Specifies how a `Schedule` will be run.
- **Variants**:
  - `SingleThreaded`: Runs the schedule using a single thread. Useful for single-threaded environments.
  - `Simple`: Calls `apply_deferred` immediately after running each system.
  - `MultiThreaded`: Runs the schedule using a thread pool, allowing non-conflicting systems to run in parallel.
- **Usage**: Used to configure the execution model of a schedule based on the target platform and performance requirements.

## Structs

### `SystemSchedule`
- **Description**: Holds systems and conditions of a `Schedule` sorted in topological order.
- **Fields**:
  - `system_ids`: A list of system node IDs.
  - `systems`: A list of boxed systems.
  - `system_conditions`: Conditions associated with each system.
  - `system_dependencies`: Number of systems that each system depends on.
  - `system_dependents`: Systems that depend on each system.
  - `sets_with_conditions_of_systems`: Bitsets indicating which sets have conditions.
  - `set_ids`: A list of system set node IDs.
  - `set_conditions`: Conditions associated with each system set.
  - `systems_in_sets_with_conditions`: Systems that are in sets with conditions.
- **Usage**: Used to manage the execution order and dependencies of systems within a schedule.

### `apply_deferred`
- **Description**: Applies deferred system buffers after all systems have run.
- **Parameters**:
  - `world`: A mutable reference to the `World`.
- **Usage**: Used to finalize the execution of systems by applying any deferred changes, such as commands or other system buffers.

### `is_apply_deferred`
- **Description**: Checks if a system is an instance of `apply_deferred`.
- **Parameters**:
  - `system`: A boxed system to check.
- **Returns**: A boolean indicating if the system is an `apply_deferred` instance.
- **Usage**: Used to determine if a system should apply deferred changes.

## Traits

### `SystemExecutor`
- **Description**: A trait for types that can run a `SystemSchedule` on a `World`.
- **Methods**:
  - `kind(&self) -> ExecutorKind`: Returns the kind of executor.
  - `init(&mut self, schedule: &SystemSchedule)`: Initializes the executor with the given schedule.
  - `run(&mut self, schedule: &mut SystemSchedule, world: &mut World, skip_systems: Option<&FixedBitSet>)`: Runs the systems in the schedule.
  - `set_apply_final_deferred(&mut self, value: bool)`: Sets whether to apply deferred changes after all systems have run.
- **Usage**: Implemented by different executor types to provide specific execution behavior for systems.

## Example Usage

### Creating and Running a Schedule with an Executor
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::schedule::{Schedule, ExecutorKind};

fn main() {
    let mut world = World::new();
    let mut schedule = Schedule::default();
    schedule.set_executor_kind(ExecutorKind::SingleThreaded);

    // Add systems to the schedule
    schedule.add_systems(|| {
        // System logic here
    });

    // Run the schedule
    schedule.run(&mut world);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `executor` module of the `bevy_ecs` library to manage system execution in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.