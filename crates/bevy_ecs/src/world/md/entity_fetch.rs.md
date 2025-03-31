# Bevy ECS Entity Fetch Module Documentation

This document provides a comprehensive overview of the public API available in the `world/entity_fetch` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to fetch entity references from a `World`.

## Overview

- **Purpose**: This module defines types and traits that facilitate fetching entity references from the ECS `World`, allowing for safe access to entity data.

## Traits

### `WorldEntityFetch`
- **Description**: A trait that defines types that can be used to fetch `Entity` references from a `World`.
- **Associated Types**:
  - `Ref<'w>`: The read-only reference type returned by `fetch_ref`.
  - `Mut<'w>`: The mutable reference type returned by `fetch_mut`.
  - `DeferredMut<'w>`: The mutable reference type returned by `fetch_deferred_mut`, but without structural mutability.
- **Methods**:
  - `fetch_ref`: Returns read-only reference(s) to the entities with the given `Entity` IDs.
  - `fetch_mut`: Returns mutable reference(s) to the entities with the given `Entity` IDs.
  - `fetch_deferred_mut`: Returns mutable reference(s) to the entities with the given `Entity` IDs, but without structural mutability.
- **Usage**: Implement this trait for types that need to fetch entities from the world, ensuring safe access to their components.

## Structs

### `DeferredWorld`
- **Description**: A reference to a `World` that disallows structural ECS changes, such as initializing resources, registering components, or spawning entities.
- **Fields**:
  - `world`: An `UnsafeWorldCell` reference to the underlying world.
- **Usage**: Use `DeferredWorld` to perform read operations on the world without the risk of modifying its structure.

## Example Usage

### Fetching Entities
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

### Using WorldEntityFetch
```rust
fn fetch_entities_system(entities: &[Entity], mut deferred_world: DeferredWorld) {
    let mut entity_refs = deferred_world.fetch_ref(entities).unwrap();
    for entity_ref in entity_refs {
        // Perform operations on entity_ref
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `world/entity_fetch` module of the `bevy_ecs` library to manage entity fetching in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.