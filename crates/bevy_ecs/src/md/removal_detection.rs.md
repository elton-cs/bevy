# Bevy Removal Detection Documentation

This document provides a comprehensive overview of the public API available in the `removal_detection` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to handle events related to component removals in a Bevy application or game.

## Structs

### `RemovedComponentEntity`
- **Description**: A wrapper around `Entity` for `RemovedComponents`.
- **Fields**:
  - `0`: The `Entity` that had a component removed.
- **Usage**: Used to represent an entity that has had a specific component removed, allowing systems to react to these events.

### `RemovedComponentReader<T>`
- **Description**: A wrapper around an `EventCursor<RemovedComponentEntity>` for a specific component type.
- **Fields**:
  - `reader`: The event cursor for removed component events.
  - `marker`: A phantom data marker for the component type.
- **Methods**:
  - `default() -> Self`: Creates a default instance of `RemovedComponentReader`.
  - `deref(&self) -> &EventCursor<RemovedComponentEntity>`: Dereferences to the underlying event cursor.
  - `deref_mut(&mut self) -> &mut EventCursor<RemovedComponentEntity>`: Dereferences mutably to the underlying event cursor.

### `RemovedComponentEvents`
- **Description**: Stores the `RemovedComponents` event buffers for all types of components in a given world.
- **Fields**:
  - `event_sets`: A sparse set of events for removed components.
- **Methods**:
  - `new() -> Self`: Creates an empty storage buffer for component removal events.
  - `update(&mut self)`: Swaps the event buffers and clears the oldest event buffer.
  - `iter(&self)`: Returns an iterator over components and their entity events.
  - `get(&self, component_id: impl Into<ComponentId>)`: Gets the event storage for a given component.
  - `send(&mut self, component_id: impl Into<ComponentId>, entity: Entity)`: Sends a removal event for the specified component.

### `RemovedComponents<'w, 's, T>`
- **Description**: A system parameter that yields entities that had their `T` component removed or have been despawned with it.
- **Fields**:
  - `component_id`: The `ComponentIdFor` for the specific component type.
  - `reader`: A local `RemovedComponentReader` for the component type.
  - `event_sets`: A reference to `RemovedComponentEvents`.
- **Methods**:
  - `reader(&self) -> &EventCursor<RemovedComponentEntity>`: Fetches the underlying event cursor.
  - `read(&mut self) -> RemovedIter<'_>`: Iterates over the events this `RemovedComponents` has not seen yet.
  - `len(&self) -> usize`: Determines the number of removal events available to be read.
  - `is_empty(&self) -> bool`: Returns `true` if there are no events available to read.
  - `clear(&mut self)`: Consumes all available events.

## Traits

### `DetectChanges`
- **Description**: A trait for types that can read change detection information.
- **Key Methods**:
  - `is_added(&self) -> bool`: Returns `true` if the value was added after the last system run.
  - `is_changed(&self) -> bool`: Returns `true` if the value was added or mutably dereferenced since the last system run.
  - `last_changed(&self) -> Tick`: Returns the change tick recording the last time this data was changed.

### `DetectChangesMut`
- **Description**: A trait for types that implement reliable change detection.
- **Key Methods**:
  - `set_changed(&mut self)`: Flags the value as having been changed.
  - `bypass_change_detection(&mut self) -> &mut Self::Inner`: Allows mutation without updating the change tick.

## Example Usage

### Using RemovedComponents in a System
```rust
use bevy_ecs::component::Component;
use bevy_ecs::removal_detection::RemovedComponents;

#[derive(Component)]
struct MyComponent;

fn react_on_removal(mut removed: RemovedComponents<MyComponent>) {
    removed.read().for_each(|removed_entity| {
        println!("Entity {:?} had MyComponent removed!", removed_entity);
    });
}
```

### Sending Removal Events
```rust
use bevy_ecs::component::Component;
use bevy_ecs::removal_detection::RemovedComponentEvents;

#[derive(Component)]
struct MyComponent;

fn remove_component_events(events: &mut RemovedComponentEvents) {
    let entity = Entity::from_raw(1);
    events.send(MyComponent::component_id(), entity);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `removal_detection` module of the `bevy_ecs` library to manage component removal events in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.