# Bevy State Reflection Documentation

This document provides a comprehensive overview of the public API available in the `reflect` module of the `bevy_state` library. It includes details on structs, traits, and functions that can be utilized to manage reflection for states in a Bevy application or game.

## Structs

### `ReflectState`
- **Description**: A struct used to operate on the reflected `States` trait of a type.
- **Usage**: A `ReflectState` for type `T` can be obtained via `bevy_reflect::TypeRegistration::data`.
- **Example**:
  ```rust
  let reflect_state: ReflectState = ReflectState::new::<MyState>();
  ```

### `ReflectStateFns`
- **Description**: The raw function pointers needed to make up a `ReflectState`.
- **Fields**:
  - `reflect`: Function pointer implementing `ReflectState::reflect()`.
- **Methods**:
  - `new<T: States + Reflect>() -> Self`: Gets the default set of `ReflectStateFns` for a specific component type using its `FromType` implementation.
- **Usage**: Use this struct to create custom reflection functions for state types.

### `ReflectFreelyMutableState`
- **Description**: A struct used to operate on the reflected `FreelyMutableState` trait of a type.
- **Usage**: A `ReflectFreelyMutableState` for type `T` can be obtained via `bevy_reflect::TypeRegistration::data`.

### `ReflectFreelyMutableStateFns`
- **Description**: The raw function pointers needed to make up a `ReflectFreelyMutableState`.
- **Fields**:
  - `set_next_state`: Function pointer implementing `ReflectFreelyMutableState::set_next_state()`.
- **Methods**:
  - `new<T: FreelyMutableState + Reflect + TypePath>() -> Self`: Gets the default set of `ReflectFreelyMutableStateFns` for a specific component type using its `FromType` implementation.
- **Usage**: Use this struct to create custom reflection functions for freely mutable state types.

## Traits

### `FromType`
- **Description**: A trait that allows for the creation of types from their reflected counterparts.
- **Usage**: Implement this trait for types that need to be reflected in the Bevy ECS.

## Functions

### `ReflectState::reflect(&self, world: &World) -> Option<&dyn Reflect>`
- **Description**: Gets the value of this `States` type from the world as a reflected reference.
- **Parameters**:
  - `world`: A reference to the current world.
- **Returns**: An optional reference to the reflected state.
- **Usage**: Use this method to retrieve the current state value in a reflected form.

### `ReflectFreelyMutableState::set_next_state(&self, world: &mut World, state: &dyn Reflect, registry: &TypeRegistry)`
- **Description**: Tentatively sets a pending state transition to a reflected `ReflectFreelyMutableState`.
- **Parameters**:
  - `world`: A mutable reference to the current world.
  - `state`: A reference to the reflected state to set.
  - `registry`: A reference to the type registry.
- **Usage**: Use this method to update the next state in the world based on the reflected state.

## Example Usage

### Using Reflection with States
```rust
use bevy_app::{App, Plugin};
use bevy_state::{StatesPlugin, ReflectState};

#[derive(States, Clone, Copy, PartialEq, Eq, Hash, Debug)]
enum GameState {
    Playing,
    Paused,
}

fn main() {
    let mut app = App::new();
    app.add_plugins(StatesPlugin)
        .insert_state(GameState::Playing)
        .register_type_mutable_state::<GameState>();

    let reflect_state = ReflectState::new::<GameState>();
    // Use reflect_state to manage state transitions
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `reflect` module of the `bevy_state` library to manage reflection for states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.