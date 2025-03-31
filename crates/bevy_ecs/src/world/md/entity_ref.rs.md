# Bevy ECS Entity Reference Module Documentation

This document provides a comprehensive overview of the public API available in the `world/entity_ref` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to manage entity references in an ECS-based application.

## Overview

- **Purpose**: This module defines types and traits that facilitate read-only and mutable access to entity components, allowing for safe manipulation of entity data.

## Structs

### `EntityRef<'w>`
- **Description**: A read-only reference to a particular `Entity` and all of its components.
- **Fields**:
  - `UnsafeEntityCell<'w>`: A cell that provides unsafe access to the entity's components.
- **Methods**:
  - `new`: Creates a new `EntityRef` from an `UnsafeEntityCell`.
  - `id`: Returns the ID of the current entity.
  - `location`: Gets metadata indicating where the current entity is stored.
  - `archetype`: Returns the archetype that the current entity belongs to.
  - `contains<T: Component>`: Checks if the current entity has a component of type `T`.
  - `get<T: Component>`: Gets access to the component of type `T` for the current entity.
  - `get_change_ticks<T: Component>`: Retrieves the change ticks for the given component.
  - `get_by_id`: Returns untyped read-only reference(s) to component(s) based on the given `ComponentId`s.

### `EntityMut<'w>`
- **Description**: Provides mutable access to a single entity and all of its components.
- **Fields**:
  - `UnsafeEntityCell<'w>`: A cell that provides unsafe access to the entity's components.
- **Methods**:
  - `new`: Creates a new `EntityMut` from an `UnsafeEntityCell`.
  - `id`: Returns the ID of the current entity.
  - `get_mut<T: Component>`: Gets mutable access to the component of type `T` for the current entity.
  - `insert<T: Bundle>`: Adds a bundle of components to the entity.
  - `remove<T: Bundle>`: Removes all components in the specified bundle from the entity.
  - `despawn`: Despawns the current entity.

### `EntityWorldMut<'w>`
- **Description**: A mutable reference to a particular `Entity` and the entire world.
- **Fields**:
  - `world`: A mutable reference to the `World`.
  - `entity`: The ID of the current entity.
  - `location`: Metadata indicating where the current entity is stored.
- **Methods**:
  - `new`: Creates a new `EntityWorldMut` from a mutable reference to a `World`, an `Entity`, and its location.
  - `id`: Returns the ID of the current entity.
  - `get_mut<T: Component>`: Gets mutable access to the component of type `T` for the current entity.
  - `insert<T: Bundle>`: Adds a bundle of components to the entity.
  - `remove_by_id`: Removes a component from the entity by its `ComponentId`.
  - `despawn`: Despawns the current entity.

### `FilteredEntityRef<'w>`
- **Description**: Provides read-only access to a single entity and its components, with certain components excluded.
- **Fields**:
  - `entity`: An `UnsafeEntityCell` reference to the entity.
  - `access`: An `Access<ComponentId>` that defines which components can be accessed.
- **Methods**:
  - `new`: Creates a new `FilteredEntityRef` from an `UnsafeEntityCell` and an `Access`.
  - `get<T: Component>`: Gets access to the component of type `T` for the current entity.
  - `contains<T: Component>`: Checks if the current entity has a component of type `T`.

### `FilteredEntityMut<'w>`
- **Description**: Provides mutable access to a single entity and its components, with certain components excluded.
- **Fields**:
  - `entity`: An `UnsafeEntityCell` reference to the entity.
  - `access`: An `Access<ComponentId>` that defines which components can be accessed.
- **Methods**:
  - `new`: Creates a new `FilteredEntityMut` from an `UnsafeEntityCell` and an `Access`.
  - `get_mut<T: Component>`: Gets mutable access to the component of type `T` for the current entity.
  - `contains<T: Component>`: Checks if the current entity has a component of type `T`.

## Traits

### `DynamicComponentFetch`
- **Description**: A trait that defines types that can be used to fetch components from an entity dynamically by `ComponentId`s.
- **Associated Types**:
  - `Ref<'w>`: The read-only reference type returned by `fetch_ref`.
  - `Mut<'w>`: The mutable reference type returned by `fetch_mut`.
- **Methods**:
  - `fetch_ref`: Returns untyped read-only reference(s) to the component(s) with the given `ComponentId`s.
  - `fetch_mut`: Returns untyped mutable reference(s) to the component(s) with the given `ComponentId`s.

## Example Usage

### Fetching Components
```rust
use bevy_ecs::prelude::*;

fn example_system(entity: Entity, world: &mut World) {
    let entity_ref: EntityRef = world.entity(entity).into();
    if let Some(component) = entity_ref.get::<MyComponent>() {
        // Use the component
    }
}

fn example_mut_system(entity: Entity, world: &mut World) {
    let mut entity_mut: EntityMut = world.entity_mut(entity);
    if let Some(component) = entity_mut.get_mut::<MyComponent>() {
        component.value += 1;
    }
}
```

### Using FilteredEntityRef
```rust
fn filtered_system(query: Query<FilteredEntityRef<MyComponent>>) {
    for entity_ref in query.iter() {
        if let Some(component) = entity_ref.get::<MyComponent>() {
            // Process the component
        }
    }
}
```

### Using FilteredEntityMut
```rust
fn filtered_mut_system(query: Query<FilteredEntityMut<MyComponent>>) {
    for mut entity_mut in query.iter_mut() {
        if let Some(component) = entity_mut.get_mut::<MyComponent>() {
            component.value += 1;
        }
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `world/entity_ref` module of the `bevy_ecs` library to manage entity references in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.