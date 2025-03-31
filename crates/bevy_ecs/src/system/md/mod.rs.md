# Bevy ECS System Module Documentation

This document provides a comprehensive overview of the public API available in the `system` module of the `bevy_ecs` library. It includes details on structs, enums, traits, and their usage that can be utilized to control behavior in an ECS application.

## Overview

- **Purpose**: This module provides tools for defining and managing systems in an ECS-based application. Systems dictate how the application behaves and interact with the ECS state.

## Structs

### `System`
- **Description**: Represents a system that can be executed within a Bevy ECS application.
- **Usage**: Systems are typically defined as functions that can query and mutate the ECS state.

### `Commands`
- **Description**: A struct used to queue commands for modifying the ECS state.
- **Usage**: Use `Commands` to add, remove, or modify entities and components within a system.

### `Query`
- **Description**: A struct that allows querying entities and their components.
- **Usage**: Use `Query` to access and manipulate components of entities that match specific criteria.

### `ParamSet`
- **Description**: A struct that allows grouping multiple queries or commands together.
- **Usage**: Use `ParamSet` to manage multiple queries or commands in a single system.

### `SystemState`
- **Description**: A struct that holds the state of a system, allowing it to maintain information across frames.
- **Usage**: Use `SystemState` to manage and access system parameters over multiple runs.

## Traits

### `IntoSystem`
- **Description**: A trait that allows converting a function into a system.
- **Methods**:
  - `into_system`: Converts a function into its corresponding `System`.
  - `pipe`: Passes the output of one system into another, creating a compound system.
  - `map`: Transforms the output of a system using a provided function.
- **Usage**: Implement this trait for functions to define them as systems that can be executed within the ECS framework.

### `SystemParam`
- **Description**: A trait that defines types that can be used as parameters in systems.
- **Usage**: Implement this trait for custom types that need to interact with the ECS state.

## Functions

### `assert_is_system`
- **Description**: Ensures that a given function is a valid system.
- **Parameters**:
  - `system`: The function to validate as a system.
- **Usage**: Use this function in documentation examples to confirm that systems are valid.

### `assert_is_read_only_system`
- **Description**: Ensures that a given function is a read-only system.
- **Parameters**:
  - `system`: The function to validate as a read-only system.
- **Usage**: Use this function in documentation examples to confirm that systems do not mutate state.

### `run_system`
- **Description**: Executes a system within a given world.
- **Parameters**:
  - `world`: The world in which to run the system.
  - `system`: The system to execute.
- **Usage**: Use this function to run systems and apply their logic to the ECS state.

### `pipe_change_detection`
- **Description**: Allows chaining systems together while maintaining change detection.
- **Parameters**:
  - `system`: The system to chain.
- **Usage**: Use this function to create complex systems that depend on the output of other systems.

## Example Usage

### Defining and Running a System
```rust
use bevy_ecs::prelude::*;

#[derive(Component)]
struct Player { alive: bool }

#[derive(Component)]
struct Score(u32);

fn update_score_system(
    mut query: Query<(&Player, &mut Score)>,
) {
    for (player, mut score) in &mut query {
        if player.alive {
            score.0 += 1;
        }
    }
}

fn main() {
    let mut world = World::new();
    let mut schedule = Schedule::default();
    schedule.add_system(update_score_system);
    schedule.run(&mut world);
}
```

### Using Commands in a System
```rust
fn spawn_player_system(mut commands: Commands) {
    commands.spawn().insert(Player { alive: true }).insert(Score(0));
}
```

### Querying Components
```rust
fn print_scores_system(query: Query<&Score>) {
    for score in &query {
        println!("Score: {}", score.0);
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `system` module of the `bevy_ecs` library to manage systems in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.