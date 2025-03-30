# Bevy Map Entities Documentation

This document provides a comprehensive overview of the public API available in the `map_entities` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to map entity references in a Bevy application or game.

## Traits

### `MapEntities`
- **Description**: A trait for types that contain `Entity` references and allow mapping these references to new values.
- **Key Points**:
  - This trait is essential for defining custom mappings for entity references when copying components from one world to another.
  - It is particularly useful for types like `HashSet<Entity>` and `EntityHashMap`, which must be rebuilt when their contained `Entity` references are remapped.
- **Key Methods**:
  - `map_entities<M: EntityMapper>(&mut self, entity_mapper: &mut M)`: Updates all `Entity` references stored inside using the provided `entity_mapper`.

### `EntityMapper`
- **Description**: A trait for types that know how to map an `Entity` into another `Entity`.
- **Key Points**:
  - This trait is used to define how to translate entity references between different worlds.
  - Typically implemented using an `EntityHashMap<Entity>` to map source entities to the current world's entities.
- **Key Methods**:
  - `map_entity(&mut self, entity: Entity) -> Entity`: Maps an entity to another entity.

## Structs

### `SceneEntityMapper<'m>`
- **Description**: A wrapper for `EntityHashMap<Entity>`, augmenting it with the ability to allocate new `Entity` references in a destination world.
- **Fields**:
  - `map`: A mutable reference to an `EntityHashMap<Entity>`.
  - `dead_start`: A base `Entity` used to allocate new references.
  - `generations`: The number of generations this mapper has allocated thus far.
- **Methods**:
  - `new(map: &'m mut EntityHashMap<Entity>, world: &mut World) -> Self`: Creates a new `SceneEntityMapper`, spawning a temporary base `Entity` in the provided `World`.
  - `finish(self, world: &mut World)`: Reserves the allocated references to dead entities within the world and frees the temporary base `Entity`.
  - `world_scope<R>(entity_map: &'m mut EntityHashMap<Entity>, world: &mut World, f: impl FnOnce(&mut World, &mut Self) -> R) -> R`: Creates a `SceneEntityMapper` and calls the provided function with it.

### `RemovedComponentEntity`
- **Description**: A wrapper around `Entity` for `RemovedComponents`.
- **Fields**:
  - `0`: The `Entity` that had a component removed.

### `RemovedComponentReader<T>`
- **Description**: A wrapper around an `EventCursor<RemovedComponentEntity>` for a specific component type.
- **Fields**:
  - `reader`: The event cursor for removed component events.
  - `marker`: A phantom data marker for the component type.

### `RemovedComponentEvents`
- **Description**: Stores the `RemovedComponents` event buffers for all types of components in a given world.
- **Fields**:
  - `event_sets`: A sparse set of events for removed components.
- **Methods**:
  - `new() -> Self`: Creates an empty storage buffer for component removal events.
  - `update(&mut self)`: Swaps the event buffers and clears the oldest event buffer.
  - `iter(&self)`: Returns an iterator over components and their entity events.
  - `get(&self, component_id: impl Into<ComponentId>)`: Gets the event storage for a given component.
  - `send(&mut self, component_id: impl Into<ComponentId>, entity: Entity)`: Sends a removal event for the specified component.

## Example Usage

### Implementing MapEntities for a Component
```rust
use bevy_ecs::entity::Entity;
use bevy_ecs::entity::MapEntities;

#[derive(Component)]
struct Spring {
    a: Entity,
    b: Entity,
}

impl MapEntities for Spring {
    fn map_entities<M: EntityMapper>(&mut self, entity_mapper: &mut M) {
        self.a = entity_mapper.map_entity(self.a);
        self.b = entity_mapper.map_entity(self.b);
    }
}
```

### Creating a Simple Entity Mapper
```rust
use bevy_ecs::entity::{Entity, EntityMapper};
use bevy_ecs::entity::EntityHashMap;

pub struct SimpleEntityMapper {
    map: EntityHashMap<Entity>,
}

impl EntityMapper for SimpleEntityMapper {
    fn map_entity(&mut self, entity: Entity) -> Entity {
        self.map.get(&entity).copied().unwrap_or(entity)
    }
}
```

### Using SceneEntityMapper
```rust
use bevy_ecs::entity::{Entity, SceneEntityMapper};
use bevy_ecs::entity::EntityHashMap;
use bevy_ecs::world::World;

fn main() {
    let mut world = World::new();
    let mut entity_map = EntityHashMap::default();

    SceneEntityMapper::world_scope(&mut entity_map, &mut world, |_, mapper| {
        let entity = Entity::from_raw(1);
        mapper.map_entity(entity);
    });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `map_entities` module of the `bevy_ecs` library to manage entity mappings in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.