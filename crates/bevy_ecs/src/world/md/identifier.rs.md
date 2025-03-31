# Bevy ECS Identifier Module Documentation

This document provides a comprehensive overview of the public API available in the `world/identifier` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to manage unique identifiers for worlds in an ECS-based application.

## Overview

- **Purpose**: This module defines a unique identifier for a `World`, allowing for safe and efficient management of multiple worlds within the Bevy ECS framework.

## Structs

### `WorldId`
- **Description**: A unique identifier for a `World`.
- **Fields**:
  - `usize`: The underlying value representing the unique ID.
- **Methods**:
  - `new`: Creates a new, unique `WorldId`. Returns `None` if the supply of unique `WorldId`s has been exhausted.
    - **Usage**: This method is used to generate unique identifiers for new worlds, ensuring that each world can be referenced distinctly.
  
```rust
let world_id = WorldId::new().expect("Failed to create a new WorldId");
```

## Static Variables

### `MAX_WORLD_ID`
- **Description**: A static atomic variable that tracks the next available `WorldId`.
- **Usage**: This variable is used internally to ensure that each `WorldId` generated is unique and to manage the lifecycle of world identifiers.

## Traits Implementations

### `FromWorld`
- **Description**: The `FromWorld` trait is implemented for `WorldId`, allowing it to be created from a reference to a `World`.
- **Methods**:
  - `from_world`: Returns the `WorldId` of the provided `World`.
  
```rust
impl FromWorld for WorldId {
    fn from_world(world: &mut World) -> Self {
        world.id()
    }
}
```

### `ReadOnlySystemParam`
- **Description**: The `ReadOnlySystemParam` trait is implemented for `WorldId`, allowing it to be used as a read-only parameter in systems.
- **Safety**: No world data is accessed, ensuring that it can be safely used in a read-only context.

### `SystemParam`
- **Description**: The `SystemParam` trait is implemented for `WorldId`, allowing it to be used as a parameter in systems that may require mutable access.
- **Methods**:
  - `init_state`: Initializes the state for the system parameter.
  - `get_param`: Retrieves the parameter from the world.

### `ExclusiveSystemParam`
- **Description**: The `ExclusiveSystemParam` trait is implemented for `WorldId`, allowing it to be used in systems that require exclusive access to the world.
- **Methods**:
  - `init`: Initializes the parameter for the system.
  - `get_param`: Retrieves the parameter from the system state.

### `SparseSetIndex`
- **Description**: The `SparseSetIndex` trait is implemented for `WorldId`, allowing it to be used as an index in sparse sets.
- **Methods**:
  - `sparse_set_index`: Returns the sparse set index for the `WorldId`.
  - `get_sparse_set_index`: Creates a `WorldId` from a given sparse set index.

## Example Usage

### Creating a New WorldId
```rust
fn create_world_id() {
    if let Some(world_id) = WorldId::new() {
        println!("Created a new WorldId: {:?}", world_id);
    } else {
        println!("Failed to create a new WorldId: supply exhausted.");
    }
}
```

### Using WorldId in a System
```rust
fn example_system(world_id: WorldId) {
    // Use the WorldId for some operation
    println!("Current WorldId: {:?}", world_id);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `world/identifier` module of the `bevy_ecs` library to manage unique identifiers for worlds in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.