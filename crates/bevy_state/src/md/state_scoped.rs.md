# Bevy State Scoped Documentation

This document provides a comprehensive overview of the public API available in the `state_scoped` module of the `bevy_state` library. It includes details on structs, functions, and their usage for managing entities that are scoped to specific states in a Bevy application or game.

## Structs

### `StateScoped<S: States>`
- **Description**: A component that marks entities for removal when the world's state of the matching type no longer matches the supplied value.
- **Fields**:
  - `0`: The state value that this component is scoped to.
- **Usage**: Attach this component to entities that should be removed when the application transitions away from the specified state.
- **Example**:
  ```rust
  use bevy_state::prelude::*;
  use bevy_ecs::prelude::*;

  #[derive(Clone, Copy, PartialEq, Eq, Hash, Debug, Default, States)]
  enum GameState {
      #[default]
      MainMenu,
      SettingsMenu,
      InGame,
  }

  fn spawn_player(mut commands: Commands) {
      commands.spawn((
          StateScoped(GameState::InGame),
          Player
      ));
  }
  ```

## Functions

### `clear_state_scoped_entities<S: States>(mut commands: Commands, mut transitions: EventReader<StateTransitionEvent<S>>, query: Query<(Entity, &StateScoped<S>)>)`
- **Description**: Removes entities marked with `StateScoped<S>` when their state no longer matches the world state.
- **Parameters**:
  - `commands`: A mutable reference to the commands for entity manipulation.
  - `transitions`: An event reader for state transition events.
  - `query`: A query for entities with the `StateScoped<S>` component.
- **Usage**: Call this function in a system to automatically despawn entities that are no longer valid in the current state.
- **Example**:
  ```rust
  fn clear_entities_on_state_change(
      mut commands: Commands,
      transitions: EventReader<StateTransitionEvent<GameState>>,
      query: Query<(Entity, &StateScoped<GameState>)>,
  ) {
      clear_state_scoped_entities(commands, transitions, query);
  }
  ```

## Example Usage

### Using State Scoped Entities
```rust
use bevy_state::prelude::*;
use bevy_ecs::prelude::*;

#[derive(Clone, Copy, PartialEq, Eq, Hash, Debug, Default, States)]
enum GameState {
    #[default]
    MainMenu,
    SettingsMenu,
    InGame,
}

fn spawn_player(mut commands: Commands) {
    commands.spawn((
        StateScoped(GameState::InGame),
        Player
    ));
}

fn main() {
    let mut app = App::new();
    app.init_state::<GameState>();
    app.enable_state_scoped_entities::<GameState>();
    app.add_systems(OnEnter(GameState::InGame), spawn_player);
    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `state_scoped` module of the `bevy_state` library to manage state-scoped entities in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.