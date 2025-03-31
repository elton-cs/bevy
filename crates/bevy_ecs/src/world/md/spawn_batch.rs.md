# Bevy ECS Spawn Batch Module Documentation

This document provides a comprehensive overview of the public API available in the `world/spawn_batch` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to spawn batches of entities in an ECS-based application.

## Overview

- **Purpose**: This module defines an iterator that spawns a series of entities and returns the IDs of each spawned entity, facilitating efficient entity creation in bulk.

## Structs

### `SpawnBatchIter<'w, I>`
- **Description**: An iterator that spawns a series of entities and returns the `Entity` ID of each spawned entity.
- **Fields**:
  - `inner`: The inner iterator that produces bundles to spawn.
  - `spawner`: A `BundleSpawner<'w>` that handles the spawning of bundles.
  - `caller`: A reference to the location of the caller, used for tracking change detection (enabled with the `track_change_detection` feature).
- **Methods**:
  - `new`: Creates a new `SpawnBatchIter` instance.
    - **Parameters**:
      - `world`: A mutable reference to the `World`.
      - `iter`: The iterator producing bundles to spawn.
      - `caller`: The location of the caller (if tracking change detection).
    - **Usage**: This method initializes the iterator and prepares it for spawning entities.

```rust
let spawn_iter = SpawnBatchIter::new(&mut world, bundle_iterator, caller);
```

## Traits Implementations

### `Iterator`
- **Description**: The `Iterator` trait is implemented for `SpawnBatchIter`, allowing it to be used as a standard iterator.
- **Methods**:
  - `next`: Retrieves the next `Entity` ID from the iterator, spawning the corresponding bundle.
    - **Returns**: An `Option<Entity>` representing the next spawned entity or `None` if there are no more entities to spawn.
  - `size_hint`: Returns the size hint of the inner iterator.

### `ExactSizeIterator`
- **Description**: The `ExactSizeIterator` trait is implemented for `SpawnBatchIter`, allowing it to provide an exact length.
- **Methods**:
  - `len`: Returns the length of the inner iterator.

### `FusedIterator`
- **Description**: The `FusedIterator` trait is implemented for `SpawnBatchIter`, indicating that once the iterator returns `None`, it will not yield any more items.

## Example Usage

### Spawning Entities in a Batch
```rust
use bevy_ecs::prelude::*;

fn spawn_entities(world: &mut World) {
    let bundles = vec![BundleA {}, BundleB {}, BundleC {}]; // Example bundles
    let spawn_iter = SpawnBatchIter::new(world, bundles.into_iter(), caller);

    for entity in spawn_iter {
        println!("Spawned entity with ID: {:?}", entity);
    }
}
```

### Handling Remaining Entities on Drop
```rust
fn spawn_entities_with_drop(world: &mut World) {
    let bundles = vec![BundleA {}, BundleB {}, BundleC {}]; // Example bundles
    let spawn_iter = SpawnBatchIter::new(world, bundles.into_iter(), caller);

    // If the iterator is not fully exhausted, remaining entities will be spawned when dropped.
    // This can be useful for ensuring all entities are created even if the iterator is not fully consumed.
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `world/spawn_batch` module of the `bevy_ecs` library to manage the spawning of entities in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.