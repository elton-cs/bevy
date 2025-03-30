# Bevy Freely Mutable State Documentation

This document provides a comprehensive overview of the public API available in the `freely_mutable_state` module of the `bevy_state` library. It includes details on traits and functions that can be utilized to manage states that can be mutated directly in a Bevy application or game.

## Traits

### `FreelyMutableState`
- **Description**: This trait allows a state to be mutated directly using the `NextState<S>` resource.
- **Usage**:
  - Ordinary states are freely mutable and implement this trait as part of their derive macro.
  - Computed states are not freely mutable; they can only change when the states that drive them do.
- **Methods**:
  - `register_state(schedule: &mut Schedule)`: Registers all the necessary systems to apply state changes and run transition schedules.
    - **Description**: This method sets up the required systems for managing state transitions and schedules.
    - **Usage**: Call this method to ensure that the state is properly integrated into the Bevy ECS schedule.
    - **Example**:
      ```rust
      fn my_state_register(schedule: &mut Schedule) {
          MyState::register_state(schedule);
      }
      ```

## Functions

### `apply_state_transition<S: FreelyMutableState>(...)`
- **Description**: Applies the state transition based on the current and next state.
- **Parameters**:
  - `event`: An event writer for state transition events.
  - `commands`: Commands for entity manipulation.
  - `current_state`: An optional reference to the current state.
  - `next_state`: An optional reference to the next state.
- **Usage**: This function is called to handle the logic of transitioning from one state to another.
- **Example**:
  ```rust
  fn my_transition_system(
      event: EventWriter<StateTransitionEvent<MyState>>,
      commands: Commands,
      current_state: Option<ResMut<State<MyState>>>,
      next_state: Option<ResMut<NextState<MyState>>>,
  ) {
      apply_state_transition(event, commands, current_state, next_state);
  }
  ```

## Example Usage

### Implementing a Freely Mutable State
```rust
use bevy_ecs::prelude::*;
use bevy_state::{FreelyMutableState, NextState, State};

#[derive(States, Clone, Copy, PartialEq, Eq, Hash, Debug)]
enum GameState {
    Playing,
    Paused,
}

impl FreelyMutableState for GameState {}

fn main() {
    let mut app = App::new();
    app.init_state::<GameState>();
    app.add_systems(Update, |commands: &mut Commands| {
        commands.set_state(GameState::Playing);
    });
    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `freely_mutable_state` module of the `bevy_state` library to manage freely mutable states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.