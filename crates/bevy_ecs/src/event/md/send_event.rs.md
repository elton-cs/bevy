# Bevy Send Event Documentation

This document provides a comprehensive overview of the public API available in the `event/send_event` module of the `bevy_ecs` library. It includes details on structs and their usage that can be utilized to send events in a Bevy application or game.

## Structs

### `SendEvent<E: Event>`
- **Description**: A command struct used to send an arbitrary event of type `E` within the ECS world.
- **Fields**:
  - `event`: The event to send, which must implement the `Event` trait.
- **Key Points**:
  - This struct is typically used in conjunction with the `Commands::send_event` method to enqueue an event for processing in the ECS.
  - It encapsulates the event data that will be sent to the `Events<E>` resource.

## Implementations

### `Command` Trait Implementation
- **Description**: The `SendEvent` struct implements the `Command` trait, allowing it to be executed within the ECS system.
- **Methods**:
  - `fn apply(self, world: &mut World)`: This method is called to execute the command. It sends the encapsulated event to the `Events<E>` resource in the provided world.
    - **Parameters**:
      - `self`: The `SendEvent` instance containing the event to be sent.
      - `world`: A mutable reference to the `World` where the event will be sent.
    - **Usage**: This method is automatically invoked when the command is executed in the ECS system.

## Example Usage

### Sending an Event
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::event::{Event, Events, SendEvent};

#[derive(Event)]
struct MyEvent {
    value: usize,
}

fn send_my_event(commands: &mut Commands) {
    let event = MyEvent { value: 42 };
    commands.add(SendEvent { event });
}
```

### Applying the Command
```rust
fn apply_send_event(world: &mut World) {
    let event = MyEvent { value: 100 };
    let command = SendEvent { event };
    command.apply(world); // This will send the event to the Events resource
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `event/send_event` module of the `bevy_ecs` library to manage event sending in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.