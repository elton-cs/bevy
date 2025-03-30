# Bevy Archetype Documentation

This document provides a comprehensive overview of the public API available in the `archetype` module of the `bevy_ecs` library. It includes details on structs, enums, and functions that can be utilized to manage archetypes in a Bevy application or game.

## Structs

### `ArchetypeRow`
- **Description**: An opaque location within an `Archetype`. This can be used in conjunction with `ArchetypeId` to find the exact location of an `Entity` within a `World`.
- **Fields**:
  - `0`: The index of the row within the archetype.
- **Methods**:
  - `new(index: usize) -> Self`: Creates a new `ArchetypeRow`.
  - `index(self) -> usize`: Gets the index of the row.
- **Usage**: Use this struct to track the position of entities within an archetype.

### `ArchetypeId`
- **Description**: An opaque unique ID for a single `Archetype` within a `World`.
- **Fields**:
  - `0`: The ID value of the archetype.
- **Methods**:
  - `new(index: usize) -> Self`: Creates a new `ArchetypeId` from a plain value.
  - `index(self) -> usize`: Gets the plain value of this `ArchetypeId`.
- **Usage**: Use this struct to uniquely identify archetypes within a world.

### `Archetype`
- **Description**: Metadata for a single archetype within a `World`.
- **Fields**:
  - `id`: The ID of the archetype.
  - `table_id`: The ID of the table associated with the archetype.
  - `entities`: A vector of entities contained in this archetype.
  - `components`: Information about the components in the archetype.
  - `flags`: Flags indicating the state of the archetype.
- **Methods**:
  - `new(...)`: Creates a new `Archetype` with specified components.
  - `id(&self) -> ArchetypeId`: Fetches the ID for the archetype.
  - `table_id(&self) -> TableId`: Fetches the archetype's table ID.
  - `entities(&self) -> &[ArchetypeEntity]`: Fetches the entities contained in this archetype.
  - `len(&self) -> usize`: Gets the total number of entities that belong to the archetype.
  - `is_empty(&self) -> bool`: Checks if the archetype has any entities.
  - `contains(&self, component_id: ComponentId) -> bool`: Checks if the archetype contains a specific component.
- **Usage**: Use this struct to manage and interact with archetypes in your application.

### `Edges`
- **Description**: The backing store of all `Archetype`s within a `World`.
- **Fields**:
  - `add_bundle`: Sparse array for tracking bundles being added.
  - `remove_bundle`: Sparse array for tracking bundles being removed.
  - `take_bundle`: Sparse array for tracking bundles being taken.
- **Methods**:
  - `get_add_bundle(&self, bundle_id: BundleId) -> Option<ArchetypeId>`: Checks the cache for the target archetype when adding a bundle.
  - `insert_add_bundle(...)`: Caches the target archetype when adding a bundle.
  - `get_remove_bundle(&self, bundle_id: BundleId) -> Option<Option<ArchetypeId>>`: Checks the cache for the target archetype when removing a bundle.
  - `insert_remove_bundle(...)`: Caches the target archetype when removing a bundle.
- **Usage**: Use this struct to manage the relationships between archetypes and bundles.

## Example Usage

### Creating and Using Archetypes
```rust
use bevy_ecs::prelude::*;
use bevy_state::prelude::*;

fn main() {
    let mut world = World::new();
    let archetype_id = ArchetypeId::new(0);
    let archetype = Archetype::new(...); // Initialize with components
    world.insert_resource(archetype);
}
```

### Managing Entities in Archetypes
```rust
fn add_entity_to_archetype(entity: Entity, archetype: &mut Archetype) {
    let table_row = TableRow::new(...); // Create a new table row
    archetype.allocate(entity, table_row);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `archetype` module of the `bevy_ecs` library to manage archetypes in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.