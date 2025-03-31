# Bevy ECS Adapter System Module Documentation

This document provides a comprehensive overview of the public API available in the `adapter_system` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to customize system behavior in a Bevy application or game.

## Concepts

### Trait: `Adapt<S>`
- **Description**: Customizes the behavior of an `AdapterSystem`.
- **Type Parameters**:
  - `S`: The system type that this trait adapts.
- **Associated Types**:
  - `In`: The input type for the `AdapterSystem`.
  - `Out`: The output type for the `AdapterSystem`.
- **Methods**:
  - `adapt`: 
    - **Description**: Customizes how the system is run and how its inputs/outputs are adapted.
    - **Parameters**:
      - `input`: The input data for the system.
      - `run_system`: A function that runs the system and returns its output.
    - **Returns**: The adapted output.

### Struct: `IntoAdapterSystem<Func, S>`
- **Description**: An `IntoSystem` that creates an instance of `AdapterSystem`.
- **Type Parameters**:
  - `Func`: The function type used for adaptation.
  - `S`: The system type being adapted.
- **Fields**:
  - `func`: The adaptation function.
  - `system`: The system being adapted.
- **Methods**:
  - `new`: 
    - **Description**: Creates a new `IntoAdapterSystem` that uses `func` to adapt `system`.
    - **Parameters**:
      - `func`: The adaptation function.
      - `system`: The system to adapt.
    - **Returns**: A new instance of `IntoAdapterSystem`.

### Struct: `AdapterSystem<Func, S>`
- **Description**: A system that takes the output of `S` and transforms it by applying `Func` to it.
- **Type Parameters**:
  - `Func`: The function type used for adaptation.
  - `S`: The system type being adapted.
- **Fields**:
  - `func`: The adaptation function.
  - `system`: The underlying system.
  - `name`: The name of the system.
- **Methods**:
  - `new`: 
    - **Description**: Creates a new `AdapterSystem` that uses `func` to adapt `system`.
    - **Parameters**:
      - `func`: The adaptation function.
      - `system`: The system to adapt.
      - `name`: The name of the system.
    - **Returns**: A new instance of `AdapterSystem`.

### Implementations of `System` for `AdapterSystem<Func, S>`
- **Methods**:
  - `name`: 
    - **Description**: Returns the name of the system.
  - `component_access`: 
    - **Description**: Provides access to the components used by the system.
  - `archetype_component_access`: 
    - **Description**: Provides access to the archetype components used by the system.
  - `is_send`: 
    - **Description**: Checks if the system can be sent across threads.
  - `is_exclusive`: 
    - **Description**: Checks if the system is exclusive.
  - `has_deferred`: 
    - **Description**: Checks if the system has deferred actions.
  - `run_unsafe`: 
    - **Description**: Runs the system in an unsafe context.
    - **Parameters**:
      - `input`: The input data for the system.
      - `world`: The world context.
    - **Returns**: The output of the system.
  - `run`: 
    - **Description**: Runs the system with the provided input and world context.
    - **Parameters**:
      - `input`: The input data for the system.
      - `world`: The mutable world context.
    - **Returns**: The output of the system.
  - `apply_deferred`: 
    - **Description**: Applies any deferred actions in the system.
    - **Parameters**:
      - `world`: The mutable world context.
  - `queue_deferred`: 
    - **Description**: Queues deferred actions for the system.
    - **Parameters**:
      - `world`: The deferred world context.
  - `validate_param_unsafe`: 
    - **Description**: Validates parameters in an unsafe context.
    - **Parameters**:
      - `world`: The world context.
    - **Returns**: A boolean indicating validity.
  - `initialize`: 
    - **Description**: Initializes the system with the provided world context.
    - **Parameters**:
      - `world`: The mutable world context.
  - `update_archetype_component_access`: 
    - **Description**: Updates access to archetype components.
    - **Parameters**:
      - `world`: The world context.
  - `check_change_tick`: 
    - **Description**: Checks the change tick for the system.
    - **Parameters**:
      - `change_tick`: The tick to check.
  - `default_system_sets`: 
    - **Description**: Returns the default system sets for the system.
    - **Returns**: A vector of `InternedSystemSet`.
  - `get_last_run`: 
    - **Description**: Gets the last run tick of the system.
    - **Returns**: The last run tick.
  - `set_last_run`: 
    - **Description**: Sets the last run tick of the system.
    - **Parameters**:
      - `last_run`: The tick to set.

### Implementation of `Adapt<S>` for Function Types
- **Description**: Allows function types to implement the `Adapt` trait.
- **Type Parameters**:
  - `F`: The function type.
  - `S`: The system type being adapted.
- **Methods**:
  - `adapt`: 
    - **Description**: Adapts the input and runs the system.
    - **Parameters**:
      - `input`: The input data for the system.
      - `run_system`: A function that runs the system and returns its output.
    - **Returns**: The adapted output.

This documentation serves as a comprehensive guide for developers looking to utilize the `adapter_system` module of the `bevy_ecs` library to customize system behavior in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.
