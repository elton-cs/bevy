# Bevy State Management Documentation

This document provides a comprehensive overview of the public API available in the `states` module of the `bevy_state` library. It includes details on traits that can be utilized to manage states in a Bevy application or game.

## Traits

### `States`
- **Description**: A trait for defining world-wide states in a finite-state machine. It allows for multiple states to be defined for the same world, enabling classification of the state across orthogonal dimensions.
- **Associated Constants**:
  - `DEPENDENCY_DEPTH`: Indicates how many other states this state depends on. This is used to help order transitions and de-duplicate `ComputedStates`, as well as prevent cyclical `ComputedState` dependencies.
- **Usage**:
  - Implement this trait for any type that should represent a state in your application.
  - You can access the current state of type `T` with the `State<T>` resource and the queued state with the `NextState<T>` resource.
  - State transitions typically occur in the `OnEnter<T::Variant>` and `OnExit<T::Variant>` schedules, which can be triggered by the `StateTransition` schedule.
- **Example**:
  ```rust
  use bevy_state::prelude::*;
  use bevy_ecs::prelude::IntoSystemConfigs;
  use bevy_ecs::system::ResMut;

  #[derive(Clone, Copy, PartialEq, Eq, Hash, Debug, Default, States)]
  enum GameState {
      #[default]
      MainMenu,
      SettingsMenu,
      InGame,
  }

  fn handle_escape_pressed(mut next_state: ResMut<NextState<GameState>>) {
      // Logic to check if escape is pressed
      if escape_pressed {
          next_state.set(GameState::SettingsMenu);
      }
  }

  fn open_settings_menu() {
      // Show the settings menu...
  }

  fn main() {
      let mut app = App::new();
      app.add_systems(Update, handle_escape_pressed.run_if(in_state(GameState::MainMenu)));
      app.add_systems(OnEnter(GameState::SettingsMenu), open_settings_menu);
      app.run();
  }
  ```

This documentation serves as a comprehensive guide for developers looking to utilize the `states` module of the `bevy_state` library to manage states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.