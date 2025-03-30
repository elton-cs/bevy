# Bevy Visit Entities Documentation

This document provides a comprehensive overview of the public API available in the `visit_entities` module of the `bevy_ecs` library. It includes details on traits and their usage that can be utilized to apply operations to entities in a Bevy application or game.

## Traits

### `VisitEntities`
- **Description**: A trait for types that can apply an operation to all contained `Entity` fields.
- **Key Points**:
  - This trait is implemented by default for types that implement `IntoIterator`.
  - It is useful for types that cannot produce an iterator for lifetime reasons, such as those involving internal mutexes.
- **Key Methods**:
  - `visit_entities<F: FnMut(Entity)>(&self, f: F)`: Applies the provided function `f` to all contained entities.
    - **Usage**: Implement this method to define how to visit and operate on entities within a custom type.

### `VisitEntitiesMut`
- **Description**: A trait for types that can apply an operation to mutable references to all contained entities.
- **Key Points**:
  - This trait extends `VisitEntities` and is implemented by default for types that implement `IntoIterator`.
  - It is useful for types that need to modify their contained entities.
- **Key Methods**:
  - `visit_entities_mut<F: FnMut(&mut Entity)>(&mut self, f: F)`: Applies the provided function `f` to mutable references of all contained entities.
    - **Usage**: Implement this method to define how to visit and modify entities within a custom type.

## Example Usage

### Implementing VisitEntities for a Custom Struct
```rust
use bevy_ecs::entity::Entity;
use bevy_ecs::entity::VisitEntities;

#[derive(Debug)]
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

### Implementing VisitEntitiesMut for a Custom Struct
```rust
use bevy_ecs::entity::{Entity, VisitEntitiesMut};

#[derive(Debug)]
struct MyMutableComponent {
    entities: Vec<Entity>,
}

impl VisitEntitiesMut for MyMutableComponent {
    fn visit_entities_mut<F: FnMut(&mut Entity)>(&mut self, mut f: F) {
        for entity in &mut self.entities {
            f(entity);
        }
    }
}
```

### Using VisitEntities in a System
```rust
use bevy_ecs::entity::{Entity, VisitEntities};

fn process_entities<T: VisitEntities>(component: &T) {
    component.visit_entities(|entity| {
        println!("Processing entity: {:?}", entity);
    });
}
```

### Using VisitEntitiesMut in a System
```rust
use bevy_ecs::entity::{Entity, VisitEntitiesMut};

fn modify_entities<T: VisitEntitiesMut>(component: &mut T) {
    component.visit_entities_mut(|entity| {
        // Modify the entity as needed
        println!("Modifying entity: {:?}", entity);
    });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `visit_entities` module of the `bevy_ecs` library to manage entity operations in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.