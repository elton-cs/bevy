# Bevy ECS Reflect Map Entities Module Documentation

This document provides a comprehensive overview of the public API available in the `reflect/map_entities` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to map entity references in a Bevy application or game.

## Overview

- **Purpose**: This module provides functionality for mapping fields of type `Entity` to a new world during deserialization. It ensures that entity IDs are valid for the world they originate from and allows for the reallocation of IDs in the destination world.

## Structs

### `ReflectMapEntities`
- **Description**: A struct that facilitates the mapping of entities in a reflected value using an `EntityMapper`.
- **Key Points**:
  - Contains a function pointer for mapping entities in reflected values.
  - Designed to work with types that implement `FromReflect` and `MapEntities`.

## Implementations

### `ReflectMapEntities::map_entities`
- **Description**: A general method for remapping entities in a reflected value via an `EntityMapper`.
- **Parameters**:
  - `reflected`: A mutable reference to a reflected value that may contain entity fields.
  - `mapper`: A mutable reference to an `EntityMapper` that will handle the mapping of entities.
- **Panics**: Will panic if the type of the reflected value does not match the expected type.
- **Usage**: Call this method to remap entities in a reflected value when deserializing or transferring data between worlds.

### `FromType<C> for ReflectMapEntities`
- **Description**: Implementation of the `FromType` trait for `ReflectMapEntities`.
- **Usage**: Allows for the creation of a `ReflectMapEntities` instance for a specific type `C` that implements `FromReflect` and `MapEntities`.

## Example Usage

### Mapping Entities in a Reflected Value
```rust
use bevy_ecs::prelude::*;
use bevy_reflect::{PartialReflect};

#[derive(Reflect, FromReflect, MapEntities)]
struct MyComponent {
    entity_id: Entity,
}

fn map_entities_example(reflected: &mut dyn PartialReflect, mapper: &mut dyn EntityMapper) {
    let reflect_map_entities = ReflectMapEntities::from_type::<MyComponent>();
    reflect_map_entities.map_entities(reflected, mapper);
}
```

### Using ReflectMapEntities in Commands
```rust
fn insert_reflect_component(commands: &mut Commands, component: Box<dyn PartialReflect>, mapper: &mut dyn EntityMapper) {
    let reflect_map_entities = ReflectMapEntities::from_type::<MyComponent>();
    reflect_map_entities.map_entities(component.as_mut(), mapper);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `reflect/map_entities` module of the `bevy_ecs` library to manage entity mappings in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.