# Bevy ECS Reflect Visit Entities Module Documentation

This document provides a comprehensive overview of the public API available in the `reflect/visit_entities` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to apply operations to entities contained within reflected values in a Bevy application or game.

## Overview

- **Purpose**: This module provides functionality for applying operations to all entities contained within a reflected value. It allows for both immutable and mutable access to the entities, facilitating operations on them dynamically at runtime.

## Structs

### `ReflectVisitEntities`
- **Description**: A struct that facilitates applying an operation to all contained entities in a reflected value.
- **Key Points**:
  - Contains a function pointer for visiting entities in reflected components.
  
#### Methods

1. **`visit_entities`**
   - **Description**: A general method for applying an operation to all entities in a reflected component.
   - **Parameters**:
     - `component`: A reference to a reflected value that may contain entities.
     - `f`: A mutable reference to a function that takes an `Entity` as an argument.
   - **Usage**: Call this method to perform operations on all entities contained within a reflected component.

### `ReflectVisitEntitiesMut`
- **Description**: A struct that facilitates applying operations to mutable references of all contained entities in a reflected value.
- **Key Points**:
  - Contains a function pointer for visiting entities in reflected components with mutable access.

#### Methods

1. **`visit_entities`**
   - **Description**: A general method for applying an operation to all entities in a reflected component with mutable access.
   - **Parameters**:
     - `component`: A mutable reference to a reflected value that may contain entities.
     - `f`: A mutable reference to a function that takes a mutable reference to an `Entity` as an argument.
   - **Usage**: Call this method to perform operations on all entities contained within a reflected component, allowing for modifications.

## Implementations

### `FromType<C> for ReflectVisitEntities`
- **Description**: Implementation of the `FromType` trait for `ReflectVisitEntities`.
- **Usage**: Allows for the creation of a `ReflectVisitEntities` instance for a specific type `C` that implements `FromReflect` and `VisitEntities`.

### `FromType<C> for ReflectVisitEntitiesMut`
- **Description**: Implementation of the `FromType` trait for `ReflectVisitEntitiesMut`.
- **Usage**: Allows for the creation of a `ReflectVisitEntitiesMut` instance for a specific type `C` that implements `FromReflect` and `VisitEntitiesMut`.

## Example Usage

### Visiting Entities in a Reflected Value
```rust
use bevy_ecs::prelude::*;
use bevy_reflect::{PartialReflect};

#[derive(Reflect, FromReflect, VisitEntities)]
struct MyComponent {
    entity_id: Entity,
}

fn visit_entities_example(reflected: &dyn PartialReflect, f: &mut dyn FnMut(Entity)) {
    let reflect_visit_entities = ReflectVisitEntities::from_type::<MyComponent>();
    reflect_visit_entities.visit_entities(reflected, f);
}
```

### Using ReflectVisitEntities in Commands
```rust
fn apply_to_entities(commands: &mut Commands, component: &mut dyn PartialReflect) {
    let reflect_visit_entities = ReflectVisitEntities::from_type::<MyComponent>();
    reflect_visit_entities.visit_entities(component, &mut |entity| {
        // Perform operations on each entity
    });
}
```

### Using ReflectVisitEntitiesMut for Mutable Access
```rust
fn apply_to_entities_mut(commands: &mut Commands, component: &mut dyn PartialReflect) {
    let reflect_visit_entities_mut = ReflectVisitEntitiesMut::from_type::<MyComponent>();
    reflect_visit_entities_mut.visit_entities(component, &mut |entity| {
        // Perform mutable operations on each entity
    });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `reflect/visit_entities` module of the `bevy_ecs` library to manage entity operations in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.