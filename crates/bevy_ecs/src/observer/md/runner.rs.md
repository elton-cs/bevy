# Bevy Observer Runner Documentation

This document provides a comprehensive overview of the public API available in the `observer/runner` module of the `bevy_ecs` library. It includes details on structs, methods, and their usage that can be utilized to manage observer behavior in a Bevy application or game.

## Structs

### `ObserverState`
- **Description**: Contains information about an observer, defining how a given observer behaves. It serves as the "source of truth" for a given observer entity's behavior.
- **Fields**:
  - `descriptor`: An instance of `ObserverDescriptor` that holds the configuration for the observer.
  - `runner`: A function pointer of type `ObserverRunner` that defines the behavior of the observer when triggered.
  - `last_trigger_id`: A `u32` that tracks the last trigger ID to prevent duplicate processing.
  - `despawned_watched_entities`: A `u32` that counts how many watched entities have been despawned.
- **Key Points**:
  - This struct is essential for managing the lifecycle and behavior of observers in the ECS.
  - It is marked with `Default`, allowing for easy instantiation with default values.

## Implementations

### `Default` Trait Implementation
- **Description**: Implements the `Default` trait for `ObserverState`, allowing for the creation of a default instance.
- **Methods**:
  - `fn default() -> Self`: Returns a new instance of `ObserverState` with default values.

### `Component` Trait Implementation
- **Description**: Implements the `Component` trait for `ObserverState`, allowing it to be used as a component in the ECS.
- **Methods**:
  - `const STORAGE_TYPE: StorageType`: Specifies that the storage type for this component is `SparseSet`.
  
  - `fn register_component_hooks(hooks: &mut ComponentHooks)`: Registers hooks for component lifecycle events.
    - **Parameters**:
      - `hooks`: A mutable reference to `ComponentHooks`, which allows for registering callbacks for component events.
    - **Functionality**:
      - Defines hooks for when the `ObserverState` component is added or removed from an entity, managing the registration and unregistration of observers.

### `ObserverRunner`
- **Description**: A type alias for a function that is run when an observer is triggered.
- **Key Points**:
  - Typically refers to the default runner that executes the system stored in the associated `Observer` component.
  - Can be overridden for custom behavior.

## Functions

### `observer_system_runner`
- **Description**: A function that runs the observer system when triggered.
- **Parameters**:
  - `mut world`: A `DeferredWorld` that allows for deferred operations on the world.
  - `observer_trigger`: An instance of `ObserverTrigger` that contains information about the trigger event.
  - `ptr`: A pointer to the observer's data.
  - `propagate`: A mutable reference to a boolean that indicates whether the event should propagate.
- **Key Points**:
  - This function is responsible for executing the observer's logic when an event is triggered.
  - It ensures that the observer's state is updated and that the appropriate actions are taken based on the trigger.

## Example Usage

### Creating and Using an Observer
```rust
use bevy_ecs::prelude::*;

#[derive(Event)]
struct Speak {
    message: String,
}

fn setup(world: &mut World) {
    world.add_observer(|trigger: Trigger<Speak>| {
        println!("{}", trigger.event().message);
    });

    // Ensure observers are registered
    world.flush();
}

fn trigger_event(world: &mut World) {
    world.trigger(Speak {
        message: "Hello, Bevy!".into(),
    });
}
```

### Adding an Observer to an Entity
```rust
fn add_observer_to_entity(world: &mut World, entity: Entity) {
    let observer = Observer::new(|trigger: Trigger<Speak>| {
        println!("Observer triggered: {}", trigger.event().message);
    });
    world.entity_mut(entity).insert(observer);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `observer/runner` module of the `bevy_ecs` library to manage observer behavior in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.