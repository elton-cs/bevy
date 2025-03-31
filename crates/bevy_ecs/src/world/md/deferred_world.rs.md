# Bevy ECS Deferred World Module Documentation

This document provides a comprehensive overview of the public API available in the `world/deferred_world` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to manage a deferred reference to the ECS world, allowing for safe access without structural changes.

## Overview

- **Purpose**: This module provides a `DeferredWorld` struct that allows for accessing the `World` without permitting structural changes, ensuring safe read access to components and resources.

## Structs

### `DeferredWorld`
- **Description**: A reference to a `World` that disallows structural ECS changes, such as initializing resources, registering components, or spawning entities.
- **Fields**:
  - `world`: An `UnsafeWorldCell` reference to the underlying world.
- **Usage**: Use `DeferredWorld` to perform read operations on the world without the risk of modifying its structure.

## Functions

### `DeferredWorld::commands`
- **Description**: Creates a `Commands` instance that pushes to the world's command queue.
- **Returns**: A `Commands` instance for queuing commands.
- **Usage**: Use this method to obtain a `Commands` instance for modifying the world in a deferred manner.

### `DeferredWorld::get_mut`
- **Description**: Retrieves a mutable reference to the given entity's component of the specified type.
- **Parameters**:
  - `entity`: The entity to query.
- **Returns**: An `Option<Mut<T>>` containing a mutable reference to the component if it exists.
- **Usage**: Use this method to access and modify a specific component of an entity.

### `DeferredWorld::get_entity_mut`
- **Description**: Returns mutable references to the specified entities.
- **Parameters**:
  - `entities`: The entities to fetch.
- **Returns**: A result containing mutable references to the entities or an error if any do not exist.
- **Usage**: Use this method to fetch mutable references to multiple entities for modification.

### `DeferredWorld::resource_mut`
- **Description**: Gets a mutable reference to the resource of the given type.
- **Parameters**:
  - `R`: The resource type to query.
- **Returns**: A mutable reference to the resource.
- **Usage**: Use this method to modify a specific resource in the world.

### `DeferredWorld::get_resource_mut`
- **Description**: Gets a mutable reference to the resource of the given type if it exists.
- **Parameters**:
  - `R`: The resource type to query.
- **Returns**: An `Option<Mut<R>>` containing a mutable reference to the resource if it exists.
- **Usage**: Use this method to safely access a resource without panicking if it does not exist.

### `DeferredWorld::send_event`
- **Description**: Sends an event to the world.
- **Parameters**:
  - `event`: The event to send.
- **Returns**: An `Option<EventId<E>>` containing the ID of the sent event.
- **Usage**: Use this method to trigger events that observers can respond to.

### `DeferredWorld::trigger`
- **Description**: Sends a "global" trigger without any targets.
- **Parameters**:
  - `trigger`: The trigger to send.
- **Usage**: Use this method to trigger events that are not scoped to specific targets.

## Example Usage

### Using DeferredWorld in a System
```rust
use bevy_ecs::prelude::*;

fn example_system(mut deferred_world: DeferredWorld) {
    let entity = Entity::new(1);
    if let Some(mut component) = deferred_world.get_mut::<MyComponent>(entity) {
        component.value += 1;
    }
}
```

### Accessing Resources
```rust
fn resource_system(mut deferred_world: DeferredWorld) {
    if let Some(mut resource) = deferred_world.get_resource_mut::<MyResource>() {
        resource.value += 1;
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `world/deferred_world` module of the `bevy_ecs` library to manage deferred access to the ECS world in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.