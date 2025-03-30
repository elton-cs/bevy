# Bevy Event Base Documentation

This document provides a comprehensive overview of the public API available in the `event` module of the `bevy_ecs` library. It includes details on traits, structs, and their usage that can be utilized to manage events in a Bevy application or game.

## Traits

### `Event`
- **Description**: A trait for types that represent events in the ECS.
- **Key Points**:
  - Events can be stored in an `Events<E>` resource.
  - They can be accessed using the `EventReader` and `EventWriter` system parameters.
  - Events must be thread-safe and implement the `Component` trait.
- **Key Methods**:
  - `type Traversal: Traversal`: The component that describes which entity to propagate this event to next when propagation is enabled.
  - `const AUTO_PROPAGATE: bool`: When true, this event will always attempt to propagate when triggered, without requiring a call to `Trigger::propagate`.

## Structs

### `EventId<E: Event>`
- **Description**: A struct that uniquely identifies an event stored in a specific world.
- **Fields**:
  - `id`: A `usize` that corresponds to the order in which each event was added to the world.
  - `_marker`: A phantom data marker for the event type.
- **Methods**:
  - `fmt(&self, f: &mut fmt::Formatter)`: Implements the `Debug` trait for formatting the event ID.
  - `eq(&self, other: &Self)`: Implements the `PartialEq` trait for comparing event IDs.
  - `cmp(&self, other: &Self)`: Implements the `Ord` trait for ordering event IDs.

### `EventInstance<E: Event>`
- **Description**: A struct that wraps an event and its associated ID.
- **Fields**:
  - `event_id`: The `EventId` associated with the event.
  - `event`: The actual event data of type `E`.

## Example Usage

### Defining an Event
```rust
use bevy_ecs::event::Event;

#[derive(Event)]
struct MyEvent {
    value: i32,
}
```

### Using EventReader and EventWriter
```rust
use bevy_ecs::event::{EventReader, EventWriter};

fn my_system(mut event_writer: EventWriter<MyEvent>) {
    event_writer.send(MyEvent { value: 42 });
}

fn react_to_event(mut event_reader: EventReader<MyEvent>) {
    for event in event_reader.iter() {
        println!("Received event with value: {}", event.value);
    }
}
```

### Triggering Events
```rust
use bevy_ecs::world::World;

fn trigger_event(world: &mut World) {
    let event_id = EventId::<MyEvent> { id: 1, _marker: PhantomData };
    // Trigger the event in the world
    world.trigger(event_id);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `event` module of the `bevy_ecs` library to manage events in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.