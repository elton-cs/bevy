# Bevy Query Filter Module Documentation

This document provides a comprehensive overview of the public API available in the `filter` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to filter results of queries in a Bevy application or game.

## Enums

### `QueryEntityError<'w>`
- **Description**: An error that occurs when retrieving a specific `Entity`'s query result from `Query` or `QueryState`.
- **Variants**:
  - **`QueryDoesNotMatch(Entity, UnsafeWorldCell<'w>)`**:
    - **Description**: Indicates that the given `Entity`'s components do not match the query.
  - **`NoSuchEntity(Entity)`**:
    - **Description**: Indicates that the given `Entity` does not exist.
  - **`AliasedMutability(Entity)`**:
    - **Description**: Indicates that the `Entity` was requested mutably more than once.

### `QuerySingleError`
- **Description**: An error that occurs when evaluating a `Query` or `QueryState` as a single expected result via `get_single` or `get_single_mut`.
- **Variants**:
  - **`NoEntities(&'static str)`**:
    - **Description**: Indicates that no entity fits the query.
  - **`MultipleEntities(&'static str)`**:
    - **Description**: Indicates that multiple entities fit the query.

## Structs

### `With<T>(PhantomData<T>)`
- **Description**: A filter that selects entities with a component `T`.
- **Key Points**:
  - Used in a `Query` to ensure entities have the specified component.
  - This filter is the negation of `Without<T>`.

### `Without<T>(PhantomData<T>)`
- **Description**: A filter that selects entities without a component `T`.
- **Key Points**:
  - Used in a `Query` to ensure entities do not have the specified component.
  - This filter is the negation of `With<T>`.

### `Or<T>(PhantomData<T>)`
- **Description**: A filter that tests if any of the given filters apply.
- **Key Points**:
  - Useful for systems that want to run when one or more components have changed.
  - Equivalent to a logical OR operation on multiple filters.

### `Added<T>(PhantomData<T>)`
- **Description**: A filter that selects entities with a component `T` that has been added since the last system run.
- **Key Points**:
  - Commonly used for one-time initialization.
  - Works with change detection to identify newly added components.

### `Changed<T>(PhantomData<T>)`
- **Description**: A filter that selects entities with a component `T` that has changed since the last system run.
- **Key Points**:
  - Useful for detecting modifications to components.
  - Helps avoid redundant work when values have not changed.

### `StorageSwitch<C: Component, T: Copy, S: Copy>`
- **Description**: A compile-time checked union of two different types that differs based on the `StorageType` of a given component.
- **Key Points**:
  - Used to handle different storage types for components in a unified manner.

## Implementations

### `QueryFilter` Trait
- **Description**: A trait for types that filter the results of a `Query`.
- **Key Points**:
  - Types that implement this trait can be used to filter entities based on their components.
  - Allows for complex filtering logic by combining multiple filters.

### `ArchetypeFilter` Trait
- **Description**: A marker trait to indicate that the filter works at an archetype level.
- **Key Points**:
  - Only implemented for filters where the corresponding `QueryFilter::IS_ARCHETYPAL` is true.
  - Ensures that archetype-level optimizations can be applied.

### `WorldQuery` Implementations
- **`unsafe impl WorldQuery for With<T>`**:
  - **Description**: Allows querying entities that have a specific component `T`.
  
- **`unsafe impl WorldQuery for Without<T>`**:
  - **Description**: Allows querying entities that do not have a specific component `T`.
  
- **`unsafe impl WorldQuery for Or<T>`**:
  - **Description**: Allows querying entities that match any of the specified filters.

## Example Usage

### Using the With Filter
```rust
use bevy_ecs::prelude::*;

#[derive(Component)]
struct IsBeautiful;

#[derive(Component)]
struct Name { name: &'static str };

fn compliment_entity_system(query: Query<&Name, With<IsBeautiful>>) {
    for name in &query {
        println!("{} is looking lovely today!", name.name);
    }
}
```

### Using the Without Filter
```rust
fn no_permit_system(query: Query<&Name, Without<Permit>>) {
    for name in &query {
        println!("{} has no permit!", name.name);
    }
}
```

### Using the Or Filter
```rust
fn print_cool_entity_system(query: Query<Entity, Or<(Changed<Color>, Changed<Node>)>>) {
    for entity in &query {
        println!("Entity {:?} got a new style or color", entity);
    }
}
```

### Using the Added Filter
```rust
fn print_add_name_component(query: Query<&Name, Added<Name>>) {
    for name in &query {
        println!("Named entity created: {:?}", name);
    }
}
```

### Using the Changed Filter
```rust
fn print_moving_objects_system(query: Query<&Name, Changed<Transform>>) {
    for name in &query {
        println!("Entity Moved: {:?}", name);
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `filter` module of the `bevy_ecs` library to filter query results in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.