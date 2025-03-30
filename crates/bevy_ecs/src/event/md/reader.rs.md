# Bevy Event Reader Documentation

This document provides a comprehensive overview of the public API available in the `event/reader` module of the `bevy_ecs` library. It includes details on structs and their usage that can be utilized to manage event reading in a Bevy application or game.

## Structs

### `EventReader<'w, 's, E: Event>`
- **Description**: A struct that reads events of type `E` in order and tracks which events have already been read. This allows multiple systems to read the same events concurrently.
- **Key Points**:
  - Unlike `EventWriter<T>`, systems with `EventReader<T>` can be executed concurrently, but not with `EventWriter<T>` or `EventMutator<T>` systems for the same event type.
- **Fields**:
  - `reader`: A local state that tracks the cursor for the events.
  - `events`: A reference to the `Events<E>` resource containing the events.
- **Methods**:
  - `fn read(&mut self) -> EventIterator<'_, E>`: Iterates over the events this `EventReader` has not seen yet, marking them as read in the process.
  - `fn read_with_id(&mut self) -> EventIteratorWithId<'_, E>`: Similar to `read`, but also returns the `EventId` of the events.
  - `fn par_read(&mut self) -> EventParIter<'_, E>`: Returns a parallel iterator over the events this `EventReader` has not seen yet (requires multi-threading feature).
  - `fn len(&self) -> usize`: Determines the number of events available to be read without consuming any.
  - `fn is_empty(&self) -> bool`: Returns true if there are no events available to read.
  - `fn clear(&mut self)`: Consumes all available events, ensuring they will not appear in future reads.

## Example Usage

### Declaring an EventReader in a System
```rust
use bevy_ecs::prelude::*;

#[derive(Event, Debug)]
struct MyEvent {
    value: usize,
}

fn my_system(mut reader: EventReader<MyEvent>) {
    for event in reader.read() {
        println!("Received event with value: {}", event.value);
    }
}
```

### Checking for Events and Clearing Them
```rust
use bevy_ecs::prelude::*;

#[derive(Event)]
struct CollisionEvent;

fn play_collision_sound(mut events: EventReader<CollisionEvent>) {
    if !events.is_empty() {
        events.clear(); // Clear events to prevent re-triggering
        // Play a sound
    }
}
```

### Using Parallel Reading of Events
```rust
#[cfg(feature = "multi_threaded")]
use bevy_ecs::prelude::*;
#[cfg(feature = "multi_threaded")]
use std::sync::atomic::{AtomicUsize, Ordering};

#[derive(Event)]
struct MyEvent {
    value: usize,
}

#[cfg(feature = "multi_threaded")]
fn process_events_parallel(mut events: EventReader<MyEvent>, counter: Res<Counter>) {
    events.par_read().for_each(|MyEvent { value }| {
        counter.0.fetch_add(*value, Ordering::Relaxed);
    });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `event/reader` module of the `bevy_ecs` library to manage event reading in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.