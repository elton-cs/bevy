# Bevy ECS World Module Documentation

This document provides a comprehensive overview of the public API available in the `world` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to manage entities, components, and resources in an ECS-based application.

## Overview

- **Purpose**: This module defines the `World` struct and APIs for accessing and manipulating entities, components, and resources directly.

## Structs

### `World`
- **Description**: The core struct that stores and manages entities, components, resources, and their associated metadata.
- **Fields**:
  - `id: WorldId`: Unique identifier for the world.
  - `entities: Entities`: Collection of entities in the world.
  - `components: Components`: Collection of component types.
  - `archetypes: Archetypes`: Collection of archetypes for efficient entity management.
  - `storages: Storages`: Storage for components and resources.
  - `bundles: Bundles`: Collection of bundles for grouping components.
  - `observers: Observers`: Collection of observers for event handling.
  - `removed_components: RemovedComponentEvents`: Tracks removed components.
  - `change_tick: AtomicU32`: Tracks the current change tick.
  - `last_change_tick: Tick`: Tracks the last change tick.
  - `last_check_tick: Tick`: Tracks the last check tick.
  - `last_trigger_id: u32`: Tracks the last event trigger ID.
  - `command_queue: RawCommandQueue`: Queue for commands to be executed.

### `Command`
- **Description**: A trait for defining commands that can mutate the `World`.
- **Methods**:
  - `apply(self, world: &mut World)`: Applies the command to mutate the provided world.

## Public Methods of `World`

### `new()`
- **Description**: Creates a new empty `World`.
- **Panics**: If the maximum number of worlds has been created.

### `id(&self) -> WorldId`
- **Description**: Retrieves the unique ID of the world.

### `as_unsafe_world_cell(&mut self) -> UnsafeWorldCell<'_>`
- **Description**: Creates a new `UnsafeWorldCell` view with complete read+write access.

### `as_unsafe_world_cell_readonly(&self) -> UnsafeWorldCell<'_>`
- **Description**: Creates a new `UnsafeWorldCell` view with only read access.

### `entities(&self) -> &Entities`
- **Description**: Retrieves the `Entities` collection of the world.

### `entities_mut(&mut self) -> &mut Entities`
- **Description**: Retrieves the `Entities` collection mutably.
- **Safety**: Mutable reference must not put the `Entities` data in an invalid state.

### `archetypes(&self) -> &Archetypes`
- **Description**: Retrieves the `Archetypes` collection of the world.

### `components(&self) -> &Components`
- **Description**: Retrieves the `Components` collection of the world.

### `storages(&self) -> &Storages`
- **Description**: Retrieves the `Storages` collection of the world.

### `bundles(&self) -> &Bundles`
- **Description**: Retrieves the `Bundles` collection of the world.

### `removed_components(&self) -> &RemovedComponentEvents`
- **Description**: Retrieves the `RemovedComponentEvents` collection of the world.

### `commands(&mut self) -> Commands`
- **Description**: Creates a new `Commands` instance that writes to the world's command queue.

### `register_component<T: Component>(&mut self) -> ComponentId`
- **Description**: Registers a new `Component` type and returns the `ComponentId` created for it.

### `register_required_components<T: Component, R: Component + Default>(&mut self)`
- **Description**: Registers a component `R` as a required component for `T`.

### `get_entity<F: WorldEntityFetch>(&self, entities: F) -> Result<F::Ref<'_>, Entity>`
- **Description**: Retrieves `EntityRef`s for the given entities, returning an error if any do not exist.

### `get_entity_mut<F: WorldEntityFetch>(&mut self, entities: F) -> Result<F::Mut<'_>, EntityFetchError>`
- **Description**: Retrieves `EntityMut`s for the given entities, returning an error if any do not exist.

### `spawn_empty(&mut self) -> EntityWorldMut`
- **Description**: Spawns a new empty `Entity` and returns a corresponding `EntityWorldMut`.

### `spawn<B: Bundle>(&mut self, bundle: B) -> EntityWorldMut`
- **Description**: Spawns a new `Entity` with a given `Bundle` of components.

### `despawn(&mut self, entity: Entity) -> bool`
- **Description**: Despawns the given entity, if it exists.

### `clear_trackers(&mut self)`
- **Description**: Clears the internal component tracker state.

### `iter_entities(&self) -> impl Iterator<Item = EntityRef<'_>>`
- **Description**: Returns an iterator of entities that exposes read-only operations.

### `iter_entities_mut(&mut self) -> impl Iterator<Item = EntityMut<'_>>`
- **Description**: Returns a mutable iterator over all entities in the world.

### `iter_resources(&self) -> impl Iterator<Item = (&ComponentInfo, Ptr<'_>)>`
- **Description**: Iterates over all resources in the world.

### `iter_resources_mut(&mut self) -> impl Iterator<Item = (&ComponentInfo, MutUntyped<'_>)>`
- **Description**: Mutably iterates over all resources in the world.

## Example Usage

### Creating a World and Spawning Entities
```rust
use bevy_ecs::prelude::*;

fn main() {
    let mut world = World::new();
    let entity = world.spawn((Position { x: 0.0, y: 0.0 },)).id();
    // Accessing the entity
    let position = world.get::<Position>(entity).unwrap();
    println!("Entity Position: {:?}", position);
}
```

### Using Commands
```rust
fn increment_counter(world: &mut World) {
    let mut commands = world.commands();
    commands.queue(AddToCounter(1));
}
```

### Querying Entities
```rust
fn query_entities(world: &mut World) {
    let query = world.query::<&Position>();
    for (entity, position) in query.iter(&world) {
        println!("Entity {:?} has position {:?}", entity, position);
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `world` module of the `bevy_ecs` library to manage entities, components, and resources in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.