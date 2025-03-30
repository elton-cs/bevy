# Bevy State Scoped Events Documentation

This document provides a comprehensive overview of the public API available in the `state_scoped_events` module of the `bevy_state` library. It includes details on structs, traits, and functions that can be utilized to manage events that are scoped to specific states in a Bevy application or game.

## Structs

### `StateScopedEvents<S: FreelyMutableState>`
- **Description**: A resource that manages cleanup functions for events associated with specific states.
- **Fields**:
  - `cleanup_fns`: A hash map that associates states with a list of cleanup functions for events.
- **Methods**:
  - `add_event<E: Event>(&mut self, state: S)`: Adds an event to the cleanup functions for the specified state.
  - `cleanup(&self, w: &mut World, state: S)`: Cleans up events associated with the specified state in the given world.
- **Usage**: Use this struct to manage events that should be cleared when transitioning away from a specific state.

## Functions

### `clear_event_queue<E: Event>(w: &mut World)`
- **Description**: Clears the event queue for a specific event type in the world.
- **Parameters**:
  - `w`: A mutable reference to the world.
- **Usage**: Call this function to clear events when transitioning states to prevent stale events from being processed.

### `add_state_scoped_event_impl<E: Event, S: FreelyMutableState>(app: &mut SubApp, _p: PhantomData<E>, state: S)`
- **Description**: Implements the logic to add a state-scoped event to a sub-application.
- **Parameters**:
  - `app`: A mutable reference to the sub-application.
  - `_p`: A phantom data marker for the event type.
  - `state`: The state to which the event is scoped.
- **Usage**: Use this function internally to register events that should be cleaned up when leaving a specific state.

## Traits

### `StateScopedEventsAppExt`
- **Description**: An extension trait for `App` and `SubApp` that adds methods for registering state-scoped events.
- **Methods**:
  - `add_state_scoped_event<E: Event>(&mut self, state: impl FreelyMutableState) -> &mut Self`: Adds an event that is automatically cleaned up when leaving the specified state.
- **Usage**: Implement this trait to easily add state-scoped events to your application.

## Example Usage

### Adding State Scoped Events
```rust
use bevy_app::{App, SubApp};
use bevy_ecs::prelude::*;
use bevy_state::prelude::*;

#[derive(Clone, Copy, PartialEq, Eq, Hash, Debug, Default, States)]
enum GameState {
    #[default]
    MainMenu,
    InGame,
}

fn main() {
    let mut app = App::new();
    app.init_state::<GameState>();
    app.enable_state_scoped_entities::<GameState>();

    app.add_state_scoped_event::<MyEvent>(GameState::InGame);
    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `state_scoped_events` module of the `bevy_state` library to manage events that are scoped to specific states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.