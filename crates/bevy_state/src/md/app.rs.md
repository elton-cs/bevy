# Bevy State Management Documentation

This document provides a comprehensive overview of the public API available in the `app` module of the `bevy_state` library. It includes details on structs, enums, and functions that can be utilized to manage states in a Bevy application or game.

## Traits

### `AppExtStates`
- **Description**: Extension trait for `App` and `SubApp` that provides methods for managing states.
- **Methods**:
  - `init_state<S: FreelyMutableState + FromWorld>(&mut self) -> &mut Self`: Initializes a `State` with standard starting values. This method is idempotent.
  - `insert_state<S: FreelyMutableState>(&mut self, state: S) -> &mut Self`: Inserts a specific `State` into the current `App`, overriding any previously added state of the same type.
  - `add_computed_state<S: ComputedStates>(&mut self) -> &mut Self`: Sets up a type implementing `ComputedStates`.
  - `add_sub_state<S: SubStates>(&mut self) -> &mut Self`: Sets up a type implementing `SubStates`.
  - `enable_state_scoped_entities<S: States>(&mut self) -> &mut Self`: Enables state-scoped entity clearing for a specific state.
  - `register_type_state<S>(&mut self) -> &mut Self`: Registers the state type `S` for reflection.
  - `register_type_mutable_state<S>(&mut self) -> &mut Self`: Registers the mutable state type `S` for reflection.

## Structs

### `StatesPlugin`
- **Description**: A plugin that registers the `StateTransition` schedule in the `MainScheduleOrder` to enable state processing.
- **Methods**:
  - `build(&self, app: &mut App)`: Configures the app to include state management functionality.
- **Usage**: Add this plugin to your app to enable state management features.

## Example Usage

### Initializing and Using States
```rust
use bevy_app::{App, Plugin};
use bevy_state::{StatesPlugin, State, NextState};

fn main() {
    let mut app = App::new();
    app.add_plugins(StatesPlugin);

    app.init_state::<MyState>();
    app.insert_state(MyState::Initial);

    app.run();
}

#[derive(Debug, Clone, Copy, PartialEq, Eq, Hash)]
enum MyState {
    Initial,
    Running,
    Finished,
}
```

### Inserting and Managing States
```rust
fn main() {
    let mut app = App::new();
    app.add_plugins(StatesPlugin);

    app.insert_state(MyState::Running);
    // Further state management logic...
}
```

## Lifecycle of a Plugin
- When adding a plugin to an `App`:
  - The app calls `Plugin::build` immediately to register the plugin.
  - Once the app starts, it waits for all registered `Plugin::ready` methods to return `true`.
  - It then calls all registered `Plugin::finish` methods.
  - Finally, it calls all registered `Plugin::cleanup` methods.

This documentation serves as a comprehensive guide for developers looking to utilize the `app` module of the `bevy_state` library to manage states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.