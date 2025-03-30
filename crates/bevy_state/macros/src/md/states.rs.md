# Bevy State Macros Documentation

This document provides a comprehensive overview of the public API available in the `macros` module of the `bevy_state` library. It includes details on macros that can be utilized to manage states in a Bevy application or game.

## Macros

### `#[proc_macro_derive(States)]`
- **Description**: A procedural macro that derives the `States` trait for a given enum.
- **Usage**:
  - This macro is used to automatically implement the `States` trait for an enum, allowing it to be used as a state in the Bevy state management system.
  - The derived implementation will include the necessary methods and traits required for state management.
- **Example**:
  ```rust
  use bevy_state::prelude::*;

  #[derive(States, Clone, Copy, PartialEq, Eq, Hash, Debug, Default)]
  enum GameState {
      #[default]
      MainMenu,
      InGame,
  }
  ```

### `#[proc_macro_derive(SubStates, attributes(source))]`
- **Description**: A procedural macro that derives the `SubStates` trait for a given enum, with an attribute to specify the source state.
- **Usage**:
  - This macro is used to automatically implement the `SubStates` trait for an enum, allowing it to be used as a sub-state that exists only when the specified source state is active.
  - The `source` attribute is used to define which state this sub-state is derived from.
- **Example**:
  ```rust
  use bevy_state::prelude::*;

  #[derive(States, Clone, Copy, PartialEq, Eq, Hash, Debug, Default)]
  enum AppState {
      #[default]
      Menu,
      InGame,
  }

  #[derive(SubStates, Clone, PartialEq, Eq, Hash, Debug, Default)]
  #[source(AppState = AppState::InGame)]
  enum GamePhase {
      #[default]
      Setup,
      Battle,
      Conclusion,
  }
  ```

## Functions

### `bevy_state_path() -> syn::Path`
- **Description**: A helper function that returns the path to the `bevy_state` crate.
- **Usage**: This function is used internally to resolve the path for the `bevy_state` crate, facilitating the use of macros and types defined within it.

## Example Usage

### Using the Macros in an Application
```rust
use bevy_app::{App, Plugin};
use bevy_state::{StatesPlugin};

#[derive(States, Clone, Copy, PartialEq, Eq, Hash, Debug, Default)]
enum GameState {
    #[default]
    MainMenu,
    InGame,
}

fn main() {
    let mut app = App::new();
    app.add_plugins(StatesPlugin)
        .init_state::<GameState>();
    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the macros in the `bevy_state` library to manage states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.