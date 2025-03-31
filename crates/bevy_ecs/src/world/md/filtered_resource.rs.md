# Bevy ECS Filtered Resource Module Documentation

This document provides a comprehensive overview of the public API available in the `world/filtered_resource` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to manage filtered access to resources in an ECS-based application.

## Overview

- **Purpose**: This module defines types and traits that facilitate read-only and mutable access to a set of resources defined by specific access rules, allowing for safe manipulation of resource data.

## Structs

### `FilteredResources<'w, 's>`
- **Description**: Provides read-only access to a set of `Resource`s defined by the contained `Access`.
- **Fields**:
  - `world`: An `UnsafeWorldCell<'w>` reference to the world.
  - `access`: A reference to the `Access<ComponentId>` that defines which resources can be accessed.
  - `last_run`: A tick indicating the last run of the system.
  - `this_run`: A tick indicating the current run of the system.
- **Methods**:
  - `new`: Creates a new `FilteredResources`.
  - `access`: Returns a reference to the underlying `Access`.
  - `has_read<R: Resource>`: Returns `true` if the `FilteredResources` has read access to the given resource.
  - `get<R: Resource>`: Gets a reference to the resource of the given type if it exists and access is granted.
  - `get_by_id`: Gets a pointer to the resource with the given `ComponentId` if it exists and access is granted.

### `FilteredResourcesMut<'w, 's>`
- **Description**: Provides mutable access to a set of `Resource`s defined by the contained `Access`.
- **Fields**:
  - `world`: An `UnsafeWorldCell<'w>` reference to the world.
  - `access`: A reference to the `Access<ComponentId>` that defines which resources can be accessed.
  - `last_run`: A tick indicating the last run of the system.
  - `this_run`: A tick indicating the current run of the system.
- **Methods**:
  - `new`: Creates a new `FilteredResourcesMut`.
  - `as_readonly`: Returns a read-only view of the resources.
  - `get<R: Resource>`: Gets a mutable reference to the resource of the given type if it exists and access is granted.
  - `get_mut<R: Resource>`: Gets mutable access to the resource of the given type if it exists and access is granted.

### `FilteredResourcesBuilder<'w>`
- **Description**: Builder struct to define the access for a `FilteredResources`.
- **Fields**:
  - `world`: A mutable reference to the `World`.
  - `access`: An `Access<ComponentId>` that defines which resources can be accessed.
- **Methods**:
  - `new`: Creates a new builder with no access.
  - `add_read<R: Resource>`: Adds access to read the resource of the given type.
  - `build`: Creates an `Access` that represents the accesses defined in the builder.

### `FilteredResourcesMutBuilder<'w>`
- **Description**: Builder struct to define the access for a `FilteredResourcesMut`.
- **Fields**:
  - `world`: A mutable reference to the `World`.
  - `access`: An `Access<ComponentId>` that defines which resources can be accessed.
- **Methods**:
  - `new`: Creates a new builder with no access.
  - `add_write<R: Resource>`: Adds access to write the resource of the given type.
  - `build`: Creates an `Access` that represents the accesses defined in the builder.

## Example Usage

### Using FilteredResources
```rust
use bevy_ecs::prelude::*;

fn resource_system(filtered: FilteredResources) {
    if let Some(resource) = filtered.get::<MyResource>() {
        // Use the resource
    }
}
```

### Using FilteredResourcesMut
```rust
fn resource_mut_system(mut filtered: FilteredResourcesMut) {
    if let Some(mut resource) = filtered.get_mut::<MyResource>() {
        resource.value += 1;
    }
}
```

### Building a System with FilteredResources
```rust
fn setup_system(world: &mut World) {
    let mut builder = FilteredResourcesParamBuilder::new(|b| {
        b.add_read::<ResourceA>().add_write::<ResourceB>();
    });

    let system = builder.build_state(world).build_system(resource_system);
    world.run_system_once(system);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `world/filtered_resource` module of the `bevy_ecs` library to manage filtered access to resources in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.