# Bevy ECS Sparse Set Module Documentation

This document provides a comprehensive overview of the public API available in the `storage/sparse_set` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to manage sparse data structures in a Bevy application or game.

## Overview

- **Purpose**: This module provides a sparse data structure designed for efficiently storing and managing components in an Entity-Component-System (ECS) architecture. It allows for fast insertions and deletions while maintaining a mapping between entities and their associated components.

## Structs

### `SparseArray<I, V>`
- **Description**: A sparse array that holds values indexed by a sparse set index.
- **Fields**:
  - `values`: A vector of optional values.
  - `marker`: A phantom data marker for the index type.
- **Usage**: Used internally to manage sparse data storage for components.

### `ImmutableSparseArray<I, V>`
- **Description**: A space-optimized version of `SparseArray` that cannot be changed after construction.
- **Fields**:
  - `values`: A boxed slice of optional values.
  - `marker`: A phantom data marker for the index type.
- **Usage**: Used to provide immutable access to sparse data.

### `ComponentSparseSet`
- **Description**: A sparse data structure for storing components associated with entities.
- **Fields**:
  - `dense`: A column of component data.
  - `entities`: A vector of entity indices.
  - `sparse`: A sparse array mapping entity indices to dense indices.
- **Usage**: Used to manage component data for entities in a sparse manner, allowing for efficient access and modification.

### `SparseSets`
- **Description**: A collection of `ComponentSparseSet` storages, indexed by `ComponentId`.
- **Fields**:
  - `sets`: A sparse set of component sparse sets.
- **Usage**: Used to manage multiple component types and their associated sparse sets within a `World`.

## Functions

### `SparseArray::new`
- **Description**: Creates a new `SparseArray`.
- **Returns**: A new instance of `SparseArray`.
- **Usage**: Call this method to initialize a sparse array for storing values.

### `SparseArray::contains`
- **Description**: Checks if the array contains a value for the specified index.
- **Parameters**:
  - `index`: The index to check.
- **Returns**: A boolean indicating if the value exists.
- **Usage**: Use this method to verify the presence of a value at a specific index.

### `SparseArray::get`
- **Description**: Returns a reference to the value at a specified index.
- **Parameters**:
  - `index`: The index of the value to retrieve.
- **Returns**: An optional reference to the value.
- **Usage**: Use this method to access a value safely.

### `SparseArray::insert`
- **Description**: Inserts a value at a specified index in the array.
- **Parameters**:
  - `index`: The index to insert the value at.
  - `value`: The value to insert.
- **Usage**: Call this method to add a new value to the sparse array.

### `ComponentSparseSet::new`
- **Description**: Creates a new `ComponentSparseSet` with a specified component type layout and initial capacity.
- **Parameters**:
  - `component_info`: Information about the component type.
  - `capacity`: The initial capacity of the sparse set.
- **Returns**: A new instance of `ComponentSparseSet`.
- **Usage**: Call this method to initialize a sparse set for a specific component type.

### `ComponentSparseSet::insert`
- **Description**: Inserts an entity and component value pair into the sparse set.
- **Parameters**:
  - `entity`: The entity to associate with the component.
  - `value`: The value of the component to insert.
  - `change_tick`: The tick associated with the change.
- **Usage**: Call this method to add a component to an entity in the sparse set.

### `SparseSets::len`
- **Description**: Returns the number of `ComponentSparseSet`s in the collection.
- **Returns**: The number of sparse sets.
- **Usage**: Use this method to check how many component types are managed.

### `SparseSets::get`
- **Description**: Gets a reference to the `ComponentSparseSet` of a `ComponentId`.
- **Parameters**:
  - `component_id`: The ID of the component to retrieve.
- **Returns**: An optional reference to the sparse set.
- **Usage**: Use this method to access a specific component's sparse set.

### `SparseSets::get_or_insert`
- **Description**: Gets a mutable reference to a `ComponentSparseSet`, creating it if it does not exist.
- **Parameters**:
  - `component_info`: Information about the component type.
- **Returns**: A mutable reference to the sparse set.
- **Usage**: Call this method to ensure a sparse set exists for a component type.

## Example Usage

### Creating and Using Sparse Sets
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::storage::SparseSets;

fn main() {
    let mut world = World::new();
    let mut sparse_sets = SparseSets::default();

    // Initialize a component
    let component_info = ComponentInfo::new(ComponentId::new(1), ComponentDescriptor::new::<MyComponent>());
    sparse_sets.get_or_insert(&component_info);

    // Access a component sparse set
    if let Some(sparse_set) = sparse_sets.get(ComponentId::new(1)) {
        // Use the sparse set...
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `storage/sparse_set` module of the `bevy_ecs` library to manage sparse data structures in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.