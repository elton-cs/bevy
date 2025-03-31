# Exclusive System Parameter Documentation

This documentation provides an overview of the public functions, structs, traits, and implementations available in the `exclusive_system_param.rs` file of the Bevy ECS library. It serves as a guide for developers to effectively utilize these components when building a Bevy application or game.

## Traits

### `ExclusiveSystemParam`
- **Description**: A parameter that can be used in an exclusive system (a system with an `&mut World` parameter). Any parameters implementing this trait must come after the `&mut World` parameter.
- **Associated Types**:
  - `State`: Used to store data which persists across invocations of a system. Must implement `Send + Sync + 'static`.
  - `Item<'s>`: The item type returned when constructing this system param. Must implement `ExclusiveSystemParam<State = Self::State>`.
- **Methods**:
  - `fn init(world: &mut World, system_meta: &mut SystemMeta) -> Self::State`
    - **Description**: Creates a new instance of this param's `State`.
    - **Usage**: Call this method to initialize the state for the exclusive system parameter.
  - `fn get_param<'s>(state: &'s mut Self::State, system_meta: &SystemMeta) -> Self::Item<'s>`
    - **Description**: Creates a parameter to be passed into an `ExclusiveSystemParamFunction`.
    - **Usage**: Use this method to retrieve the parameter for the exclusive system function.

## Type Aliases

### `ExclusiveSystemParamItem<'s, P>`
- **Description**: Shorthand way of accessing the associated type `ExclusiveSystemParam::Item` for a given `ExclusiveSystemParam`.
- **Usage**: Use this type alias to simplify the syntax when working with exclusive system parameters.

## Implementations

### `ExclusiveSystemParam` for `&'a mut QueryState<D, F>`
- **Description**: Implementation for mutable references to `QueryState`.
- **Methods**:
  - `fn init(world: &mut World, _system_meta: &mut SystemMeta) -> Self::State`
    - **Returns**: A new `QueryState` initialized with the provided world.
  - `fn get_param<'s>(state: &'s mut Self::State, _system_meta: &SystemMeta) -> Self::Item<'s>`
    - **Returns**: The mutable reference to the `QueryState`.

### `ExclusiveSystemParam` for `&'a mut SystemState<P>`
- **Description**: Implementation for mutable references to `SystemState`.
- **Methods**:
  - `fn init(world: &mut World, _system_meta: &mut SystemMeta) -> Self::State`
    - **Returns**: A new `SystemState` initialized with the provided world.
  - `fn get_param<'s>(state: &'s mut Self::State, _system_meta: &SystemMeta) -> Self::Item<'s>`
    - **Returns**: The mutable reference to the `SystemState`.

### `ExclusiveSystemParam` for `Local<'_s, T>`
- **Description**: Implementation for local parameters that can be used in exclusive systems.
- **Methods**:
  - `fn init(world: &mut World, _system_meta: &mut SystemMeta) -> Self::State`
    - **Returns**: A new `SyncCell` initialized with the value from `T::from_world(world)`.
  - `fn get_param<'s>(state: &'s mut Self::State, _system_meta: &SystemMeta) -> Self::Item<'s>`
    - **Returns**: A `Local` instance containing the value from the `SyncCell`.

### `ExclusiveSystemParam` for `PhantomData<S>`
- **Description**: Implementation for `PhantomData`, used to indicate that a type is not used directly.
- **Methods**:
  - `fn init(_world: &mut World, _system_meta: &mut SystemMeta) -> Self::State`
    - **Returns**: An empty state.
  - `fn get_param<'s>(_state: &'s mut Self::State, _system_meta: &SystemMeta) -> Self::Item<'s>`
    - **Returns**: A `PhantomData` instance.

## Macro

### `impl_exclusive_system_param_tuple`
- **Description**: A macro that implements the `ExclusiveSystemParam` trait for tuples of parameters.
- **Usage**: Use this macro to create implementations for tuples containing multiple exclusive system parameters.

## Tests

### `test_exclusive_system_params`
- **Description**: A test function to verify the behavior of exclusive system parameters.
- **Usage**: This test initializes a resource and runs a system that modifies the resource, ensuring the expected behavior of the exclusive system parameters.

This documentation serves as a comprehensive guide for developers looking to utilize the `exclusive_system_param` module of the `bevy_ecs` library to customize system behavior in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.
