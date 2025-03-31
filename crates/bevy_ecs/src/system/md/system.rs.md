# Bevy ECS System Documentation

This documentation provides an overview of the public functions, structs, enumerations, traits, and implementations available in the Bevy ECS system module. It serves as a guide for developers to effectively utilize these components when building Bevy applications or games.

## Traits

### `System`
- **Description**: Represents an ECS system that can be added to a `Schedule`.
- **Usage**:
  - Systems are functions that take parameters implementing `SystemParam`.
  - Added to an application using `App::add_systems(Update, my_system)`.
  - Systems run once per pass of the main loop, executed in parallel with automatic data access management.
  
- **Associated Types**:
  - `type In`: The system's input type, which must implement `SystemInput`.
  - `type Out`: The system's output type.

- **Methods**:
  - `fn name(&self) -> Cow<'static, str>`: Returns the name of the system.
  - `fn type_id(&self) -> TypeId`: Returns the `TypeId` of the underlying system type.
  - `fn component_access(&self) -> &Access<ComponentId>`: Returns the system's component access.
  - `fn archetype_component_access(&self) -> &Access<ArchetypeComponentId>`: Returns the system's archetype component access.
  - `fn is_send(&self) -> bool`: Checks if the system is `Send`.
  - `fn is_exclusive(&self) -> bool`: Checks if the system must run exclusively.
  - `fn has_deferred(&self) -> bool`: Checks if the system has deferred buffers.
  - `unsafe fn run_unsafe(&mut self, input: SystemIn<'_, Self>, world: UnsafeWorldCell) -> Self::Out`: Runs the system with the given input in the world, allowing unsafe parallel execution.
  - `fn run(&mut self, input: SystemIn<'_, Self>, world: &mut World) -> Self::Out`: Runs the system safely, applying deferred parameters immediately.
  - `fn apply_deferred(&mut self, world: &mut World)`: Applies deferred system parameters to the world.
  - `fn queue_deferred(&mut self, world: DeferredWorld)`: Enqueues deferred parameters into the world's command buffer.
  - `unsafe fn validate_param_unsafe(&mut self, world: UnsafeWorldCell) -> bool`: Validates parameters for the system, ensuring it can run without panic.
  - `fn validate_param(&mut self, world: &World) -> bool`: Safe version of `validate_param_unsafe`.
  - `fn initialize(&mut self, _world: &mut World)`: Initializes the system.
  - `fn update_archetype_component_access(&mut self, world: UnsafeWorldCell)`: Updates the system's archetype component access.
  - `fn check_change_tick(&mut self, change_tick: Tick)`: Checks and wraps ticks for change detection.
  - `fn default_system_sets(&self) -> Vec<InternedSystemSet>`: Returns the system's default system sets.
  - `fn get_last_run(&self) -> Tick`: Gets the tick indicating the last time the system ran.
  - `fn set_last_run(&mut self, last_run: Tick)`: Overwrites the last run tick.

### `ReadOnlySystem`
- **Description**: A trait for systems that do not modify the `World` when run.
- **Usage**: Implemented for systems whose parameters all implement `ReadOnlySystemParam`.
- **Methods**:
  - `fn run_readonly(&mut self, input: SystemIn<'_, Self>, world: &World) -> Self::Out`: Runs the system with a shared reference to the world.

## Type Aliases

### `BoxedSystem`
- **Description**: A convenience type alias for a boxed `System` trait object.
- **Usage**: `pub type BoxedSystem<In = (), Out = ()> = Box<dyn System<In = In, Out = Out>>;`

## Functions

### `check_system_change_tick`
- **Description**: Checks if the system has not run for a specified number of ticks and logs a warning if so.
- **Parameters**:
  - `last_run: &mut Tick`: The last tick when the system ran.
  - `this_run: Tick`: The current tick.
  - `system_name: &str`: The name of the system.

## Enums

### `RunSystemError`
- **Description**: Represents errors that occur when running a system.
- **Variants**:
  - `InvalidParams(Cow<'static, str>)`: Indicates that the system could not run due to invalid parameters.

## Implementations

### `Debug` for `RunSystemError`
- **Description**: Provides a debug implementation for `RunSystemError`.
- **Methods**:
  - `fn fmt(&self, f: &mut core::fmt::Formatter<'_>) -> core::fmt::Result`: Formats the error for debugging.

### `RunSystemOnce`
- **Description**: Trait used to run a system immediately on a `World`.
- **Methods**:
  - `fn run_system_once<T, Out, Marker>(self, system: T) -> Result<Out, RunSystemError>`: Tries to run a system and apply its deferred parameters.
  - `fn run_system_once_with<T, In, Out, Marker>(self, input: SystemIn<'_, T::System>, system: T) -> Result<Out, RunSystemError>`: Tries to run a system with given input.

### `RunSystemOnce` for `&mut World`
- **Description**: Implementation of `RunSystemOnce` for mutable references to `World`.
- **Methods**:
  - `fn run_system_once_with<T, In, Out, Marker>(self, input: SystemIn<'_, T::System>, system: T) -> Result<Out, RunSystemError>`: Executes the system with the provided input.

## Testing

### Tests
- **Description**: Contains unit tests for various functionalities of the system.
- **Examples**:
  - `run_system_once`: Tests running a system once.
  - `run_two_systems`: Tests running two systems sequentially.
  - `command_processing`: Tests command processing in the world.
  - `non_send_resources`: Tests handling of non-send resources.
  - `run_system_once_invalid_params`: Tests behavior when running a system with invalid parameters.

This documentation should provide sufficient context for developers to understand and utilize the Bevy ECS system effectively in their applications and games.
