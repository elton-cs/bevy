# Bevy Event Module Documentation

This document provides a comprehensive overview of the public API available in the `event` module of the `bevy_ecs` library. It includes details on structs, methods, and their usage that can be utilized to manage event handling in a Bevy application or game.

## Modules

### `base`
- **Description**: Contains the foundational types for events, including `Event` and `EventId`.
- **Key Types**:
  - `Event`: A trait for types that represent events in the ECS.
  - `EventId`: A struct that uniquely identifies an event stored in a specific world.

### `collections`
- **Description**: Provides the `Events` struct, which manages collections of events.
- **Key Types**:
  - `Events<E>`: A struct that represents a collection of events, allowing for sending and reading events.
  - `SendBatchIds<E>`: An iterator over the IDs of events sent in a batch.

### `event_cursor`
- **Description**: Contains the `EventCursor` struct, which tracks the state of event reading.
- **Key Types**:
  - `EventCursor<E>`: A struct that stores the state for reading events, allowing systems to track which events have been processed.

### `iterators`
- **Description**: Provides iterators for reading events.
- **Key Types**:
  - `EventIterator<E>`: An iterator that yields unread events from an `EventReader` or `EventCursor`.
  - `EventIteratorWithId<E>`: An iterator that yields unread events along with their IDs.
  - `EventParIter<E>`: A parallel iterator for processing events concurrently.

### `mut_iterators`
- **Description**: Provides mutable iterators for reading events.
- **Key Types**:
  - `EventMutIterator<E>`: An iterator that yields mutable unread events from an `EventMutator` or `EventCursor`.
  - `EventMutIteratorWithId<E>`: An iterator that yields mutable unread events along with their IDs.
  - `EventMutParIter<E>`: A parallel iterator for processing mutable events concurrently.

### `mutator`
- **Description**: Contains the `EventMutator` struct, which allows systems to read and modify events.
- **Key Types**:
  - `EventMutator<'w, 's, E>`: A struct that mutably reads events of type `E`, tracking which events have been read.

### `reader`
- **Description**: Contains the `EventReader` struct, which allows systems to read events in order.
- **Key Types**:
  - `EventReader<'w, 's, E>`: A struct that reads events of type `E`, tracking which events have been read.

### `registry`
- **Description**: Manages event registration and deregistration in the ECS world.
- **Key Types**:
  - `EventRegistry`: A struct that handles the registration of events in the ECS world.
  - `ShouldUpdateEvents`: An enum that determines whether events should be updated.

### `send_event`
- **Description**: Provides functionality for sending events within the ECS.
- **Key Types**:
  - `SendEvent`: A struct that encapsulates the logic for sending events.

### `update`
- **Description**: Contains functions related to updating events.
- **Key Types**:
  - `event_update_system`: A system that updates events.
  - `signal_event_update_system`: A system that signals when events should be updated.
  - `EventUpdates`: A struct that manages event updates.

### `writer`
- **Description**: Contains the `EventWriter` struct, which allows systems to send events.
- **Key Types**:
  - `EventWriter<E>`: A struct that allows systems to send events of type `E`.

## Example Usage

### Sending and Reading Events
```rust
use bevy_ecs::prelude::*;

#[derive(Event)]
struct MyEvent {
    value: usize,
}

fn my_system(mut events: ResMut<Events<MyEvent>>) {
    events.send(MyEvent { value: 42 });
}

fn read_events(mut reader: EventReader<MyEvent>) {
    for event in reader.read() {
        println!("Received event with value: {}", event.value);
    }
}
```

### Using EventMutator
```rust
fn my_mutator_system(mut mutator: EventMutator<MyEvent>) {
    for event in mutator.read() {
        event.value += 1; // Mutate the event
        println!("Updated event value: {}", event.value);
    }
}
```

### Parallel Event Processing
```rust
#[cfg(feature = "multi_threaded")]
fn process_events_parallel(mut events: EventReader<MyEvent>, counter: Res<Counter>) {
    events.par_read().for_each(|event| {
        counter.0.fetch_add(event.value, Ordering::Relaxed);
    });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `event` module of the `bevy_ecs` library to manage event handling in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.