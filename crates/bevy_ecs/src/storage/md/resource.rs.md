# Bevy ECS Resource Module Documentation

This document provides a comprehensive overview of the public API available in the `storage/resource` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to manage resources in a Bevy application or game.

## Overview

- **Purpose**: This module provides the backing storage and metadata for resources within a `World`. It allows for efficient management of resources, including their lifecycle and access patterns.

## Structs

### `ResourceData<SEND>`
- **Description**: The type-erased backing storage and metadata for a single resource within a `World`.
- **Fields**:
  - `data`: A `ManuallyDrop<BlobVec>` that stores the resource data.
  - `added_ticks`: An `UnsafeCell` for tracking when the resource was added.
  - `changed_ticks`: An `UnsafeCell` for tracking when the resource was changed.
  - `type_name`: A string representing the name of the resource type.
  - `id`: The `ArchetypeComponentId` for the resource.
  - `origin_thread_id`: An optional `ThreadId` indicating the thread that created the resource.
  - `changed_by`: An optional `UnsafeCell` for tracking who changed the resource (if tracking is enabled).
- **Usage**: Used to manage the lifecycle and access of resources in the ECS.

### `Resources<SEND>`
- **Description**: The backing store for all resources stored in the `World`.
- **Fields**:
  - `resources`: A `SparseSet` that holds the resources indexed by their `ComponentId`.
- **Usage**: Used to manage and access resources within a `World`.

## Functions

### `ResourceData::validate_access`
- **Description**: Validates that access to non-`Send` resources is only done on the thread they were created from.
- **Usage**: Call this method to ensure thread safety when accessing resources.

### `ResourceData::is_present`
- **Description**: Returns true if the resource is populated.
- **Returns**: A boolean indicating if the resource is present.
- **Usage**: Use this method to check if a resource has been initialized.

### `ResourceData::id`
- **Description**: Gets the `ArchetypeComponentId` for the resource.
- **Returns**: The `ArchetypeComponentId` associated with the resource.
- **Usage**: Use this method to retrieve the ID of the resource.

### `ResourceData::get_data`
- **Description**: Returns a reference to the resource, if it exists.
- **Returns**: An optional pointer to the resource data.
- **Usage**: Use this method to access the resource data safely.

### `ResourceData::get_ticks`
- **Description**: Returns a reference to the resource's change ticks, if it exists.
- **Returns**: An optional `ComponentTicks` structure containing the ticks.
- **Usage**: Use this method to check the change state of the resource.

### `ResourceData::get_with_ticks`
- **Description**: Returns references to the resource and its change ticks, if it exists.
- **Returns**: An optional tuple containing a pointer to the resource, its ticks, and location information.
- **Usage**: Use this method to access both the resource and its change ticks.

### `ResourceData::get_mut`
- **Description**: Gets mutable access to a resource, if it exists.
- **Returns**: An optional mutable reference to the resource data.
- **Usage**: Use this method to modify the resource if it exists.

### `ResourceData::insert`
- **Description**: Inserts a value into the resource. If a value is already present, it will be replaced.
- **Parameters**:
  - `value`: An owning pointer to the value to insert.
  - `change_tick`: The tick associated with the change.
  - `caller`: An optional location for tracking change detection.
- **Usage**: Call this method to add or update a resource in the ECS.

### `ResourceData::remove`
- **Description**: Removes a value from the resource, if present.
- **Returns**: An optional tuple containing the removed value and its ticks.
- **Usage**: Use this method to remove a resource from the ECS.

### `ResourceData::remove_and_drop`
- **Description**: Removes a value from the resource and drops it.
- **Usage**: Call this method to safely remove and drop a resource.

### `Resources::len`
- **Description**: Returns the total number of resources stored in the `World`.
- **Returns**: The number of resources.
- **Usage**: Use this method to check how many resources are currently managed.

### `Resources::iter`
- **Description**: Iterates over all initialized resources.
- **Returns**: An iterator over tuples of `ComponentId` and `ResourceData`.
- **Usage**: Use this method to access all resources in the `World`.

### `Resources::is_empty`
- **Description**: Returns true if there are no resources stored in the `World`.
- **Returns**: A boolean indicating if the resources are empty.
- **Usage**: Use this method to check if the resource store is empty.

### `Resources::get`
- **Description**: Gets read-only access to a resource, if it exists.
- **Parameters**:
  - `component_id`: The ID of the resource to retrieve.
- **Returns**: An optional reference to the resource data.
- **Usage**: Use this method to access a resource without modifying it.

### `Resources::clear`
- **Description**: Clears all resources from the `World`.
- **Usage**: Call this method to remove all resources from the ECS.

### `Resources::get_mut`
- **Description**: Gets mutable access to a resource, if it exists.
- **Parameters**:
  - `component_id`: The ID of the resource to retrieve.
- **Returns**: An optional mutable reference to the resource data.
- **Usage**: Use this method to modify a resource if it exists.

### `Resources::initialize_with`
- **Description**: Fetches or initializes a new resource and returns its underlying column.
- **Parameters**:
  - `component_id`: The ID of the resource to initialize.
  - `components`: A reference to the components for validation.
  - `f`: A function to generate the `ArchetypeComponentId`.
- **Usage**: Call this method to ensure a resource is initialized and accessible.

### `Resources::check_change_ticks`
- **Description**: Checks the change ticks for all resources.
- **Parameters**:
  - `change_tick`: The tick to check against.
- **Usage**: Use this method to validate the change state of all resources.

## Example Usage

### Creating and Using Resources
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::storage::Resources;

fn main() {
    let mut world = World::new();
    let mut resources = Resources::<true>::default();

    // Initialize a resource
    resources.initialize_with(ComponentId::new(0), &world.components(), || {
        ArchetypeComponentId::new(0)
    });

    // Access a resource
    if let Some(resource) = resources.get(ComponentId::new(0)) {
        // Use the resource...
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `storage/resource` module of the `bevy_ecs` library to manage resources in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.