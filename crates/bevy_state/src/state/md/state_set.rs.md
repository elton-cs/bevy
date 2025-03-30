# Bevy State Set Documentation

This document provides a comprehensive overview of the public API available in the `state_set` module of the `bevy_state` library. It includes details on traits and functions that can be utilized to manage sets of states in a Bevy application or game.

## Traits

### `StateSet`
- **Description**: A trait for defining a set of states or tuples of types that implement `States`.
- **Associated Constants**:
  - `SET_DEPENDENCY_DEPTH`: The total dependency depth of all states that are part of this `StateSet`, added together.
- **Methods**:
  - `register_computed_state_systems_in_schedule<T: ComputedStates<SourceStates = Self>>(schedule: &mut Schedule)`: Sets up the systems needed to compute `T` whenever any `State` in this `StateSet` changes.
  - `register_sub_state_systems_in_schedule<T: SubStates<SourceStates = Self>>(schedule: &mut Schedule)`: Sets up the systems needed to compute whether `T` exists whenever any `State` in this `StateSet` changes.
- **Usage**: Implement this trait for types that represent a collection of states, allowing for dynamic state management.

## Structs

### `InnerStateSet`
- **Description**: A trait used to isolate `ComputedStates` and `SubStates` from needing to wrap all state dependencies in an `Option<S>`.
- **Associated Types**:
  - `RawState`: The raw state type associated with the `InnerStateSet`.
- **Constants**:
  - `DEPENDENCY_DEPTH`: The total dependency depth of the state.
- **Methods**:
  - `convert_to_usable_state(wrapped: Option<&State<Self::RawState>>) -> Option<Self>`: Converts the wrapped state to a usable state.
- **Usage**: Implement this trait for types that need to manage state dependencies more flexibly.

## Example Usage

### Implementing a State Set
```rust
use bevy_ecs::prelude::*;
use bevy_state::{States, StateSet};

#[derive(States, PartialEq, Eq, Debug, Default, Hash, Clone)]
enum GameState {
    #[default]
    Menu,
    Playing,
}

impl StateSet for GameState {
    const SET_DEPENDENCY_DEPTH: usize = 1;

    fn register_computed_state_systems_in_schedule<T: ComputedStates<SourceStates = Self>>(schedule: &mut Schedule) {
        // Implementation for registering computed state systems
    }

    fn register_sub_state_systems_in_schedule<T: SubStates<SourceStates = Self>>(schedule: &mut Schedule) {
        // Implementation for registering sub-state systems
    }
}
```

### Using State Sets in an App
```rust
use bevy_app::{App, Plugin};
use bevy_state::{StatesPlugin, StateSet};

fn main() {
    let mut app = App::new();
    app.add_plugins(StatesPlugin)
        .init_state::<GameState>();

    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `state_set` module of the `bevy_state` library to manage sets of states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.