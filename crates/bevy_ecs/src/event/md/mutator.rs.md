# Bevy Event Mutator Documentation

This document provides a comprehensive overview of the public API available in the `event/mutator` module of the `bevy_ecs` library. It includes details on structs and their usage that can be utilized to manage mutable event reading in a Bevy application or game.

## Structs

### `EventMutator<'w, 's, E: Event>`
- **Description**: A struct that mutably reads events of type `E`, keeping track of which events have already been read by each system. This allows multiple systems to read the same events, making it ideal for chains of systems that all want to modify the same events.
- **Key Points**:
  - `EventMutators` are usually declared as a `SystemParam`.
  - They provide a way to read events while ensuring that each system can track its own state regarding which events have been processed.
  - Multiple systems with `EventMutator<T>` of the same event type cannot run concurrently.
- **Fields**:
  - `reader`: A local state that tracks the cursor for the events.
  - `events`: A mutable reference to the `Events<E>` resource containing the events.
- **Methods**:
  - `fn read(&mut self) -> EventMutIterator<'_, E>`: Iterates over the events this `EventMutator` has not seen yet, marking them as read in the process.
  - `fn read_with_id(&mut self) -> EventMutIteratorWithId<'_, E>`: Similar to `read`, but also returns the `EventId` of the events.
  - `fn par_read(&mut self) -> EventMutParIter<'_, E>`: Returns a parallel iterator over the events this `EventMutator` has not seen yet (requires multi-threading feature).
  - `fn len(&self) -> usize`: Determines the number of events available to be read without consuming any.
  - `fn is_empty(&self) -> bool`: Returns true if there are no events available to read.
  - `fn clear(&mut self)`: Consumes all available events, ensuring they will not appear in future reads.

## Example Usage

### Declaring an EventMutator in a System
```rust
use bevy_ecs::prelude::*;

#[derive(Event, Debug)]
pub struct MyEvent(pub u32); // Custom event type.

fn my_system(mut reader: EventMutator<MyEvent>) {
    for event in reader.read() {
        event.0 += 1; // Mutate the event
        println!("Received event: {:?}", event);
    }
}
```

### Checking for Events and Clearing Them
```rust
use bevy_ecs::prelude::*;

#[derive(Event)]
struct CollisionEvent;

fn play_collision_sound(mut events: EventMutator<CollisionEvent>) {
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
fn process_events_parallel(mut events: EventMutator<MyEvent>, counter: Res<Counter>) {
    events.par_read().for_each(|MyEvent { value }| {
        counter.0.fetch_add(*value, Ordering::Relaxed);
    });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `event/mutator` module of the `bevy_ecs` library to manage mutable event reading in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.