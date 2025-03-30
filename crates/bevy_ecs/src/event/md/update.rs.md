# Bevy Event Update Documentation

This document provides a comprehensive overview of the public API available in the `event/update` module of the `bevy_ecs` library. It includes details on functions and their usage that can be utilized to manage event updates in a Bevy application or game.

## Structs

### `EventUpdates`
- **Description**: A struct used as a system set for organizing event update systems.
- **Key Points**:
  - It is used to group systems that are related to event updates, allowing for better management and execution order within the ECS.

## Functions

### `signal_event_update_system`
- **Description**: Signals the `event_update_system` to run after `FixedUpdate` systems.
- **Parameters**:
  - `signal`: An optional mutable reference to the `EventRegistry`.
- **Key Points**:
  - This function changes the behavior of the `EventRegistry` to only run after a fixed update cycle has passed.
  - It sets the `should_update` field of the `EventRegistry` to `Ready`, indicating that events are ready to be updated.

### `event_update_system`
- **Description**: A system that calls `Events::update` on all registered `Events` in the world.
- **Parameters**:
  - `world`: A mutable reference to the `World`.
  - `last_change_tick`: A local tick counter to track changes.
- **Key Points**:
  - This function updates all registered events in the world based on their change detection.
  - It modifies the `should_update` field of the `EventRegistry` to control future updates.

### `event_update_condition`
- **Description**: A run condition for the `event_update_system`.
- **Parameters**:
  - `maybe_signal`: An optional reference to the `EventRegistry`.
- **Returns**: A boolean indicating whether the event update system should run.
- **Key Points**:
  - If `signal_event_update_system` has been run at least once, it will wait for it to be run again before updating the events.
  - If no signal is present, it defaults to always updating the events.

## Example Usage

### Signaling Event Updates
```rust
fn signal_updates(world: &mut World) {
    let mut registry = world.get_resource_mut::<EventRegistry>().unwrap();
    signal_event_update_system(Some(registry));
}
```

### Running Event Updates
```rust
fn update_events(world: &mut World, last_change_tick: &mut Local<Tick>) {
    event_update_system(world, *last_change_tick);
}
```

### Checking Update Conditions
```rust
fn should_run_event_updates(maybe_signal: Option<Res<EventRegistry>>) -> bool {
    event_update_condition(maybe_signal)
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `event/update` module of the `bevy_ecs` library to manage event updates in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.