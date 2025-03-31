# Bevy ECS Command Queue Module Documentation

This document provides a comprehensive overview of the public API available in the `world/command_queue` module of the `bevy_ecs` library. It includes details on structs, enums, traits, and their usage that can be utilized to manage command queues for modifying the ECS state in a Bevy application.

## Overview

- **Purpose**: This module provides a `CommandQueue` struct that allows for efficiently queuing commands to perform structural changes to the `World`, such as spawning entities, inserting components, and managing resources.

## Structs

### `CommandQueue`
- **Description**: A densely and efficiently stores a queue of heterogeneous types implementing `Command`.
- **Fields**:
  - `bytes`: A vector of `MaybeUninit<u8>` that stores the queued commands.
  - `cursor`: An index to track the current position in the command queue.
  - `panic_recovery`: A vector used to store commands that need to be recovered in case of a panic.
- **Usage**: Use `CommandQueue` to queue commands that modify the ECS state and apply them in sequence.

### `RawCommandQueue`
- **Description**: Wraps pointers to a `CommandQueue`, used internally to avoid stacked borrow rules when partially applying the world's command queue recursively.
- **Fields**:
  - `bytes`: A pointer to the underlying command queue's bytes.
  - `cursor`: A pointer to the cursor tracking the current position in the command queue.
  - `panic_recovery`: A pointer to the panic recovery vector.
- **Usage**: Used internally to manage command execution and ensure safety during command application.

## Functions

### `CommandQueue::push`
- **Description**: Pushes a `Command` onto the queue.
- **Parameters**:
  - `command`: The command to queue.
- **Usage**: Use this method to add commands to the command queue for later execution.

### `CommandQueue::apply`
- **Description**: Executes the queued `Command`s in the world after applying any commands in the world's internal queue.
- **Parameters**:
  - `world`: A mutable reference to the `World`.
- **Usage**: Call this method to apply all queued commands to the world.

### `CommandQueue::is_empty`
- **Description**: Returns false if there are any commands in the queue.
- **Returns**: A boolean indicating whether the queue is empty.
- **Usage**: Use this method to check if there are any commands queued for execution.

### `CommandQueue::append`
- **Description**: Takes all commands from another `CommandQueue` and appends them to the current queue, leaving the other queue empty.
- **Parameters**:
  - `other`: A mutable reference to another `CommandQueue`.
- **Usage**: Use this method to combine command queues.

## Example Usage

### Using CommandQueue in a System
```rust
use bevy_ecs::prelude::*;

#[derive(Component)]
struct Player { alive: bool }

fn spawn_player_system(mut commands: Commands) {
    commands.spawn_empty().insert(Player { alive: true });
}

fn main() {
    let mut world = World::new();
    let mut command_queue = CommandQueue::default();
    let mut commands = Commands::new(&mut command_queue, &world);
    spawn_player_system(commands);
    command_queue.apply(&mut world);
}
```

### Queueing Commands
```rust
fn modify_player_system(mut commands: Commands, player_entity: Entity) {
    commands.entity(player_entity).insert(Player { alive: false });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `world/command_queue` module of the `bevy_ecs` library to manage command queues in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.