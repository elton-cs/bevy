# Bevy Event Registry Documentation

This document provides a comprehensive overview of the public API available in the `event/registry` module of the `bevy_ecs` library. It includes details on structs, methods, and their usage that can be utilized to manage event registration and updates in a Bevy application or game.

## Structs

### `RegisteredEvent`
- **Description**: A struct that holds information about a registered event type, including its component ID and update function.
- **Fields**:
  - `component_id`: The `ComponentId` associated with the event type.
  - `previously_updated`: A boolean indicating whether the event has been updated in the previous frame.
  - `update`: A function pointer that updates the events of the registered type.

### `EventRegistry`
- **Description**: A registry of all the `Events` in the `World`, used to update all events during the event update system.
- **Key Points**:
  - Manages the registration and deregistration of event types in the ECS world.
  - Tracks whether events should be updated each frame.
- **Fields**:
  - `should_update`: Indicates whether the events should be updated.
  - `event_updates`: A vector of `RegisteredEvent` instances.
- **Methods**:
  - `fn register_event<T: Event>(world: &mut World)`: Registers an event type to be updated in a given `World`. If no instance of `EventRegistry` exists, it adds one.
  - `fn run_updates(&mut self, world: &mut World, last_change_tick: Tick)`: Updates all registered events in the world based on their change detection.
  - `fn deregister_events<T: Event>(world: &mut World)`: Removes an event from the world and its associated `EventRegistry`.

### `ShouldUpdateEvents`
- **Description**: An enum that controls whether or not the events in an `EventRegistry` should be updated.
- **Variants**:
  - `Always`: Events should always be updated each frame.
  - `Waiting`: Events should wait until at least one pass of the fixed update schedules to update.
  - `Ready`: Events are ready to be updated after at least one pass of the fixed update schedules.

## Example Usage

### Registering an Event
```rust
use bevy_ecs::prelude::*;

#[derive(Event)]
struct MyEvent;

fn setup(world: &mut World) {
    EventRegistry::register_event::<MyEvent>(world);
}
```

### Running Event Updates
```rust
fn update_events(world: &mut World) {
    let mut registry = world.get_resource_mut::<EventRegistry>().unwrap();
    let last_change_tick = world.get_resource::<Tick>().unwrap();
    registry.run_updates(world, last_change_tick);
}
```

### Deregistering an Event
```rust
fn cleanup(world: &mut World) {
    EventRegistry::deregister_events::<MyEvent>(world);
}
```

### Checking Event Update Status
```rust
fn check_event_updates(registry: &EventRegistry) {
    match registry.should_update {
        ShouldUpdateEvents::Always => {
            println!("Events will always be updated.");
        }
        ShouldUpdateEvents::Waiting => {
            println!("Events are waiting to be updated.");
        }
        ShouldUpdateEvents::Ready => {
            println!("Events are ready to be updated.");
        }
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `event/registry` module of the `bevy_ecs` library to manage event registration and updates in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.