# Bevy Query Error Module Documentation

This document provides a comprehensive overview of the public API available in the `error` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to handle errors when working with queries in a Bevy application or game.

## Enums

### `QueryEntityError<'w>`
- **Description**: An error that occurs when retrieving a specific `Entity`'s query result from `Query` or `QueryState`.
- **Variants**:
  - **`QueryDoesNotMatch(Entity, UnsafeWorldCell<'w>)`**:
    - **Description**: Indicates that the given `Entity`'s components do not match the query.
    - **Key Points**:
      - This error occurs when the entity lacks a requested component or has a component that the query filters out.
  - **`NoSuchEntity(Entity)`**:
    - **Description**: Indicates that the given `Entity` does not exist.
    - **Key Points**:
      - This error is raised when attempting to access an entity that has been despawned or never existed.
  - **`AliasedMutability(Entity)`**:
    - **Description**: Indicates that the `Entity` was requested mutably more than once.
    - **Key Points**:
      - This error is relevant when using `QueryState::get_many_mut`, which requires unique mutable access to entities.

- **Implementations**:
  - **`core::error::Error`**: Implements the standard error trait for custom error handling.
  - **`core::fmt::Display`**: Provides a user-friendly string representation of the error.
  - **`core::fmt::Debug`**: Implements debug formatting for the error, useful for logging and debugging.

### `QuerySingleError`
- **Description**: An error that occurs when evaluating a `Query` or `QueryState` as a single expected result via `get_single` or `get_single_mut`.
- **Variants**:
  - **`NoEntities(&'static str)`**:
    - **Description**: Indicates that no entity fits the query.
    - **Key Points**:
      - This error is raised when a query returns no results.
  - **`MultipleEntities(&'static str)`**:
    - **Description**: Indicates that multiple entities fit the query.
    - **Key Points**:
      - This error is raised when a query that expects a single result finds multiple matching entities.

- **Implementations**:
  - **`derive_more::derive::Display`**: Provides a user-friendly string representation of the error.
  - **`derive_more::derive::Error`**: Implements the standard error trait for custom error handling.

## Example Usage

### Handling QueryEntityError
```rust
use bevy_ecs::prelude::*;

fn handle_query_error(entity: Entity, world: &World) {
    match world.query::<&NotPresent>().get(world, entity) {
        Ok(_) => {
            // Handle successful query
        }
        Err(err) => {
            println!("Error retrieving entity: {}", err);
            // Handle the error appropriately
        }
    }
}
```

### Handling QuerySingleError
```rust
fn get_single_entity(world: &World) {
    match world.query::<&MyComponent>().get_single(world) {
        Ok(entity) => {
            // Successfully retrieved the single entity
        }
        Err(err) => {
            println!("Error retrieving single entity: {}", err);
            // Handle the error appropriately
        }
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `error` module of the `bevy_ecs` library to manage errors related to queries in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.