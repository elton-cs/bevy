# Bevy State Commands Documentation

This document provides a comprehensive overview of the public API available in the `commands` module of the `bevy_state` library. It includes details on traits and functions that can be utilized to manage states in a Bevy application or game.

## Traits

### `CommandsStatesExt`
- **Description**: An extension trait for `Commands` that adds helpers for managing states within the Bevy ECS (Entity Component System).
- **Methods**:
  - `set_state<S: FreelyMutableState>(&mut self, state: S)`: Sets the next state the app should move to.
    - **Description**: This method schedules a command that updates the `NextState<S>` resource with the provided state.
    - **Note**: Modifying `NextState` directly may be more efficient depending on your use case, as commands introduce synchronization points in the ECS schedule.
    - **Usage**: Call this method to change the application's state in a way that integrates with Bevy's command system.
    - **Example**:
      ```rust
      use bevy_app::{App, Commands};
      use bevy_state::{NextState, CommandsStatesExt};

      fn main() {
          let mut app = App::new();
          app.add_systems(Update, |commands: &mut Commands| {
              commands.set_state(MyState::Running);
          });
          app.run();
      }
      ```

## Example Usage

### Setting the Next State
```rust
use bevy_app::{App, Commands};
use bevy_state::{NextState, CommandsStatesExt};

#[derive(Debug, Clone, Copy, PartialEq, Eq, Hash)]
enum MyState {
    Initial,
    Running,
    Finished,
}

fn main() {
    let mut app = App::new();
    app.add_systems(Update, |commands: &mut Commands| {
        commands.set_state(MyState::Running);
    });
    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `commands` module of the `bevy_state` library to manage states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.