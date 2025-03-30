# Bevy Event Cursor Documentation

This document provides a comprehensive overview of the public API available in the `event/event_cursor` module of the `bevy_ecs` library. It includes details on structs and their usage that can be utilized to manage event reading in a Bevy application or game.

## Structs

### `EventCursor<E: Event>`
- **Description**: A struct that stores the state for an `EventReader` or `EventMutator`. It is used to track which events have been seen and allows for reading events from the `Events<E>` resource.
- **Key Points**:
  - It is useful for manually tracking events, especially when sending and receiving events of the same type in the same system.
  - In most cases, you should use an `EventReader` or `EventMutator`, which automatically manage the state for you.
- **Fields**:
  - `last_event_count`: A `usize` that tracks the last event count seen by this cursor.
  - `_marker`: A phantom data marker for the event type.
- **Methods**:
  - `fn read<'a>(&'a mut self, events: &'a Events<E>) -> EventIterator<'a, E>`: Reads events from the `Events<E>` resource without IDs.
  - `fn read_mut<'a>(&'a mut self, events: &'a mut Events<E>) -> EventMutIterator<'a, E>`: Reads mutable events from the `Events<E>` resource without IDs.
  - `fn read_with_id<'a>(&'a mut self, events: &'a Events<E>) -> EventIteratorWithId<'a, E>`: Reads events with their IDs from the `Events<E>` resource.
  - `fn read_mut_with_id<'a>(&'a mut self, events: &'a mut Events<E>) -> EventMutIteratorWithId<'a, E>`: Reads mutable events with their IDs from the `Events<E>` resource.
  - `fn par_read<'a>(&'a mut self, events: &'a Events<E>) -> EventParIter<'a, E>`: Reads events in parallel if the multi-threaded feature is enabled.
  - `fn par_read_mut<'a>(&'a mut self, events: &'a mut Events<E>) -> EventMutParIter<'a, E>`: Reads mutable events in parallel if the multi-threaded feature is enabled.
  - `fn len(&self, events: &Events<E>) -> usize`: Returns the number of events that this cursor has seen since the last read.
  - `fn missed_events(&self, events: &Events<E>) -> usize`: Returns the number of events that were missed since the last read.
  - `fn is_empty(&self, events: &Events<E>) -> bool`: Returns true if there are no events that this cursor has seen.
  - `fn clear(&mut self, events: &Events<E>)`: Resets the cursor's last event count to the current event count in the `Events<E>` resource.

## Example Usage

### Using EventCursor to Send and Receive Events
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::event::{Event, Events, EventCursor};

#[derive(Event, Clone, Debug)]
struct MyEvent;

fn send_and_receive_events(
    mut local_event_reader: Local<EventCursor<MyEvent>>,
    mut events: ResMut<Events<MyEvent>>,
) {
    let mut events_to_resend = Vec::new();

    // Read events and collect them for resending
    for event in local_event_reader.read(&mut events) {
        events_to_resend.push(event.clone());
    }

    // Resend collected events
    for event in events_to_resend {
        events.send(MyEvent);
    }
}

// Ensure the function is a valid system
# bevy_ecs::system::assert_is_system(send_and_receive_events);
```

This documentation serves as a comprehensive guide for developers looking to utilize the `event/event_cursor` module of the `bevy_ecs` library to manage event reading in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.