# Bevy SubStates Documentation

This document provides a comprehensive overview of the public API available in the `sub_states` module of the `bevy_state` library. It includes details on traits and their usage for managing sub-states in a Bevy application or game.

## Traits

### `SubStates`
- **Description**: A trait for defining sub-states that exist only when the source state meets certain conditions. Unlike `ComputedStates`, sub-states can be manually modified while they exist.
- **Associated Types**:
  - `SourceStates`: The set of states from which the `SubStates` is derived. This can be a single type or a tuple containing multiple types that implement `States`.
- **Methods**:
  - `should_exist(sources: Self::SourceStates) -> Option<Self>`: This function is called whenever one of the `SourceStates` changes. The result is used to determine the existence of `State<Self>`. If the result is `None`, the `State<Self>` resource will be removed from the world.
    - **Usage**: Implement this method to define the conditions under which the sub-state should exist.
  - `register_sub_state_systems(schedule: &mut Schedule)`: Sets up systems that compute the state whenever one of the `SourceStates` changes. This is called by `App::add_computed_state`, but can also be called manually.
    - **Usage**: Call this method to ensure that the sub-state is properly integrated into the Bevy ECS schedule.

## Example Usage

### Defining a SubState
```rust
use bevy_ecs::prelude::*;
use bevy_state::prelude::*;

#[derive(States, Clone, PartialEq, Eq, Hash, Debug, Default)]
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

fn main() {
    let mut app = App::new();
    app.init_state::<AppState>()
        .add_sub_state::<GamePhase>();
    app.run();
}
```

### Using SubStates in an App
```rust
use bevy_app::{App, Plugin};
use bevy_state::{StatesPlugin, SubStates};

fn main() {
    let mut app = App::new();
    app.add_plugins(StatesPlugin)
        .init_state::<AppState>()
        .add_sub_state::<GamePhase>();

    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `sub_states` module of the `bevy_state` library to manage sub-states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.