# Bevy Entity Observer Documentation

This document provides a comprehensive overview of the public API available in the `observer/entity_observer` module of the `bevy_ecs` library. It includes details on structs, methods, and their usage that can be utilized to manage entity observers in a Bevy application or game.

## Structs

### `ObservedBy`
- **Description**: A struct that tracks a list of entities that observe a specific entity. This is used to manage relationships between entities and their observers.
- **Fields**:
  - `0`: A vector of `Entity` instances representing the entities that are observing the entity this component is attached to.
- **Key Points**:
  - This struct is used in conjunction with the `Entity` component to manage observer relationships dynamically.
  - It is marked with `#[derive(Default)]`, allowing for easy instantiation with default values.

## Implementations

### `Component` Trait Implementation
- **Description**: Implements the `Component` trait for the `ObservedBy` struct, allowing it to be used as a component in the ECS.
- **Methods**:
  - `const STORAGE_TYPE: StorageType`: Specifies that the storage type for this component is `SparseSet`, which is suitable for components that may not be present on all entities.
  
  - `fn register_component_hooks(hooks: &mut ComponentHooks)`: Registers hooks for component lifecycle events.
    - **Parameters**:
      - `hooks`: A mutable reference to `ComponentHooks`, which allows for registering callbacks for component events.
    - **Functionality**:
      - This method defines a hook that is triggered when the `ObservedBy` component is removed from an entity. It handles the cleanup of observer relationships by:
        - Taking the list of entities that were observing the entity.
        - For each observing entity, it checks if the observer should be despawned based on the number of active sources.
        - If an observer has no more active sources, it is despawned from the world.

## Example Usage

### Creating and Using ObservedBy
```rust
use bevy_ecs::prelude::*;

#[derive(Default)]
struct MyObserver;

fn setup(mut commands: Commands) {
    let observer_entity = commands.spawn().id();
    let observed_entity = commands.spawn().insert(ObservedBy(vec![observer_entity])).id();
}

fn cleanup_observed_entities(mut commands: Commands, mut query: Query<(Entity, &ObservedBy)>) {
    for (entity, observed_by) in query.iter_mut() {
        // Logic to handle cleanup or processing of observed entities
        commands.entity(entity).remove::<ObservedBy>();
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `observer/entity_observer` module of the `bevy_ecs` library to manage entity observers in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.