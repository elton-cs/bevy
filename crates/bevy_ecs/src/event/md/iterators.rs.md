# Bevy Event Iterators Documentation

This document provides a comprehensive overview of the public API available in the `event/iterators` module of the `bevy_ecs` library. It includes details on structs and their usage that can be utilized to manage event iteration in a Bevy application or game.

## Structs

### `EventIterator<'a, E: Event>`
- **Description**: An iterator that yields any unread events from an `EventReader` or `EventCursor`.
- **Key Points**:
  - It allows for easy access to events that have not yet been processed.
  - It wraps around `EventIteratorWithId` to provide a simpler interface without event IDs.
- **Methods**:
  - `fn next(&mut self) -> Option<Self::Item>`: Returns the next unread event.
  - `fn size_hint(&self) -> (usize, Option<usize>)`: Provides a hint about the number of remaining events.
  - `fn count(self) -> usize`: Returns the total number of unread events.
  - `fn last(self) -> Option<Self::Item>`: Returns the last unread event.
  - `fn nth(&mut self, n: usize) -> Option<Self::Item>`: Returns the nth unread event.

### `EventIteratorWithId<'a, E: Event>`
- **Description**: An iterator that yields any unread events (and their IDs) from an `EventReader` or `EventCursor`.
- **Key Points**:
  - It provides access to both the event and its associated ID, allowing for more detailed event handling.
  - It is useful for scenarios where the order of event processing is important.
- **Methods**:
  - `fn new(reader: &'a mut EventCursor<E>, events: &'a Events<E>) -> Self`: Creates a new iterator for unread events.
  - `fn without_id(self) -> EventIterator<'a, E>`: Converts the iterator to one that yields only events without IDs.
  - `fn next(&mut self) -> Option<Self::Item>`: Returns the next unread event along with its ID.
  - `fn size_hint(&self) -> (usize, Option<usize>)`: Provides a hint about the number of remaining events.
  - `fn count(self) -> usize`: Returns the total number of unread events.
  - `fn last(self) -> Option<Self::Item>`: Returns the last unread event along with its ID.
  - `fn nth(&mut self, n: usize) -> Option<Self::Item>`: Returns the nth unread event along with its ID.

### `EventParIter<'a, E: Event>` (Multi-threaded)
- **Description**: A parallel iterator over events that have not yet been seen by the reader.
- **Key Points**:
  - It allows for processing events in parallel, improving performance in multi-threaded scenarios.
  - The order of events is not guaranteed, making it suitable for scenarios where order does not matter.
- **Methods**:
  - `fn new(reader: &'a mut EventCursor<E>, events: &'a Events<E>) -> Self`: Creates a new parallel iterator for unread events.
  - `fn batching_strategy(mut self, strategy: BatchingStrategy) -> Self`: Changes the batching strategy used when iterating.
  - `fn for_each<FN: Fn(&'a E) + Send + Sync + Clone>(self, func: FN)`: Runs a closure for each unread event in parallel.
  - `fn for_each_with_id<FN: Fn(&'a E, EventId<E>) + Send + Sync + Clone>(mut self, func: FN)`: Runs a closure for each unread event in parallel, providing the event ID.
  - `fn len(&self) -> usize`: Returns the number of events to be iterated.
  - `fn is_empty(&self) -> bool`: Returns true if there are no events remaining in this iterator.

## Example Usage

### Using EventIterator to Read Events
```rust
use bevy_ecs::event::{Event, Events, EventIterator};

#[derive(Event)]
struct MyEvent {
    value: usize,
}

fn process_events(events: &mut Events<MyEvent>, cursor: &mut EventCursor<MyEvent>) {
    let mut iterator = EventIterator::new(cursor, events);
    while let Some(event) = iterator.next() {
        println!("Processing event with value: {}", event.value);
    }
}
```

### Using EventIteratorWithId to Read Events with IDs
```rust
use bevy_ecs::event::{Event, Events, EventIteratorWithId};

#[derive(Event)]
struct MyEvent {
    value: usize,
}

fn process_events_with_id(events: &mut Events<MyEvent>, cursor: &mut EventCursor<MyEvent>) {
    let mut iterator = EventIteratorWithId::new(cursor, events);
    while let Some((event, id)) = iterator.next() {
        println!("Processing event with ID: {:?} and value: {}", id, event.value);
    }
}
```

### Using EventParIter for Parallel Processing
```rust
#[cfg(feature = "multi_threaded")]
use bevy_ecs::event::{Event, Events, EventParIter};

#[derive(Event)]
struct MyEvent {
    value: usize,
}

#[cfg(feature = "multi_threaded")]
fn process_events_parallel(events: &mut Events<MyEvent>, cursor: &mut EventCursor<MyEvent>) {
    let mut iterator = EventParIter::new(cursor, events);
    iterator.for_each(|event| {
        println!("Processing event in parallel with value: {}", event.value);
    });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `event/iterators` module of the `bevy_ecs` library to manage event iteration in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.