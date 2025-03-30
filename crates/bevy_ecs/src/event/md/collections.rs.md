# Bevy Event Collections Documentation

This document provides a comprehensive overview of the public API available in the `event/collections` module of the `bevy_ecs` library. It includes details on traits, structs, and their usage that can be utilized to manage event collections in a Bevy application or game.

## Structs

### `Events<E: Event>`
- **Description**: A collection that represents events that occurred within the last two `Events::update` calls.
- **Key Points**:
  - Events can be written to using an `EventWriter` and read using an `EventReader`.
  - Supports parallel consumption of events by multiple systems, with consumption tracked on a per-system basis.
  - Events persist across a single frame boundary, and if not handled by the end of the frame, they will be dropped silently.
- **Fields**:
  - `events_a`: Holds the oldest still active events.
  - `events_b`: Holds the newer events.
  - `event_count`: The total number of events sent.
- **Methods**:
  - `fn oldest_event_count(&self) -> usize`: Returns the index of the oldest event stored in the event buffer.
  - `fn send(&mut self, event: E) -> EventId<E>`: Sends an event to the current event buffer and returns its ID.
  - `fn send_batch(&mut self, events: impl IntoIterator<Item = E>) -> SendBatchIds<E>`: Sends a list of events all at once, returning their IDs.
  - `fn send_default(&mut self) -> EventId<E>`: Sends the default value of the event.
  - `fn get_cursor(&self) -> EventCursor<E>`: Gets a new cursor that includes all events already in the event buffers.
  - `fn update(&mut self)`: Swaps the event buffers and clears the oldest event buffer, typically called once per frame/update.
  - `fn update_drain(&mut self) -> impl Iterator<Item = E> + '_`: Swaps the event buffers and drains the oldest event buffer, returning an iterator of all removed events.
  - `fn clear(&mut self)`: Removes all events from the collection.
  - `fn len(&self) -> usize`: Returns the number of events currently stored in the event buffer.
  - `fn is_empty(&self) -> bool`: Returns true if there are no events currently stored.
  - `fn iter_current_update_events(&self) -> impl ExactSizeIterator<Item = &E>`: Iterates over events that happened since the last `update` call.

### `EventSequence<E: Event>`
- **Description**: A struct that holds a sequence of events.
- **Fields**:
  - `events`: A vector of `EventInstance<E>`, which contains the events.
  - `start_event_count`: The starting count of events in this sequence.
- **Methods**:
  - Implements `Deref` and `DerefMut` traits to allow access to the underlying vector of events.

### `SendBatchIds<E>`
- **Description**: An iterator over sent `EventIds` from a batch.
- **Fields**:
  - `last_count`: The last count of events sent.
  - `event_count`: The total count of events sent.
- **Methods**:
  - `fn next(&mut self) -> Option<Self::Item>`: Returns the next `EventId` in the batch.
  - `fn len(&self) -> usize`: Returns the number of remaining `EventIds` in the batch.

## Example Usage

### Defining an Event
```rust
use bevy_ecs::event::{Event, Events};

#[derive(Event)]
struct MyEvent {
    value: usize,
}

// Setup
let mut events = Events::<MyEvent>::default();
events.send(MyEvent { value: 1 });
```

### Sending and Reading Events
```rust
// Sending an event
events.send(MyEvent { value: 2 });

// Reading events
let cursor = events.get_cursor();
for event in cursor.read(&events) {
    println!("Received event with value: {}", event.value);
}
```

### Updating Events
```rust
// Call this once per frame/update
events.update();
```

### Sending a Batch of Events
```rust
let batch = vec![MyEvent { value: 3 }, MyEvent { value: 4 }];
events.send_batch(batch);
```

This documentation serves as a comprehensive guide for developers looking to utilize the `event/collections` module of the `bevy_ecs` library to manage event collections in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.