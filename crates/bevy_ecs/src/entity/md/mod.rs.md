# Bevy Entity Handling Documentation

This document provides a comprehensive overview of the public API available in the `entity` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to manage entities in a Bevy application or game.

## Modules

### `map_entities`
- **Description**: Contains types and traits for mapping entities between different worlds.
- **Key Points**: 
  - Provides functionality to define custom mappings for entity references, which is essential when transferring data between worlds.

### `visit_entities`
- **Description**: Contains traits for applying operations to entities in a container.
- **Key Points**: 
  - Allows for operations to be performed on all entities contained within a type, facilitating entity management.

## Structs

### `Entity`
- **Description**: A lightweight identifier of an entity that exclusively owns zero or more component instances.
- **Fields**:
  - `index`: The index of the entity.
  - `generation`: A non-zero value representing the generation of the entity.
- **Methods**:
  - `from_raw_and_generation(index: u32, generation: NonZero<u32>) -> Entity`: Constructs an `Entity` from a raw index and generation.
  - `to_bits(self) -> u64`: Converts the entity to a bit representation for serialization or comparison.
  - `from_bits(bits: u64) -> Entity`: Reconstructs an `Entity` from its bit representation.
  - `index(self) -> u32`: Returns the index of the entity.
  - `generation(self) -> u32`: Returns the generation of the entity.

### `EntityMeta`
- **Description**: Metadata for an `Entity`, including its generation and location.
- **Fields**:
  - `generation`: The current generation of the entity.
  - `location`: The current location of the entity in an archetype.
- **Methods**:
  - `EMPTY`: Represents a pending entity with invalid metadata.

### `Entities`
- **Description**: A structure that manages the allocation and metadata of entities within a world.
- **Fields**:
  - `meta`: A vector of `EntityMeta` for tracking entity states.
  - `pending`: A vector of reserved entity IDs.
  - `free_cursor`: An atomic cursor for managing free entity IDs.
  - `len`: The count of currently allocated entities.
- **Methods**:
  - `new() -> Self`: Creates a new instance of `Entities`.
  - `reserve_entities(&self, count: u32) -> ReserveEntitiesIterator`: Reserves entity IDs concurrently.
  - `reserve_entity(&self) -> Entity`: Reserves a single entity ID.
  - `alloc(&mut self) -> Entity`: Allocates a new entity ID.
  - `clear(&mut self)`: Clears all entities from the world.

## Traits

### `EntityMapper`
- **Description**: A trait for types that know how to map an `Entity` into another `Entity`.
- **Key Methods**:
  - `map_entity(&mut self, entity: Entity) -> Entity`: Maps an entity to another entity.

### `VisitEntities`
- **Description**: A trait for types that can apply an operation to all contained entities.
- **Key Methods**:
  - `visit_entities<F: FnMut(Entity)>(&self, f: F)`: Applies the provided function to all contained entities.

### `VisitEntitiesMut`
- **Description**: A trait for types that can apply an operation to mutable references to all contained entities.
- **Key Methods**:
  - `visit_entities_mut<F: FnMut(&mut Entity)>(&mut self, f: F)`: Applies the provided function to mutable references of all contained entities.

## Example Usage

### Creating and Using Entities
```rust
use bevy_ecs::entity::{Entity, Entities};

fn main() {
    let mut world = World::new();
    let entity = world.spawn((A(1), B(2))).id();
    println!("Spawned entity: {:?}", entity);
}
```

### Implementing EntityMapper
```rust
use bevy_ecs::entity::{Entity, EntityMapper, EntityHashMap};

pub struct SimpleEntityMapper {
    map: EntityHashMap<Entity>,
}

impl EntityMapper for SimpleEntityMapper {
    fn map_entity(&mut self, entity: Entity) -> Entity {
        self.map.get(&entity).copied().unwrap_or(entity)
    }
}
```

### Using VisitEntities
```rust
use bevy_ecs::entity::{Entity, VisitEntities};

struct MyComponent {
    entities: Vec<Entity>,
}

impl VisitEntities for MyComponent {
    fn visit_entities<F: FnMut(Entity)>(&self, mut f: F) {
        for entity in &self.entities {
            f(*entity);
        }
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `entity` module of the `bevy_ecs` library to manage entities in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.