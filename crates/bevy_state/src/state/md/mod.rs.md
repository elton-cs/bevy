# Bevy State Management Module Documentation

This document provides a comprehensive overview of the public API available in the `state` module of the `bevy_state` library. It includes details on structs, traits, and functions that can be utilized to manage states in a Bevy application or game.

## Modules

### `computed_states`
- **Description**: Contains definitions for computed states that derive their values from other states.
- **Usage**: Use this module to create states that depend on the values of other states, allowing for dynamic state management.

### `freely_mutable_state`
- **Description**: Contains definitions for states that can be mutated directly.
- **Usage**: Use this module to define states that can be changed using the `NextState<S>` resource.

### `resources`
- **Description**: Provides definitions for resources related to state management.
- **Usage**: Use this module to manage resources that are associated with different states.

### `state_set`
- **Description**: Contains definitions for managing sets of states.
- **Usage**: Use this module to group and manage multiple states together.

### `states`
- **Description**: Provides definitions for the core state management functionality.
- **Usage**: Use this module to define and manage standard states in your application.

### `sub_states`
- **Description**: Contains definitions for sub-states that are children of other states.
- **Usage**: Use this module to create hierarchical state structures.

### `transitions`
- **Description**: Provides definitions for managing state transitions.
- **Usage**: Use this module to handle transitions between different states in your application.

## Traits

### `States`
- **Description**: A trait for defining a state in the Bevy state management system.
- **Usage**: Implement this trait for any type that should represent a state in your application.

### `FreelyMutableState`
- **Description**: A trait that allows a state to be mutated directly using the `NextState<S>` resource.
- **Methods**:
  - `register_state(schedule: &mut Schedule)`: Registers all necessary systems to apply state changes and run transition schedules.
- **Usage**: Implement this trait for states that need to be freely mutable.

## Example Usage

### Defining and Using States
```rust
use bevy_state::prelude::*;
use bevy_ecs::prelude::*;

#[derive(States, Clone, Copy, PartialEq, Eq, Hash, Debug, Default)]
enum GameState {
    #[default]
    Menu,
    Playing,
    Paused,
}

fn main() {
    let mut app = App::new();
    app.init_state::<GameState>();
    app.insert_state(GameState::Menu);
    app.run();
}
```

### Using Computed States
```rust
use bevy_state::prelude::*;
use bevy_ecs::prelude::*;

#[derive(States, Clone, Copy, PartialEq, Eq, Hash, Debug, Default)]
enum AppState {
    #[default]
    MainMenu,
    InGame,
}

#[derive(Clone, PartialEq, Eq, Hash, Debug)]
struct InGame;

impl ComputedStates for InGame {
    type SourceStates = AppState;

    fn compute(sources: AppState) -> Option<Self> {
        match sources {
            AppState::InGame => Some(InGame),
            _ => None,
        }
    }
}

fn main() {
    let mut app = App::new();
    app.add_plugins(StatesPlugin)
        .init_state::<AppState>()
        .add_computed_state::<InGame>();

    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `state` module of the `bevy_state` library to manage states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.