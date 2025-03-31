# Bevy ECS Commands Module Documentation

This document provides a comprehensive overview of the public API available in the `commands` module of the `bevy_ecs` library. It includes details on structs, enums, traits, and their usage that can be utilized to manage commands for modifying the ECS state in a Bevy application.

## Overview

- **Purpose**: This module provides a `Commands` struct that allows for performing structural changes to the `World`, such as spawning entities, inserting components, and managing resources.

## Structs

### `Commands`
- **Description**: A command queue to perform structural changes to the `World`.
- **Fields**:
  - `queue`: An internal queue for managing commands.
  - `entities`: A reference to the `Entities` resource, allowing access to entity data.
- **Usage**: Use `Commands` in your systems to queue commands that modify the ECS state.

### `EntityCommands`
- **Description**: A struct that provides methods for modifying a specific entity.
- **Fields**:
  - `entity`: The ID of the entity being modified.
  - `commands`: A reference to the `Commands` struct for queuing commands.
- **Usage**: Use `EntityCommands` to add, remove, or modify components of a specific entity.

### `EntityEntryCommands`
- **Description**: A wrapper around `EntityCommands` with convenience methods for working with a specified component type.
- **Fields**:
  - `entity_commands`: The underlying `EntityCommands`.
  - `marker`: A phantom data marker for the component type.
- **Usage**: Use `EntityEntryCommands` to modify a specific component of an entity conditionally.

## Functions

### `Commands::new`
- **Description**: Creates a new `Commands` instance from a `CommandQueue` and a `World`.
- **Parameters**:
  - `queue`: A mutable reference to a `CommandQueue`.
  - `world`: A reference to the `World`.
- **Returns**: A new `Commands` instance.
- **Usage**: Use this function to create a `Commands` instance when you need to perform commands on a specific world.

### `Commands::spawn_empty`
- **Description**: Reserves a new empty `Entity` to be spawned and returns its corresponding `EntityCommands`.
- **Returns**: An `EntityCommands` for the newly spawned entity.
- **Usage**: Use this method to create a new entity without any components.

### `Commands::spawn`
- **Description**: Pushes a command to the queue for creating a new entity with the given `Bundle`'s components.
- **Parameters**:
  - `bundle`: The bundle of components to associate with the new entity.
- **Returns**: An `EntityCommands` for the newly spawned entity.
- **Usage**: Use this method to spawn a new entity with specific components.

### `Commands::insert`
- **Description**: Adds a `Bundle` of components to the entity.
- **Parameters**:
  - `bundle`: The bundle of components to insert.
- **Returns**: A mutable reference to `Commands`.
- **Usage**: Use this method to add components to an existing entity.

### `Commands::remove`
- **Description**: Removes a `Bundle` of components from the entity.
- **Parameters**:
  - `bundle`: The bundle of components to remove.
- **Returns**: A mutable reference to `Commands`.
- **Usage**: Use this method to remove components from an existing entity.

### `Commands::despawn`
- **Description**: Despawns the entity.
- **Usage**: Use this method to remove an entity from the world.

### `Commands::trigger`
- **Description**: Sends a "global" trigger without any targets.
- **Parameters**:
  - `event`: The event to trigger.
- **Usage**: Use this method to trigger events that observers can respond to.

### `Commands::add_observer`
- **Description**: Spawns an `Observer` and returns the `EntityCommands` associated with the entity that stores the observer.
- **Parameters**:
  - `observer`: The observer system to add.
- **Returns**: An `EntityCommands` for the observer entity.
- **Usage**: Use this method to add an observer that listens for specific events.

## Example Usage

### Using Commands in a System
```rust
use bevy_ecs::prelude::*;

#[derive(Component)]
struct Player { alive: bool }

fn spawn_player_system(mut commands: Commands) {
    commands.spawn_empty().insert(Player { alive: true });
}

fn main() {
    let mut world = World::new();
    let mut schedule = Schedule::default();
    schedule.add_system(spawn_player_system);
    schedule.run(&mut world);
}
```

### Modifying an Entity with EntityCommands
```rust
fn modify_player_system(mut commands: Commands, player_entity: Entity) {
    commands.entity(player_entity).insert(Player { alive: false });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `commands` module of the `bevy_ecs` library to manage commands in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.