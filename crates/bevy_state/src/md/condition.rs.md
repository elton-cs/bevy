# Bevy State Conditions Documentation

This document provides a comprehensive overview of the public API available in the `condition` module of the `bevy_state` library. It includes details on functions that can be utilized to manage conditions related to states in a Bevy application or game.

## Functions

### `state_exists<S: States>(current_state: Option<Res<State<S>>>) -> bool`
- **Description**: A condition-satisfying function that returns `true` if the state machine exists.
- **Parameters**:
  - `current_state`: An optional reference to the current state.
- **Returns**: `true` if the state exists, `false` otherwise.
- **Usage**: Use this function to check if a specific state is currently initialized in the application.
- **Example**:
  ```rust
  app.add_systems(
      my_system.run_if(state_exists::<GameState>),
  );
  ```

### `in_state<S: States>(state: S) -> impl FnMut(Option<Res<State<S>>>) -> bool + Clone`
- **Description**: Generates a condition-satisfying closure that returns `true` if the state machine is currently in the specified state.
- **Parameters**:
  - `state`: The state to check against.
- **Returns**: A closure that checks if the current state matches the specified state.
- **Usage**: Use this function to conditionally run systems based on the current state of the application.
- **Example**:
  ```rust
  app.add_systems((
      play_system.run_if(in_state(GameState::Playing)),
      pause_system.run_if(in_state(GameState::Paused)),
  ));
  ```

### `state_changed<S: States>(current_state: Option<Res<State<S>>>) -> bool`
- **Description**: A condition-satisfying function that returns `true` if the state machine changed state.
- **Parameters**:
  - `current_state`: An optional reference to the current state.
- **Returns**: `true` if the state has changed, `false` otherwise.
- **Usage**: Use this function to detect any state transitions, regardless of the specific value.
- **Example**:
  ```rust
  app.add_systems(
      my_system.run_if(state_changed::<GameState>),
  );
  ```

## Example Usage

### Using State Conditions in Systems
```rust
use bevy_ecs::prelude::*;
use bevy_state::prelude::*;

#[derive(States, Clone, Copy, Default, Eq, PartialEq, Hash, Debug)]
enum GameState {
    #[default]
    Playing,
    Paused,
}

fn main() {
    let mut app = App::new();
    app.init_resource::<State<GameState>>();

    app.add_systems(
        my_system.run_if(state_exists::<GameState>),
    );

    app.run();
}

fn my_system() {
    // System logic here
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `condition` module of the `bevy_state` library to manage state conditions in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.