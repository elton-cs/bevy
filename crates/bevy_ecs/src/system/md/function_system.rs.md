# Bevy ECS Function System Documentation

This documentation provides an overview of the public functions, structs, enumerations, traits, and implementations available in the `function_system.rs` file of the Bevy ECS library. This information is intended to assist developers in building Bevy applications or games.

## Structs

### `SystemMeta`
- **Description**: Holds metadata for a system, including its name, access permissions, and execution state.
- **Fields**:
  - `name`: The name of the system.
  - `component_access_set`: Tracks component access for soundness and scheduling.
  - `archetype_component_access`: Determines parallel execution capabilities.
  - `is_send`: Indicates if the system can be sent across threads.
  - `has_deferred`: Indicates if the system has deferred parameters.
  - `last_run`: The last tick when the system was executed.
  - `param_warn_policy`: Policy for warning about parameter access issues.

### `ParamWarnPolicy`
- **Description**: Enum representing the warning policy for system parameters.
- **Variants**:
  - `Panic`: The application will panic on access issues.
  - `Never`: No warnings will be emitted.
  - `Once`: A warning will be emitted once, then suppressed.

### `SystemState<Param>`
- **Description**: Holds the state required to manage system parameters for a system.
- **Generic Parameter**: `Param` must implement `SystemParam`.
- **Fields**:
  - `meta`: Metadata for the system.
  - `param_state`: State of the system parameters.
  - `world_id`: Identifier for the world the system is associated with.
  - `archetype_generation`: Tracks the generation of archetypes.

## Traits

### `WithParamWarnPolicy<M, F>`
- **Description**: Trait for manipulating the warning policy of systems.
- **Methods**:
  - `with_param_warn_policy`: Sets the warning policy for the system.
  - `param_warn_once`: Sets the policy to emit a warning only once.
  - `never_param_warn`: Disables all parameter warnings.

### `SystemParamFunction<Marker>`
- **Description**: Trait for functions that can be used as systems.
- **Associated Types**:
  - `In`: The input type for the system.
  - `Out`: The output type for the system.
  - `Param`: The parameters used to access the world.
- **Methods**:
  - `run`: Executes the system with the provided input and parameters.

## Implementations

### `SystemMeta`
- **`new<T>()`**: Creates a new `SystemMeta` instance for a given type `T`.
- **`name(&self) -> &str`**: Returns the name of the system.
- **`set_name(&mut self, new_name: impl Into<Cow<'static, str>>)`**: Sets a new name for the system.
- **`is_send(&self) -> bool`**: Checks if the system is sendable.
- **`set_non_send(&mut self)`**: Marks the system as non-sendable (irreversible).
- **`has_deferred(&self) -> bool`**: Checks if the system has deferred parameters.
- **`set_has_deferred(&mut self)`**: Marks the system as having deferred parameters.
- **`archetype_component_access(&self) -> &Access<ArchetypeComponentId>`**: Returns access information for archetype components.
- **`component_access_set(&self) -> &FilteredAccessSet<ComponentId>`**: Returns the access set for components.

### `ParamWarnPolicy`
- **`advance(&mut self)`**: Advances the warning policy after a validation failure.
- **`try_warn<P>(&self, name: &str)`**: Emits a warning about inaccessible system parameters based on the current policy.

### `SystemState<Param>`
- **`new(world: &mut World) -> Self`**: Creates a new `SystemState` with default state.
- **`get<'w, 's>(&'s mut self, world: &'w World) -> SystemParamItem<'w, 's, Param>`**: Retrieves read-only system parameters.
- **`get_mut<'w, 's>(&'s mut self, world: &'w mut World) -> SystemParamItem<'w, 's, Param>`**: Retrieves mutable system parameters.
- **`apply(&mut self, world: &mut World)`**: Applies queued state for system parameters to the world.
- **`update_archetypes(&mut self, world: &World)`**: Updates the internal view of the world's archetypes.

### `FunctionSystem<Marker, F>`
- **`with_name(mut self, new_name: impl Into<Cow<'static, str>>) -> Self`**: Returns the system with a new name.
- **`initialize(&mut self, world: &mut World)`**: Initializes the system with the provided world.
- **`run_unsafe(&mut self, input: SystemIn<'_, Self>, world: UnsafeWorldCell) -> Self::Out`**: Executes the system unsafely.
- **`apply_deferred(&mut self, world: &mut World)`**: Applies deferred commands to the world.

## Usage Insights
- Use `SystemMeta` to manage system metadata, including naming and access control.
- Implement `ParamWarnPolicy` to control how your systems handle parameter access warnings.
- Utilize `SystemState` to manage system parameters effectively, ensuring proper access and state management.
- Create systems using `FunctionSystem` to encapsulate logic that operates on ECS components and resources.

This documentation serves as a comprehensive guide for developers looking to leverage the Bevy ECS system functionality effectively in their applications or games.
