# Bevy Query World Query Module Documentation

This document provides a comprehensive overview of the public API available in the `world_query` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to define and manage queries in a Bevy application or game.

## Traits

### `WorldQuery`
- **Description**: A trait for types that can be used as parameters in a `Query`.
- **Key Points**:
  - Types that implement this trait should also implement either `QueryData` or `QueryFilter`.
  - Provides methods for fetching query results and managing component access.
- **Safety**: Implementors must ensure that methods like `update_component_access`, `matches_component_set`, and `fetch` obey specific safety invariants regarding component access.

### `QueryData`
- **Description**: A trait for types that can be fetched from a `World` using a `Query`.
- **Key Points**:
  - Defines the item type returned by the query and the fetch type used to retrieve data.
  - Allows for complex queries by enabling tuples and custom types to be used as query parameters.

### `QueryFilter`
- **Description**: A trait for types that filter the results of a `Query`.
- **Key Points**:
  - Types that implement this trait can be used to filter entities based on their components.
  - Allows for complex filtering logic by combining multiple filters.

### `ArchetypeFilter`
- **Description**: A marker trait to indicate that the filter works at an archetype level.
- **Key Points**:
  - Only implemented for filters where the corresponding `QueryFilter::IS_ARCHETYPAL` is true.
  - Ensures that archetype-level optimizations can be applied.

## Structs

### `StorageId`
- **Description**: An ID for either a table or an archetype, used for query iteration.
- **Key Points**:
  - This union allows for efficient storage and access based on the type of query being executed (table or archetype).

### `NopWorldQuery<D: QueryData>`
- **Description**: A query that does not access any data.
- **Key Points**:
  - Used to create a query that effectively does nothing, useful for certain query compositions.

## Implementations

### `WorldQuery` Implementations
- **`unsafe impl WorldQuery for With<T>`**:
  - **Description**: Allows querying entities that have a specific component `T`.
  
- **`unsafe impl WorldQuery for Without<T>`**:
  - **Description**: Allows querying entities that do not have a specific component `T`.
  
- **`unsafe impl WorldQuery for Or<T>`**:
  - **Description**: Allows querying entities that match any of the specified filters.

### `QueryData` Implementations
- **`unsafe impl QueryData for Entity`**:
  - **Description**: Allows querying of entities directly.
  
- **`unsafe impl QueryData for EntityLocation`**:
  - **Description**: Allows querying of entity location metadata.

## Example Usage

### Creating a Query with Filters
```rust
use bevy_ecs::prelude::*;

#[derive(Component)]
struct A;

#[derive(Component)]
struct B;

fn my_system(world: &mut World) {
    let query_state = world.query::<&A>();
    // Use query_state to access components
}
```

### Using the With Filter
```rust
fn my_with_filter_system(query: Query<&Name, With<IsBeautiful>>) {
    for name in &query {
        println!("{} is looking lovely today!", name.name);
    }
}
```

### Using the Without Filter
```rust
fn my_without_filter_system(query: Query<&Name, Without<Permit>>) {
    for name in &query {
        println!("{} has no permit!", name.name);
    }
}
```

### Using the Or Filter
```rust
fn my_or_filter_system(query: Query<Entity, Or<(Changed<Color>, Changed<Node>)>>) {
    for entity in &query {
        println!("Entity {:?} got a new style or color", entity);
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `world_query` module of the `bevy_ecs` library to manage queries in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.