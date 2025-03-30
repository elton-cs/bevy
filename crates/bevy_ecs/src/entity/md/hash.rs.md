# Bevy Hash Documentation

This document provides a comprehensive overview of the public API available in the `hash` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized for hashing entities and managing collections in a Bevy application or game.

## Structs

### `EntityHash`
- **Description**: A `BuildHasher` that results in an `EntityHasher`.
- **Fields**: None.
- **Methods**:
  - `build_hasher(&self) -> Self::Hasher`: Creates a new instance of `EntityHasher`.

### `EntityHasher`
- **Description**: A fast hash designed to work specifically with generational indices like `Entity`.
- **Fields**:
  - `hash`: A `u64` value representing the hash.
- **Methods**:
  - `finish(&self) -> u64`: Returns the final hash value.
  - `write(&mut self, _bytes: &[u8])`: Panics if attempting to hash non-u64 fields.
  - `write_u64(&mut self, bits: u64)`: Writes a `u64` value to the hasher, using a specific hashing strategy optimized for entity IDs.

### `EntityHashMap<V>`
- **Description**: A `HashMap` pre-configured to use `EntityHash` for hashing.
- **Usage**: This type can be used to create hash maps where the keys are `Entity` types, allowing for efficient lookups and storage of values associated with entities.

### `EntityHashSet`
- **Description**: A `HashSet` pre-configured to use `EntityHash` for hashing.
- **Usage**: This type can be used to create sets of `Entity` types, allowing for efficient membership testing and storage of unique entities.

## Example Usage

### Using EntityHashMap
```rust
use bevy_ecs::entity::Entity;
use bevy_ecs::entity::hash::{EntityHashMap};

fn main() {
    let mut entity_map: EntityHashMap<i32> = EntityHashMap::new();
    let entity = Entity::from_raw(1);
    entity_map.insert(entity, 42);
    
    if let Some(value) = entity_map.get(&entity) {
        println!("Value for entity {:?} is {}", entity, value);
    }
}
```

### Using EntityHashSet
```rust
use bevy_ecs::entity::Entity;
use bevy_ecs::entity::hash::{EntityHashSet};

fn main() {
    let mut entity_set: EntityHashSet = EntityHashSet::new();
    let entity = Entity::from_raw(1);
    entity_set.insert(entity);
    
    if entity_set.contains(&entity) {
        println!("Entity {:?} is in the set", entity);
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `hash` module of the `bevy_ecs` library to manage entity hashing in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.