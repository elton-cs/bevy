# Bevy Access Module Documentation

This document provides a comprehensive overview of the public API available in the `access` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to manage access to components and resources in a Bevy application or game.

## Structs

### `FormattedBitSet<'a, T: SparseSetIndex>`
- **Description**: A wrapper struct to provide a more readable `Debug` representation of `FixedBitSet` when used to store `SparseSetIndex`.
- **Fields**:
  - `bit_set`: A reference to the `FixedBitSet`.
  - `_marker`: A phantom data marker for the type `T`.
- **Key Points**:
  - This struct helps in debugging by converting raw integer representations into meaningful component IDs.

### `Access<T: SparseSetIndex>`
- **Description**: Tracks read and write access to specific elements in a collection, ensuring soundness during system initialization and execution.
- **Fields**:
  - `component_read_and_writes`: A `FixedBitSet` of accessed components.
  - `component_writes`: A `FixedBitSet` of exclusively accessed components.
  - `resource_read_and_writes`: A `FixedBitSet` of accessed resources.
  - `resource_writes`: A `FixedBitSet` of exclusively accessed resources.
  - `component_read_and_writes_inverted`: A boolean indicating if access is inverted.
  - `component_writes_inverted`: A boolean indicating if write access is inverted.
  - `reads_all_resources`: A boolean indicating if all resources can be read.
  - `writes_all_resources`: A boolean indicating if all resources can be written.
  - `archetypal`: A `FixedBitSet` for components that affect query results.
- **Key Points**:
  - This struct is essential for managing access permissions in Bevy systems, ensuring that components and resources are accessed safely.

### `FilteredAccess<T: SparseSetIndex>`
- **Description**: A struct that records access permissions filtered by specific conditions (e.g., `With`, `Without`).
- **Fields**:
  - `access`: An instance of `Access<T>` representing the unfiltered access.
  - `required`: A `FixedBitSet` of required components.
  - `filter_sets`: A vector of `AccessFilters<T>` representing the filters applied.
- **Key Points**:
  - This struct allows for complex queries that can filter components and resources based on specific criteria.

### `AccessConflicts`
- **Description**: An enum that records how two accesses conflict with each other.
- **Variants**:
  - `All`: Indicates a conflict for all indices.
  - `Individual(FixedBitSet)`: Indicates a conflict for a subset of indices.
- **Key Points**:
  - This enum is used to determine if two access sets can coexist without conflicts.

## Implementations

### `Access` Methods
- **`new() -> Self`**:
  - **Description**: Creates an empty `Access` collection.
  
- **`add_component_read(&mut self, index: T)`**:
  - **Description**: Adds read access to the component given by `index`.
  
- **`add_component_write(&mut self, index: T)`**:
  - **Description**: Adds exclusive access to the component given by `index`.
  
- **`remove_component_read(&mut self, index: T)`**:
  - **Description**: Removes read access to the component given by `index`.
  
- **`remove_component_write(&mut self, index: T)`**:
  - **Description**: Removes write access to the component given by `index`.
  
- **`has_component_read(&self, index: T) -> bool`**:
  - **Description**: Returns `true` if this can access the component given by `index`.
  
- **`has_component_write(&self, index: T) -> bool`**:
  - **Description**: Returns `true` if this can exclusively access the component given by `index`.
  
- **`is_compatible(&self, other: &Access<T>) -> bool`**:
  - **Description**: Returns `true` if the access and `other` can be active at the same time.

### `FilteredAccess` Methods
- **`matches_everything() -> Self`**:
  - **Description**: Returns a `FilteredAccess` which has no access and matches everything.
  
- **`add_component_read(&mut self, index: T)`**:
  - **Description**: Adds access to the component given by `index`.
  
- **`add_component_write(&mut self, index: T)`**:
  - **Description**: Adds exclusive access to the component given by `index`.
  
- **`is_compatible(&self, other: &FilteredAccess<T>) -> bool`**:
  - **Description**: Returns `true` if this and `other` can be active at the same time.

### `FilteredAccessSet` Methods
- **`is_compatible(&self, other: &FilteredAccessSet<T>) -> bool`**:
  - **Description**: Returns `true` if this and `other` can be active at the same time.
  
- **`get_conflicts(&self, other: &FilteredAccessSet<T>) -> AccessConflicts`**:
  - **Description**: Returns a vector of elements that this set and `other` cannot access at the same time.
  
- **`add(&mut self, filtered_access: FilteredAccess<T>)`**:
  - **Description**: Adds the filtered access to the set.

## Example Usage

### Managing Access in a System
```rust
use bevy_ecs::prelude::*;

fn my_system(access: Access<MyComponent>) {
    if access.has_component_read(my_component_id) {
        // Perform read operations
    }
    if access.has_component_write(my_component_id) {
        // Perform write operations
    }
}
```

### Using Filtered Access
```rust
fn my_filtered_system(filtered_access: FilteredAccess<MyComponent>) {
    if filtered_access.access().has_component_read(my_component_id) {
        // Handle read access
    }
    if filtered_access.access().has_component_write(my_component_id) {
        // Handle write access
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `access` module of the `bevy_ecs` library to manage access to components and resources in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.