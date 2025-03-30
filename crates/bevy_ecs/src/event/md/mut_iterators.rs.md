# Bevy Mutable Event Iterators Documentation

This document provides a comprehensive overview of the public API available in the `event/mut_iterators` module of the `bevy_ecs` library. It includes details on structs and their usage that can be utilized to manage mutable event iteration in a Bevy application or game.

## Structs

### `EventMutIterator<'a, E: Event>`
- **Description**: An iterator that yields any unread events from an `EventMutator` or `EventCursor`.
- **Key Points**:
  - It allows for mutable access to events that have not yet been processed.
  - It wraps around `EventMutIteratorWithId` to provide a simpler interface without event IDs.
- **Methods**:
  - `fn next(&mut self) -> Option<Self::Item>`: Returns the next unread mutable event.
  - `fn size_hint(&self) -> (usize, Option<usize>)`: Provides a hint about the number of remaining events.
  - `fn count(self) -> usize`: Returns the total number of unread events.
  - `fn last(self) -> Option<Self::Item>`: Returns the last unread mutable event.
  - `fn nth(&mut self, n: usize) -> Option<Self::Item>`: Returns the nth unread mutable event.

### `EventMutIteratorWithId<'a, E: Event>`
- **Description**: An iterator that yields any unread events (and their IDs) from an `EventMutator` or `EventCursor`.
- **Key Points**:
  - It provides access to both the mutable event and its associated ID, allowing for more detailed event handling.
  - It is useful for scenarios where the order of event processing is important.
- **Methods**:
  - `fn new(mutator: &'a mut EventCursor<E>, events: &'a mut Events<E>) -> Self`: Creates a new iterator for unread mutable events.
  - `fn without_id(self) -> EventMutIterator<'a, E>`: Converts the iterator to one that yields only mutable events without IDs.
  - `fn next(&mut self) -> Option<Self::Item>`: Returns the next unread mutable event along with its ID.
  - `fn size_hint(&self) -> (usize, Option<usize>)`: Provides a hint about the number of remaining events.
  - `fn count(self) -> usize`: Returns the total number of unread mutable events.
  - `fn last(self) -> Option<Self::Item>`: Returns the last unread mutable event along with its ID.
  - `fn nth(&mut self, n: usize) -> Option<Self::Item>`: Returns the nth unread mutable event along with its ID.

### `EventMutParIter<'a, E: Event>` (Multi-threaded)
- **Description**: A parallel iterator over mutable events that have not yet been seen by the mutator.
- **Key Points**:
  - It allows for processing events in parallel, improving performance in multi-threaded scenarios.
  - The order of events is not guaranteed, making it suitable for scenarios where order does not matter.
- **Methods**:
  - `fn new(mutator: &'a mut EventCursor<E>, events: &'a mut Events<E>) -> Self`: Creates a new parallel iterator for unread mutable events.
  - `fn batching_strategy(mut self, strategy: BatchingStrategy) -> Self`: Changes the batching strategy used when iterating.
  - `fn for_each<FN: Fn(&'a mut E) + Send + Sync + Clone>(self, func: FN)`: Runs a closure for each unread mutable event in parallel.
  - `fn for_each_with_id<FN: Fn(&'a mut E, EventId<E>) + Send + Sync + Clone>(mut self, func: FN)`: Runs a closure for each unread mutable event in parallel, providing the event ID.
  - `fn len(&self) -> usize`: Returns the number of mutable events to be iterated.
  - `fn is_empty(&self) -> bool`: Returns true if there are no mutable events remaining in this iterator.

## Example Usage

### Using EventMutIterator to Read Mutable Events
```rust
use bevy_ecs::event::{Event, Events, EventMutIterator};

#[derive(Event)]
struct MyEvent {
    value: usize,
}

fn process_events(events: &mut Events<MyEvent>, cursor: &mut EventCursor<MyEvent>) {
    let mut iterator = EventMutIterator::new(cursor, events);
    while let Some(event) = iterator.next() {
        event.value += 1; // Mutate the event
        println!("Processing event with updated value: {}", event.value);
    }
}
```

### Using EventMutIteratorWithId to Read Mutable Events with IDs
```rust
use bevy_ecs::event::{Event, Events, EventMutIteratorWithId};

#[derive(Event)]
struct MyEvent {
    value: usize,
}

fn process_events_with_id(events: &mut Events<MyEvent>, cursor: &mut EventCursor<MyEvent>) {
    let mut iterator = EventMutIteratorWithId::new(cursor, events);
    while let Some((event, id)) = iterator.next() {
        event.value += 1; // Mutate the event
        println!("Processing event with ID: {:?} and updated value: {}", id, event.value);
    }
}
```

### Using EventMutParIter for Parallel Processing
```rust
#[cfg(feature = "multi_threaded")]
use bevy_ecs::event::{Event, Events, EventMutParIter};

#[derive(Event)]
struct MyEvent {
    value: usize,
}

#[cfg(feature = "multi_threaded")]
fn process_events_parallel(events: &mut Events<MyEvent>, cursor: &mut EventCursor<MyEvent>) {
    let mut iterator = EventMutParIter::new(cursor, events);
    iterator.for_each(|event| {
        event.value += 1; // Mutate the event in parallel
        println!("Processing event in parallel with updated value: {}", event.value);
    });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `event/mut_iterators` module of the `bevy_ecs` library to manage mutable event iteration in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.