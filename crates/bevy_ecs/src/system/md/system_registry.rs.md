# Bevy ECS System Registry Documentation

This documentation provides an overview of the public functions, structs, traits, and implementations available in the `system_registry.rs` file of the Bevy ECS library. This information is intended to assist developers in building Bevy applications or games.

## Structs

### `RegisteredSystem<I, O>`
- **Description**: A small wrapper for `BoxedSystem` that tracks whether the system has been initialized.
- **Fields**:
  - `initialized`: A boolean indicating if the system has been initialized.
  - `system`: The boxed system that this struct wraps.

### `SystemIdMarker`
- **Description**: A marker component for identifying `SystemId` entities.
- **Usage**: Used to mark entities that represent systems in the ECS.

### `RemovedSystem<I, O>`
- **Description**: Represents a system that has been removed from the registry, containing the system and its initialization state.
- **Fields**:
  - `initialized`: Indicates if the system was initialized.
  - `system`: The boxed system that was removed.

### `SystemId<I, O>`
- **Description**: An identifier for a registered system, keyed to a specific `World`.
- **Fields**:
  - `entity`: The entity associated with the system.
  - `marker`: A phantom data marker for type safety.

### `CachedSystemId<S>`
- **Description**: A cached `SystemId` distinguished by the unique function type of its system.
- **Fields**:
  - `0`: The `SystemId` associated with the cached system.

## Functions

### `system_bundle<I, O>(system: BoxedSystem<I, O>) -> impl Bundle`
- **Description**: Creates a bundle for a one-shot system entity.
- **Usage**: Used to create a bundle that includes a `RegisteredSystem` and a `SystemIdMarker`.

### `World::register_system<I, O, M>(&mut self, system: impl IntoSystem<I, O, M> + 'static) -> SystemId<I, O>`
- **Description**: Registers a system and returns a `SystemId` for later calls.
- **Usage**: Call this method to register a system that can be run later using its `SystemId`.

### `World::register_boxed_system<I, O>(&mut self, system: BoxedSystem<I, O>) -> SystemId<I, O>`
- **Description**: Registers a boxed system and returns its `SystemId`.
- **Usage**: Useful for systems that have already been converted into a `BoxedSystem`.

### `World::unregister_system<I, O>(&mut self, id: SystemId<I, O>) -> Result<RemovedSystem<I, O>, RegisteredSystemError<I, O>>`
- **Description**: Removes a registered system and returns it if it exists.
- **Usage**: Call this method to unregister a system using its `SystemId`.

### `World::run_system<O: 'static>(&mut self, id: SystemId<(), O>) -> Result<O, RegisteredSystemError<(), O>>`
- **Description**: Runs a stored system by its `SystemId`.
- **Usage**: Use this method to execute a system that has been registered.

### `World::run_system_with_input<I, O>(&mut self, id: SystemId<I, O>, input: I::Inner<'_>) -> Result<O, RegisteredSystemError<I, O>>`
- **Description**: Runs a stored chained system by its `SystemId`, providing an input value.
- **Usage**: Call this method to run a system that requires input parameters.

### `World::register_system_cached<I, O, M, S>(&mut self, system: S) -> SystemId<I, O>`
- **Description**: Registers a system or returns its cached `SystemId`.
- **Usage**: Use this method to register a system that can be reused without re-registering.

### `World::unregister_system_cached<I, O, M, S>(&mut self, _system: S) -> Result<RemovedSystem<I, O>, RegisteredSystemError<I, O>>`
- **Description**: Removes a cached system and its `CachedSystemId` resource.
- **Usage**: Call this method to unregister a cached system.

### `World::run_system_cached<O: 'static, M, S: IntoSystem<(), O, M> + 'static>(&mut self, system: S) -> Result<O, RegisteredSystemError<(), O>>`
- **Description**: Runs a cached system, registering it if necessary.
- **Usage**: Use this method to execute a cached system.

### `World::run_system_cached_with<I, O, M, S>(&mut self, system: S, input: I::Inner<'_>) -> Result<O, RegisteredSystemError<I, O>>`
- **Description**: Runs a cached system with an input, registering it if necessary.
- **Usage**: Call this method to run a cached system that requires input parameters.

## Traits

### `Command`
- **Description**: A trait for types that can be executed as commands in the ECS.
- **Usage**: Implement this trait for custom commands that modify the world state.

## Enumerations

### `RegisteredSystemError<I, O>`
- **Description**: An enumeration representing errors that can occur with registered systems.
- **Variants**:
  - `SystemIdNotRegistered(SystemId<I, O>)`: Indicates that a system was not found by its ID.
  - `SystemNotCached`: Indicates that a cached system was not found.
  - `Recursive(SystemId<I, O>)`: Indicates that a system tried to run itself recursively.
  - `SelfRemove(SystemId<I, O>)`: Indicates that a system tried to remove itself.
  - `InvalidParams(SystemId<I, O>)`: Indicates that the system could not run due to invalid parameters.

## Insights
- Use `World::register_system` to add systems to your ECS, allowing for flexible and dynamic behavior in your game.
- Utilize `SystemId` to manage and reference systems efficiently, enabling easy execution and modification.
- Implement `Command` for custom commands that can be executed within the ECS, providing a powerful way to manipulate the world state.
- Leverage `RegisteredSystemError` to handle errors gracefully when working with systems, ensuring robust application behavior.

This documentation serves as a comprehensive guide for developers looking to leverage the Bevy ECS system functionality effectively in their applications or games.
