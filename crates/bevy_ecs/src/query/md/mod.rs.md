# Bevy ECS Query Module Documentation

This document provides a comprehensive overview of the public API available in the `query` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to define and manage queries in a Bevy application or game.

## Modules

### `access`
- **Description**: Contains APIs for managing access to component data in the ECS.
- **Usage**: Use this module to define how components can be accessed during queries, ensuring safe and efficient data retrieval.

### `builder`
- **Description**: Provides functionality for building queries.
- **Usage**: Utilize this module to construct complex queries by combining different filters and data types.

### `error`
- **Description**: Defines error types related to querying.
- **Usage**: Handle errors that may arise during query execution, ensuring robust error management in your systems.

### `fetch`
- **Description**: Contains APIs for fetching component data from the world.
- **Usage**: Use this module to retrieve data from entities based on defined queries, allowing for efficient data manipulation.

### `filter`
- **Description**: Provides filtering capabilities for queries.
- **Usage**: Apply filters to queries to include or exclude entities based on their components, enabling fine-grained control over data retrieval.

### `iter`
- **Description**: Contains iterator implementations for queries.
- **Usage**: Use this module to iterate over query results, allowing for easy processing of entities and their components.

### `par_iter`
- **Description**: Provides parallel iterator implementations for queries.
- **Usage**: Utilize this module to process query results in parallel, improving performance for large datasets.

### `state`
- **Description**: Manages the state of queries.
- **Usage**: Use this module to maintain and manage the state of queries across frames, ensuring consistent data access.

### `world_query`
- **Description**: Contains the `WorldQuery` trait and related implementations.
- **Usage**: Implement this trait to define custom queries that can be used to fetch data from the ECS.

## Traits

### `DebugCheckedUnwrap`
- **Description**: A trait for safely unwrapping `Option` and `Result` types in debug mode.
- **Key Points**:
  - Provides a method `debug_checked_unwrap` that panics in debug mode if called on `None` or `Err`.
  - Ensures that unsafe operations are only performed on valid values.

## Example Usage

### Creating a Query
```rust
use bevy_ecs::prelude::*;

#[derive(Component)]
struct Position(f32, f32);

#[derive(Component)]
struct Velocity(f32, f32);

fn move_system(query: Query<(&Position, &Velocity)>) {
    for (position, velocity) in query.iter() {
        // Update position based on velocity
    }
}
```

### Using Filters
```rust
fn filter_system(query: Query<&Position, With<Velocity>>) {
    for position in query.iter() {
        // Process entities with a Position component that also have a Velocity component
    }
}
```

### Iterating Over Query Results
```rust
fn print_positions(query: Query<&Position>) {
    for position in query.iter() {
        println!("Position: {:?}", position);
    }
}
```

### Parallel Iteration
```rust
fn parallel_move_system(query: Query<(&Position, &mut Velocity)>) {
    query.par_iter_mut().for_each(|(position, mut velocity)| {
        // Update velocity based on position in parallel
    });
}
```

### Handling Errors
```rust
fn error_handling_system(query: Query<&Position>) {
    match query.get(entity) {
        Ok(position) => {
            // Use position
        }
        Err(err) => {
            // Handle error
        }
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `query` module of the `bevy_ecs` library to manage queries in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.