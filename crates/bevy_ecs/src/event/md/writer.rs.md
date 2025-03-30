# Bevy Event Writer Documentation

This document provides a comprehensive overview of the public API available in the `event/writer` module of the `bevy_ecs` library. It includes details on structs and their usage that can be utilized to send events in a Bevy application or game.

## Structs

### `EventWriter<'w, E: Event>`
- **Description**: A struct that sends events of type `E` within the ECS world.
- **Key Points**:
  - `EventWriter`s are usually declared as a `SystemParam`, allowing systems to send events easily.
  - They provide a way to send events while ensuring that the events are properly tracked and managed within the ECS.
- **Fields**:
  - `events`: A mutable reference to the `Events<E>` resource containing the events.

## Methods

### `send`
- **Description**: Sends an event of type `E` to the `Events` resource.
- **Parameters**:
  - `event`: The event to send.
- **Returns**: An `EventId<E>` representing the ID of the sent event.
- **Usage**:
  - This method is called to enqueue an event for processing in the ECS.
  
### `send_batch`
- **Description**: Sends a list of events all at once to the `Events` resource.
- **Parameters**:
  - `events`: An iterable collection of events to send.
- **Returns**: A `SendBatchIds<E>` representing the IDs of the sent events.
- **Usage**:
  - This method is more efficient than sending each event individually, especially when dealing with multiple events.

### `send_default`
- **Description**: Sends the default value of the event type `E`.
- **Returns**: An `EventId<E>` representing the ID of the sent default event.
- **Usage**:
  - This method is useful when the event is an empty struct or when a default event needs to be sent.

## Example Usage

### Sending a Single Event
```rust
use bevy_ecs::prelude::*;

#[derive(Event)]
struct MyEvent {
    value: usize,
}

fn my_system(mut writer: EventWriter<MyEvent>) {
    writer.send(MyEvent { value: 42 });
}
```

### Sending a Batch of Events
```rust
fn send_multiple_events(mut writer: EventWriter<MyEvent>) {
    let events = vec![MyEvent { value: 1 }, MyEvent { value: 2 }];
    writer.send_batch(events);
}
```

### Sending a Default Event
```rust
#[derive(Event, Default)]
struct EmptyEvent;

fn send_default_event(mut writer: EventWriter<EmptyEvent>) {
    writer.send_default(); // Sends an instance of EmptyEvent
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `event/writer` module of the `bevy_ecs` library to manage event sending in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.