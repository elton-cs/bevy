# Bevy State Resources Documentation

This document provides a comprehensive overview of the public API available in the `resources` module of the `bevy_state` library. It includes details on structs, enums, and functions that can be utilized to manage states in a Bevy application or game.

## Structs

### `State<S: States>`
- **Description**: A finite-state machine whose transitions have associated schedules (`OnEnter(state)` and `OnExit(state)`).
- **Fields**:
  - `0`: The current state value of type `S`.
- **Methods**:
  - `new(state: S) -> Self`: Creates a new state with a specific value.
    - **Usage**: To change the state, use `NextState<S>` rather than modifying the `State<S>` directly.
  - `get(&self) -> &S`: Gets the current state.
    - **Usage**: Use this method to access the current state value.
- **Example**:
  ```rust
  #[derive(Clone, Copy, PartialEq, Eq, Hash, Debug, Default, States)]
  enum GameState {
      #[default]
      MainMenu,
      InGame,
  }

  fn game_logic(game_state: Res<State<GameState>>) {
      match game_state.get() {
          GameState::InGame => {
              // Run game logic here...
          },
          _ => {},
      }
  }
  ```

### `NextState<S: FreelyMutableState>`
- **Description**: Represents the next state of `State<S>`. This can be fetched as a resource and used to queue state transitions.
- **Variants**:
  - `Unchanged`: Indicates no state transition is pending.
  - `Pending(S)`: Indicates there is a pending transition for state `S`.
- **Methods**:
  - `set(&mut self, state: S)`: Tentatively sets a pending state transition to `Some(state)`.
    - **Usage**: Call this method to queue a transition to a new state.
  - `reset(&mut self)`: Removes any pending changes to `State<S>`.
    - **Usage**: Call this method to clear any queued state transitions.
- **Example**:
  ```rust
  fn start_game(mut next_game_state: ResMut<NextState<GameState>>) {
      next_game_state.set(GameState::InGame);
  }
  ```

## Functions

### `take_next_state<S: FreelyMutableState>(next_state: Option<ResMut<NextState<S>>>) -> Option<S>`
- **Description**: Takes the next state from the resource if it is pending.
- **Parameters**:
  - `next_state`: An optional mutable reference to the next state resource.
- **Returns**: An optional state of type `S` if a transition is pending.
- **Usage**: Call this function to retrieve the next state for processing during state transitions.

## Example Usage

### Managing State Transitions
```rust
use bevy_app::{App, Plugin};
use bevy_state::{StatesPlugin, State, NextState};

#[derive(Clone, Copy, PartialEq, Eq, Hash, Debug, Default, States)]
enum GameState {
    #[default]
    MainMenu,
    InGame,
}

fn main() {
    let mut app = App::new();
    app.add_plugins(StatesPlugin)
        .init_state::<GameState>()
        .insert_resource(NextState::Pending(GameState::InGame));

    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `resources` module of the `bevy_state` library to manage state resources in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.