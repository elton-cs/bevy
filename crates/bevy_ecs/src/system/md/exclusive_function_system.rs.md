# Exclusive Function System Documentation

This documentation provides an overview of the public functions, structs, traits, and implementations available in the `ExclusiveFunctionSystem` module of the Bevy ECS library. It serves as a guide for developers to effectively utilize these components when building a Bevy application or game.

## Concepts

### 1. `ExclusiveFunctionSystem<Marker, F>`
- **Description**: A function system that runs with exclusive access to the [`World`].
- **Usage**:
  - Create an instance by calling [`IntoSystem::into_system`] on a function that accepts [`ExclusiveSystemParam`]s.
  - Must be initialized before running.
  - Use the `with_name` method to assign a readable name for debugging.

### 2. `with_name`
- **Description**: Returns the system with a new name.
- **Parameters**:
  - `new_name`: A string that represents the new name for the system.
- **Usage**: Useful for giving closure systems more readable and unique names for debugging and tracing.

### 3. `IsExclusiveFunctionSystem`
- **Description**: A marker type used to distinguish exclusive function systems from regular function systems.
- **Usage**: This type is hidden and used internally to enforce type safety.

### 4. `IntoSystem`
- **Description**: A trait that allows conversion of a function into a system.
- **Type Parameters**:
  - `F`: The function type that implements `ExclusiveSystemParamFunction`.
- **Usage**: Implement this trait to convert functions into systems that can be run within the Bevy ECS framework.

### 5. `System`
- **Description**: A trait that represents a system in Bevy ECS.
- **Methods**:
  - `name`: Returns the name of the system.
  - `component_access`: Provides access to the components used by the system.
  - `archetype_component_access`: Provides access to the archetype components used by the system.
  - `is_send`: Checks if the system can be sent across threads.
  - `is_exclusive`: Checks if the system is exclusive.
  - `has_deferred`: Checks if the system has deferred actions.
  - `run`: Executes the system with the provided input and world context.
  - `apply_deferred`: Applies any deferred actions in the system.
  - `queue_deferred`: Queues deferred actions for the system.
  - `validate_param_unsafe`: Validates parameters in an unsafe context.
  - `initialize`: Initializes the system with the provided world context.
  - `update_archetype_component_access`: Updates access to archetype components.
  - `check_change_tick`: Checks the change tick for the system.
  - `default_system_sets`: Returns the default system sets for the system.
  - `get_last_run`: Gets the last run tick of the system.
  - `set_last_run`: Sets the last run tick of the system.

### 6. `ExclusiveSystemParamFunction<Marker>`
- **Description**: A trait implemented for all exclusive system functions that can be used as [`System`]s.
- **Type Parameters**:
  - `Marker`: A marker type to distinguish different exclusive systems.
- **Methods**:
  - `run`: Executes the system once, taking the world, input, and parameters.

### 7. `HasExclusiveSystemInput`
- **Description**: A marker type used to distinguish exclusive function systems with and without input.
- **Usage**: This type is hidden and used internally to enforce type safety.

### 8. `impl_exclusive_system_function!`
- **Description**: A macro that implements the `ExclusiveSystemParamFunction` trait for various function signatures.
- **Usage**: This macro allows for the creation of implementations for functions with different parameter counts, enabling flexibility in defining exclusive systems.

### 9. `tests`
- **Description**: A module containing tests for the `ExclusiveFunctionSystem`.
- **Usage**: Use the provided tests to ensure the correctness of the system's behavior and type consistency.

This documentation serves as a comprehensive guide for developers looking to utilize the `exclusive_function_system` module of the `bevy_ecs` library to customize system behavior in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.
