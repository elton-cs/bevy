# Bevy ECS System Builder Documentation

This documentation provides an overview of the public functions, structs, traits, and implementations available in the Bevy ECS system builder module. It serves as a guide for developers to effectively utilize these components when building a Bevy application or game.

## Concepts

### 1. `SystemParamBuilder<P: SystemParam>`
- **Description**: A trait for building system parameters in Bevy ECS.
- **Usage**:
  - Implement this trait for custom system parameters to define how they are built and initialized.
  - Use the `build` method to register world accesses and create a new instance of the parameter's state.
  - The `build_state` method can be used to create a `SystemState` from a `SystemParamBuilder`.

### 2. `ParamBuilder`
- **Description**: A builder for creating system parameters with default initialization.
- **Usage**:
  - Use `ParamBuilder` to create parameters that do not require special building.
  - Factory methods like `of<T>()`, `resource()`, `resource_mut()`, `local()`, `query()`, and `query_filtered()` provide easy access to common parameter types.

### 3. `QueryParamBuilder<T>`
- **Description**: A builder for creating queries in Bevy ECS.
- **Usage**:
  - Use `QueryParamBuilder::new()` to create a query with a callback that configures the `QueryBuilder`.
  - This allows for adding filters and configuring components available to `FilteredEntityRef` or `FilteredEntityMut`.

### 4. `LocalBuilder<T>`
- **Description**: A builder for creating local parameters with an initial value.
- **Usage**:
  - Use `LocalBuilder` to define a local parameter that holds a value, which can be accessed in systems.

### 5. `FilteredResourcesParamBuilder<T>`
- **Description**: A builder for creating filtered resources.
- **Usage**:
  - Use this builder to configure which resources can be accessed in a system.
  - Accepts a callback to configure the `FilteredResourcesBuilder`.

### 6. `FilteredResourcesMutParamBuilder<T>`
- **Description**: A builder for creating mutable filtered resources.
- **Usage**:
  - Similar to `FilteredResourcesParamBuilder`, but allows for mutable access to resources.

### 7. `DynParamBuilder<'a>`
- **Description**: A builder for creating dynamic system parameters.
- **Usage**:
  - Wrap a `SystemParamBuilder` of any type to create a `DynSystemParam` that can be downcast to the specific parameter type.

### 8. `ParamSetBuilder<T>`
- **Description**: A builder for creating parameter sets.
- **Usage**:
  - Use `ParamSetBuilder` to create a set of parameters, either as a tuple or a vector.
  - This allows for grouping multiple parameters together for system access.

### 9. `SystemState`
- **Description**: Represents the state of a system in Bevy ECS.
- **Usage**:
  - Use `SystemState::build_system()` to create a system from the state.

### 10. `World`
- **Description**: Represents the Bevy ECS world.
- **Usage**:
  - Use `World` to manage entities, components, and resources in your game.

### 11. `Resource`
- **Description**: A trait for types that can be stored in the Bevy ECS world as resources.
- **Usage**:
  - Implement this trait for any type that you want to store as a resource in the world.

### 12. `Component`
- **Description**: A trait for types that can be attached to entities in Bevy ECS.
- **Usage**:
  - Implement this trait for any type that you want to use as a component in your entities.

### 13. `Query`
- **Description**: A type for querying entities with specific components.
- **Usage**:
  - Use `Query` to access and iterate over entities that match certain criteria.

### 14. `FilteredEntityRef` and `FilteredEntityMut`
- **Description**: Types for referencing entities with specific filters applied.
- **Usage**:
  - Use these types to work with entities that match certain component criteria while ensuring safe access.

## Safety
- Implementors of `SystemParamBuilder` must ensure that:
  - The `build` method correctly registers all world accesses used by the parameter.
  - No world accesses conflict with prior accesses registered on `SystemMeta`.

This documentation should provide sufficient context for developers to effectively use the Bevy ECS system builder module when creating games or applications.
