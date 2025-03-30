# Bevy State Management Library Documentation

This document provides a comprehensive overview of the public API available in the `bevy_state` library. It includes details on modules, structs, enums, and functions that can be utilized to manage states in a Bevy application or game.

## Modules

### `app`
- **Description**: Provides `App` and `SubApp` with state installation methods.
- **Usage**: Use this module to extend the functionality of the main application and sub-applications with state management capabilities.

### `commands`
- **Description**: Provides extension methods for `Commands`.
- **Usage**: Use this module to manage state transitions through commands in the Bevy ECS.

### `condition`
- **Description**: Provides definitions for runtime conditions that interact with the state system.
- **Usage**: Use this module to define conditions that determine when systems should run based on the current state.

### `state`
- **Description**: Provides definitions for the basic traits required by the state system.
- **Usage**: Use this module to define and manage states, including standard states, sub-states, and computed states.

### `state_scoped`
- **Description**: Provides `StateScoped` and `clear_state_scoped_entities` for managing the lifetime of entities.
- **Usage**: Use this module to manage entities that are scoped to specific states.

### `state_scoped_events`
- **Description**: Provides `App` and `SubApp` with methods for registering state-scoped events.
- **Usage**: Use this module to handle events that are specific to certain states in the application.

### `reflect`
- **Description**: Provides definitions for the basic traits required by the state system for reflection.
- **Usage**: Use this module to enable reflection capabilities for state types.

### `prelude`
- **Description**: The state prelude, which includes the most common types in this crate, re-exported for convenience.
- **Usage**: Import this module to easily access frequently used types and traits related to state management.

## Traits

### `AppExtStates`
- **Description**: Extension trait for `App` and `SubApp` that provides methods for managing states.
- **Methods**:
  - `init_state<S: FreelyMutableState + FromWorld>(&mut self) -> &mut Self`: Initializes a `State` with standard starting values.
  - `insert_state<S: FreelyMutableState>(&mut self, state: S) -> &mut Self`: Inserts a specific `State` into the current `App`.
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

This documentation serves as a comprehensive guide for developers looking to utilize the `bevy_state` library to manage states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.