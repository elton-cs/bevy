# Bevy ECS Parallel Commands Module Documentation

This document provides a comprehensive overview of the public API available in the `commands/parallel_scope` module of the `bevy_ecs` library. It includes details on structs, enums, traits, and their usage that can be utilized to manage commands in parallel contexts within an ECS application.

## Overview

- **Purpose**: This module provides an alternative to the standard `Commands` struct that can be used in parallel contexts, allowing for efficient command execution in multi-threaded environments.

## Structs

### `ParallelCommandQueue`
- **Description**: A struct that manages command queues for parallel execution.
- **Fields**:
  - `thread_queues`: A parallel collection of command queues.
- **Usage**: Used internally to handle commands that need to be executed in parallel across multiple threads.

### `ParallelCommands`
- **Description**: An alternative to `Commands` that can be used in parallel contexts, such as those in `Query::par_iter`.
- **Fields**:
  - `state`: Holds the state of the command queue for the current thread.
  - `entities`: A reference to the `Entities` resource, allowing access to entity data.
- **Usage**: Use `ParallelCommands` to issue commands in a parallelized manner, ensuring that commands can be executed concurrently without conflicts.

## Functions

### `ParallelCommands::command_scope`
- **Description**: Temporarily provides access to the `Commands` for the current thread.
- **Parameters**:
  - `f`: A closure that takes `Commands` as an argument and returns a value of type `R`.
- **Returns**: The result of the closure `f`.
- **Usage**: Use this method to define a scope in which commands can be issued for entities being processed in parallel. This allows for efficient command batching and execution.

## Example Usage

### Using ParallelCommands in a System
```rust
use bevy_ecs::prelude::*;
use bevy_tasks::ComputeTaskPool;

#[derive(Component)]
struct Velocity;

impl Velocity {
    fn magnitude(&self) -> f32 {
        42.0
    }
}

fn parallel_command_system(
    mut query: Query<(Entity, &Velocity)>,
    par_commands: ParallelCommands,
) {
    query.par_iter().for_each(|(entity, velocity)| {
        if velocity.magnitude() > 10.0 {
            par_commands.command_scope(|mut commands| {
                commands.entity(entity).despawn();
            });
        }
    });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `commands/parallel_scope` module of the `bevy_ecs` library to manage commands in parallel contexts in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.