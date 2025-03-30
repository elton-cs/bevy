# Bevy State Transitions Documentation

This document provides a comprehensive overview of the public API available in the `transitions` module of the `bevy_state` library. It includes details on structs, enums, and functions that can be utilized to manage state transitions in a Bevy application or game.

## Structs

### `OnEnter<S: States>`
- **Description**: A label for a `Schedule` that runs whenever `State<S>` enters the provided state.
- **Usage**: Use this struct to define systems that should execute when entering a specific state.
- **Example**:
  ```rust
  app.add_systems(OnEnter(GameState::InGame), my_system);
  ```

### `OnExit<S: States>`
- **Description**: A label for a `Schedule` that runs whenever `State<S>` exits the provided state.
- **Usage**: Use this struct to define systems that should execute when exiting a specific state.
- **Example**:
  ```rust
  app.add_systems(OnExit(GameState::Paused), cleanup_system);
  ```

### `OnTransition<S: States>`
- **Description**: A label for a `Schedule` that runs whenever `State<S>` exits and enters the provided `exited` and `entered` states.
- **Usage**: Use this struct to define systems that should execute during transitions between states.
- **Example**:
  ```rust
  app.add_systems(OnTransition { exited: GameState::Paused, entered: GameState::InGame }, transition_system);
  ```

### `StateTransition`
- **Description**: A label for a `Schedule` that runs state transitions.
- **Usage**: This schedule is triggered after `PreUpdate` by default but can be manually triggered at arbitrary times.
- **Example**:
  ```rust
  let _ = world.try_run_schedule(StateTransition);
  ```

### `StateTransitionEvent<S: States>`
- **Description**: An event sent when any state transition of `S` happens, including identity transitions.
- **Fields**:
  - `exited`: The state being exited.
  - `entered`: The state being entered.
- **Usage**: Use this event to respond to state transitions in your application.
- **Example**:
  ```rust
  fn handle_transition_event(event: StateTransitionEvent<GameState>) {
      // Handle the state transition
  }
  ```

## Functions

### `internal_apply_state_transition<S: States>(...)`
- **Description**: Applies a state change and registers the required schedules for downstream computed states and transition schedules.
- **Parameters**:
  - `event`: An event writer for state transition events.
  - `commands`: Commands for entity manipulation.
  - `current_state`: An optional reference to the current state.
  - `new_state`: An optional reference to the new state.
- **Usage**: This function is called to handle the logic of transitioning from one state to another.

### `setup_state_transitions_in_world(world: &mut World)`
- **Description**: Sets up the schedules and systems for handling state transitions within a `World`.
- **Usage**: Call this function to initialize state transition handling in your application.
- **Example**:
  ```rust
  setup_state_transitions_in_world(&mut world);
  ```

### `last_transition<S: States>(mut reader: EventReader<StateTransitionEvent<S>>) -> Option<StateTransitionEvent<S>>`
- **Description**: Returns the latest state transition event of type `S`, if any are available.
- **Parameters**:
  - `reader`: An event reader for state transition events.
- **Returns**: An optional state transition event.
- **Usage**: Use this function to retrieve the most recent state transition event for processing.

### `run_enter<S: States>(transition: In<Option<StateTransitionEvent<S>>>, world: &mut World)`
- **Description**: Runs the enter schedule for the specified state transition.
- **Parameters**:
  - `transition`: An optional state transition event.
  - `world`: A mutable reference to the world.
- **Usage**: Call this function to execute systems that should run when entering a new state.

### `run_exit<S: States>(transition: In<Option<StateTransitionEvent<S>>>, world: &mut World)`
- **Description**: Runs the exit schedule for the specified state transition.
- **Parameters**:
  - `transition`: An optional state transition event.
  - `world`: A mutable reference to the world.
- **Usage**: Call this function to execute systems that should run when exiting a state.

### `run_transition<S: States>(transition: In<Option<StateTransitionEvent<S>>>, world: &mut World)`
- **Description**: Runs the transition schedule for the specified state transition.
- **Parameters**:
  - `transition`: An optional state transition event.
  - `world`: A mutable reference to the world.
- **Usage**: Call this function to execute systems that should run during a state transition.

## Example Usage

### Managing State Transitions
```rust
use bevy_app::{App, Plugin};
use bevy_state::{StatesPlugin, StateTransition};

fn main() {
    let mut app = App::new();
    app.add_plugins(StatesPlugin);
    setup_state_transitions_in_world(&mut app.world_mut());
    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `transitions` module of the `bevy_state` library to manage state transitions in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.