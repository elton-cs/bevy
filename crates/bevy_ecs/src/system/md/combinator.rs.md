# Bevy ECS Combinator Documentation

This documentation provides an overview of the public functions, structs, traits, and implementations available in the `combinator.rs` file of the Bevy ECS library. It serves as a guide for developers to understand how to utilize these components when building a Bevy application or game.

## Traits

### `Combine<A, B>`
- **Description**: A trait that customizes the behavior of a `CombinatorSystem` by defining how two systems can be combined.
- **Associated Types**:
  - `In`: The input type for the combinator system, which must implement `SystemInput`.
  - `Out`: The output type for the combinator system.
- **Methods**:
  - `combine(input: <Self::In as SystemInput>::Inner<'_>, a: impl FnOnce(SystemIn<'_, A>) -> A::Out, b: impl FnOnce(SystemIn<'_, B>) -> B::Out) -> Self::Out`
    - **Description**: Combines the outputs of two systems based on the provided input. This method is essential for defining how the two systems interact and produce a final output.

## Structs

### `CombinatorSystem<Func, A, B>`
- **Description**: A system that combines two other systems, with behavior specified by the `Combine` trait.
- **Fields**:
  - `_marker`: A phantom data marker to ensure type safety.
  - `a`: The first system to be combined.
  - `b`: The second system to be combined.
  - `name`: The name of the combined system.
  - `component_access`: Access control for components used by the system.
  - `archetype_component_access`: Access control for archetype components used by the system.
- **Methods**:
  - `new(a: A, b: B, name: Cow<'static, str>) -> Self`
    - **Description**: Creates a new `CombinatorSystem` that combines two inner systems. The `Func` type must implement `Combine<A, B>`.
  - `name(&self) -> Cow<'static, str>`
    - **Description**: Returns the name of the system.
  - `component_access(&self) -> &Access<ComponentId>`
    - **Description**: Provides access information for the components used by the system.
  - `archetype_component_access(&self) -> &Access<ArchetypeComponentId>`
    - **Description**: Provides access information for the archetype components used by the system.
  - `run(&mut self, input: SystemIn<'_, Self>, world: &mut World) -> Self::Out`
    - **Description**: Executes the combined system, processing the input and returning the output.
  - `initialize(&mut self, world: &mut World)`
    - **Description**: Initializes the system, preparing it for execution.
  - `apply_deferred(&mut self, world: &mut World)`
    - **Description**: Applies any deferred actions from the combined systems.
  - `check_change_tick(&mut self, change_tick: Tick)`
    - **Description**: Checks for changes in the tick count for both systems.

### `IntoPipeSystem<A, B>`
- **Description**: A struct that creates an instance of a `PipeSystem` by combining two inner systems.
- **Fields**:
  - `a`: The first system to be piped.
  - `b`: The second system to be piped.
- **Methods**:
  - `new(a: A, b: B) -> Self`
    - **Description**: Creates a new `IntoPipeSystem` that pipes two inner systems.

### `PipeSystem<A, B>`
- **Description**: A system created by piping the output of the first system into the input of the second.
- **Fields**:
  - `a`: The first system in the pipe.
  - `b`: The second system in the pipe.
  - `name`: The name of the pipe system.
  - `component_access`: Access control for components used by the pipe system.
  - `archetype_component_access`: Access control for archetype components used by the pipe system.
- **Methods**:
  - `new(a: A, b: B, name: Cow<'static, str>) -> Self`
    - **Description**: Creates a new `PipeSystem` that pipes two inner systems.
  - `run(&mut self, input: SystemIn<'_, Self>, world: &mut World) -> Self::Out`
    - **Description**: Executes the pipe system, processing the input from the first system and passing it to the second system.

## Implementations

### `Clone` for `CombinatorSystem<Func, A, B>`
- **Description**: Allows cloning of the `CombinatorSystem`. The cloned instance must be initialized before it can run.
- **Method**:
  - `clone(&self) -> Self`
    - **Description**: Clones the combined system, creating a new instance with the same inner systems and name.

### `ReadOnlySystem` for `CombinatorSystem<Func, A, B>`
- **Description**: Indicates that the `CombinatorSystem` is read-only, meaning it does not modify the world state.

### `IntoSystem` for `IntoPipeSystem<A, B>`
- **Description**: Converts an `IntoPipeSystem` into a `PipeSystem`.
- **Method**:
  - `into_system(this: Self) -> Self::System`
    - **Description**: Creates a `PipeSystem` from the two inner systems defined in `IntoPipeSystem`.

## Usage Example
- The `CombinatorSystem` can be used to create complex systems by combining simpler ones, allowing for flexible and reusable system definitions in a Bevy application.
- The `PipeSystem` allows for chaining systems together, where the output of one system directly feeds into the next, enabling a streamlined data flow.

This documentation serves as a comprehensive guide for developers looking to leverage the combinator and pipe systems in their Bevy applications, providing the necessary context and examples to facilitate effective usage.
