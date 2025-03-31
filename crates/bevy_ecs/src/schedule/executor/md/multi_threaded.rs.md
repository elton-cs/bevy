# Bevy ECS Multi-Threaded Executor Module Documentation

This document provides a comprehensive overview of the public API available in the `executor/multi_threaded` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to run systems in a multi-threaded manner in a Bevy application or game.

## Overview

- **Purpose**: This module provides functionality for executing systems in parallel using a thread pool. It allows non-conflicting systems to run simultaneously, improving performance in multi-core environments.

## Structs

### `Environment`
- **Description**: Holds borrowed data used by the `MultiThreadedExecutor`.
- **Fields**:
  - `executor`: A reference to the `MultiThreadedExecutor`.
  - `systems`: A slice of systems to be executed.
  - `conditions`: A mutable reference to the conditions for the systems.
  - `world_cell`: A reference to the world cell for accessing world data.
- **Usage**: Used internally by the executor to manage system execution context.

### `Conditions`
- **Description**: Holds mutable references to conditions for systems and sets.
- **Fields**:
  - `system_conditions`: Mutable references to system conditions.
  - `set_conditions`: Mutable references to set conditions.
  - `sets_with_conditions_of_systems`: A bitset indicating which sets have conditions.
  - `systems_in_sets_with_conditions`: A bitset indicating which systems are in sets with conditions.
- **Usage**: Used to evaluate conditions for systems before execution.

### `SystemTaskMetadata`
- **Description**: Contains metadata for scheduling and running system tasks.
- **Fields**:
  - `archetype_component_access`: Access information for the system.
  - `dependents`: Indices of systems that depend on this system.
  - `is_send`: Indicates if the system can be sent across threads.
  - `is_exclusive`: Indicates if the system is exclusive.
- **Usage**: Used to manage system dependencies and access during execution.

### `SystemResult`
- **Description**: Represents the result of running a system that is sent across a channel.
- **Fields**:
  - `system_index`: The index of the system that completed execution.
- **Usage**: Used to track the completion of systems in the executor.

### `MultiThreadedExecutor`
- **Description**: Executes systems using a thread pool, allowing for parallel execution of non-conflicting systems.
- **Fields**:
  - `state`: The running state, protected by a mutex.
  - `system_completion`: A queue for system completion events.
  - `apply_final_deferred`: A flag indicating whether to apply deferred system buffers.
  - `panic_payload`: A mutex for handling panics.
  - `starting_systems`: A bitset of systems that can start execution.
  - `executor_span`: A cached tracing span (if tracing is enabled).
- **Usage**: Used to manage and execute systems in a multi-threaded environment.

### `ExecutorState`
- **Description**: Represents the state of the executor while running.
- **Fields**:
  - `system_task_metadata`: Metadata for scheduling and running system tasks.
  - `active_access`: Access information for currently running systems.
  - `local_thread_running`: Indicates if a system with non-`Send` access is running.
  - `exclusive_running`: Indicates if an exclusive system is running.
  - `num_running_systems`: The number of systems currently running.
  - `num_dependencies_remaining`: The number of dependencies for each system.
  - `evaluated_sets`: A bitset of evaluated system sets.
  - `ready_systems`: A bitset of systems ready to run.
  - `running_systems`: A bitset of currently running systems.
  - `skipped_systems`: A bitset of systems that were skipped.
  - `completed_systems`: A bitset of systems that have completed execution.
  - `unapplied_systems`: A bitset of systems that have run but not had their buffers applied.
- **Usage**: Used to track the execution state of systems and manage dependencies.

## Functions

### `apply_deferred`
- **Description**: Applies deferred system buffers after all systems have completed.
- **Parameters**:
  - `unapplied_systems`: A bitset of systems that have not had their buffers applied.
  - `systems`: A slice of systems to apply buffers for.
  - `world`: A mutable reference to the world.
- **Returns**: A result indicating success or failure.
- **Usage**: Used to finalize the execution of systems by applying any deferred changes.

### `evaluate_and_fold_conditions`
- **Description**: Evaluates and folds conditions for systems.
- **Parameters**:
  - `conditions`: A mutable slice of conditions to evaluate.
  - `world`: A reference to the world cell.
- **Returns**: A boolean indicating if all conditions were met.
- **Usage**: Used to determine if a system can run based on its conditions.

### `MainThreadExecutor`
- **Description**: A resource that runs systems on the main thread.
- **Fields**:
  - `0`: An `Arc` containing the main thread executor.
- **Usage**: Used to manage system execution on the main thread.

### `MainThreadExecutor::new`
- **Description**: Creates a new executor for running systems on the main thread.
- **Returns**: A new instance of `MainThreadExecutor`.
- **Usage**: Call this method to initialize a new main thread executor.

## Tests

### `skipped_systems_notify_dependents`
- **Description**: Tests that skipped systems notify their dependents.
- **Usage**: Ensures that when a system is skipped, its dependents are still executed.

### `check_spawn_exclusive_system_task_miri`
- **Description**: Regression test for a bug related to exclusive system tasks.
- **Usage**: Ensures that exclusive systems can be spawned correctly without panicking.

## Example Usage

### Creating and Running a Multi-Threaded Executor
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::schedule::{Schedule, MultiThreadedExecutor};

fn main() {
    let mut world = World::new();
    let mut schedule = Schedule::default();
    let mut executor = MultiThreadedExecutor::new();

    // Add systems to the schedule
    schedule.add_systems(|| {
        // System logic here
    });

    // Run the schedule using the multi-threaded executor
    executor.run(&mut schedule, &mut world, None);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `executor/multi_threaded` module of the `bevy_ecs` library to manage system execution in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.