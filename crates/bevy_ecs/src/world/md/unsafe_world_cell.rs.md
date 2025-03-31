# Bevy ECS Unsafe World Cell Module Documentation

This document provides a comprehensive overview of the public API available in the `world/unsafe_world_cell` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to manage disjoint mutable access to a `World` in an ECS-based application.

## Overview

- **Purpose**: This module contains types that allow for disjoint mutable access to a `World`, enabling safe concurrent access patterns in Bevy ECS.

## Structs

### `UnsafeWorldCell<'w>`
- **Description**: A variant of the `World` that allows mutable access to its components and resources while ensuring that aliasing violations are avoided.
- **Fields**:
  - `*mut World`: A raw pointer to the `World`.
  - `PhantomData<(&'w World, &'w UnsafeCell<World>)>`: A marker to indicate the lifetime of the world.
- **Methods**:
  - `new_readonly`: Creates a new `UnsafeWorldCell` for read-only access.
  - `new_mutable`: Creates a new `UnsafeWorldCell` for mutable access.
  - `world_mut`: Retrieves a mutable reference to the `World`.
    - **Safety**: The caller must ensure that no other mutable references exist at the same time.
  - `world`: Retrieves a read-only reference to the `World`.
    - **Safety**: The caller must ensure that no mutable references exist at the same time.
  - `id`: Retrieves the unique `WorldId` of the current world.
  - `entities`: Retrieves the `Entities` collection of the world.
  - `archetypes`: Retrieves the `Archetypes` collection of the world.
  - `components`: Retrieves the `Components` collection of the world.
  - `removed_components`: Retrieves the collection of removed components.
  - `get_entity`: Retrieves an `UnsafeEntityCell` for a specific entity.
  - `get_resource`: Gets a reference to a resource of the given type if it exists.
  - `get_resource_mut`: Gets a mutable reference to a resource of the given type if it exists.

### `UnsafeEntityCell<'w>`
- **Description**: A mutable reference to a particular `Entity` and all of its components.
- **Fields**:
  - `UnsafeWorldCell<'w>`: A reference to the world.
  - `Entity`: The ID of the current entity.
  - `EntityLocation`: Metadata indicating where the current entity is stored.
- **Methods**:
  - `new`: Creates a new `UnsafeEntityCell` from a world, entity, and location.
  - `id`: Returns the ID of the current entity.
  - `location`: Gets metadata indicating where the current entity is stored.
  - `archetype`: Returns the archetype that the current entity belongs to.
  - `contains<T: Component>`: Checks if the current entity has a component of type `T`.
  - `get<T: Component>`: Gets access to the component of type `T` for the current entity.
  - `get_change_ticks<T: Component>`: Retrieves the change ticks for the given component.

## Example Usage

### Using UnsafeWorldCell
```rust
use bevy_ecs::prelude::*;

fn example_system(world: &mut World) {
    let unsafe_world_cell = world.as_unsafe_world_cell();
    let entity = Entity::new(1);
    
    // SAFETY: Ensure no other mutable references exist
    let entity_cell = unsafe_world_cell.get_entity(entity).unwrap();
    if entity_cell.contains::<MyComponent>() {
        let component: &MyComponent = unsafe { entity_cell.get::<MyComponent>().unwrap() };
        // Use the component
    }
}
```

### Accessing Resources
```rust
fn access_resource(world: &mut World) {
    let unsafe_world_cell = world.as_unsafe_world_cell();
    
    // SAFETY: Ensure no mutable references exist
    if let Some(resource) = unsafe { unsafe_world_cell.get_resource::<MyResource>() } {
        // Use the resource
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `world/unsafe_world_cell` module of the `bevy_ecs` library to manage disjoint mutable access to a world in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.