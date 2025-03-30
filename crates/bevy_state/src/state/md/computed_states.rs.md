# Bevy Computed States Documentation

This document provides a comprehensive overview of the public API available in the `computed_states` module of the `bevy_state` library. It includes details on traits and their usage for managing computed states in a Bevy application or game.

## Traits

### `ComputedStates`
- **Description**: A trait for defining a state whose value is automatically computed based on the values of other `States`.
- **Associated Types**:
  - `SourceStates`: The set of states from which the `ComputedStates` is derived. This can be a single type, an `Option` of a type, or a tuple containing multiple types that implement `States`.
- **Methods**:
  - `compute(sources: Self::SourceStates) -> Option<Self>`: Computes the next value of the `State<Self>`. This function is called whenever one of the `SourceStates` changes. If the result is `None`, the `State<Self>` resource will be removed from the world.
  - `register_computed_state_systems(schedule: &mut Schedule)`: Sets up systems that compute the state whenever one of the `SourceStates` changes. This is called by `App::add_computed_state`, but can also be called manually.
- **Usage**: Implement this trait to create states that depend on other states, allowing for dynamic state management based on the current application context.

## Example Usage

### Defining a Computed State
```rust
use bevy_state::prelude::*;
use bevy_ecs::prelude::*;

#[derive(States, Clone, PartialEq, Eq, Hash, Debug, Default)]
enum AppState {
    #[default]
    Menu,
    InGame { paused: bool },
}

#[derive(Clone, PartialEq, Eq, Hash, Debug)]
struct InGame;

impl ComputedStates for InGame {
    type SourceStates = AppState;

    fn compute(sources: AppState) -> Option<Self> {
        match sources {
            AppState::InGame { .. } => Some(InGame),
            _ => None,
        }
    }
}
```

### Adding a Computed State to an App
```rust
use bevy_app::{App, Plugin};
use bevy_state::{StatesPlugin, ComputedStates};

fn main() {
    let mut app = App::new();
    app.add_plugins(StatesPlugin)
        .init_state::<AppState>()
        .add_computed_state::<InGame>();

    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `computed_states` module of the `bevy_state` library to manage computed states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.