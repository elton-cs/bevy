# Bevy ECS System Parameter Documentation

This documentation provides an overview of the public functions, structs, traits, and implementations available in the `bevy_ecs` library for building Bevy applications or games. Each concept is explained in detail to assist developers in utilizing the library effectively.

## Public Functions and Structs

### SystemParam
- **Description**: A trait that defines a parameter that can be used in a Bevy system.
- **Usage**: Implement this trait for custom parameters that need to interact with the Bevy world.
- **Key Methods**:
  - `init_state(world: &mut World, system_meta: &mut SystemMeta)`: Initializes the state for the system parameter.
  - `get_param<'world, 'state>(...)`: Retrieves the parameter for the current system execution.

### Res
- **Description**: A system parameter that provides read access to a resource of type `T`.
- **Usage**: Use `Res<T>` to access resources that are stored in the Bevy world.
- **Key Methods**:
  - `init_state(...)`: Registers the resource type with the world.
  - `get_param(...)`: Retrieves the resource, ensuring it exists.

### ResMut
- **Description**: A system parameter that provides mutable access to a resource of type `T`.
- **Usage**: Use `ResMut<T>` to modify resources stored in the Bevy world.
- **Key Methods**:
  - `init_state(...)`: Registers the resource type with the world.
  - `get_param(...)`: Retrieves the mutable resource.

### NonSend
- **Description**: A system parameter for accessing non-`Send` resources safely.
- **Usage**: Use `NonSend<T>` when you need to access resources that cannot be sent across threads.
- **Key Methods**:
  - `init_state(...)`: Registers the non-send resource type.
  - `get_param(...)`: Retrieves the non-send resource.

### Local
- **Description**: A system parameter that provides access to local data unique to each system invocation.
- **Usage**: Use `Local<T>` to store data that should not be shared between systems.
- **Key Methods**:
  - `init_state(...)`: Initializes the local state.
  - `get_param(...)`: Retrieves the local data.

### Query
- **Description**: A system parameter that allows querying entities in the Bevy world based on specified criteria.
- **Usage**: Use `Query<'w, 's, D, F>` to access entities that match the data type `D` and filter `F`.
- **Key Methods**:
  - `init_state(...)`: Initializes the query state.
  - `get_param(...)`: Retrieves the query results.

### Single
- **Description**: A system parameter that retrieves a single entity matching a query.
- **Usage**: Use `Single<'a, D, F>` when you expect exactly one entity to match the query.
- **Key Methods**:
  - `init_state(...)`: Initializes the single query state.
  - `get_param(...)`: Retrieves the single entity.

### ParamSet
- **Description**: A collection of potentially conflicting system parameters that allows safe access to multiple parameters.
- **Usage**: Use `ParamSet` to manage multiple parameters that may conflict with each other.
- **Key Methods**:
  - `get_mut(...)`: Accesses a specific parameter in the set.
  - `for_each(...)`: Iterates over each parameter in the set.

### DynSystemParam
- **Description**: A dynamic system parameter that can be configured at runtime.
- **Usage**: Use `DynSystemParam` for parameters that need to be determined at runtime.
- **Key Methods**:
  - `is<T: SystemParam>(&self)`: Checks if the inner parameter is of type `T`.
  - `downcast<T: SystemParam>(self)`: Attempts to downcast to the specified parameter type.

## Traits

### ReadOnlySystemParam
- **Description**: A trait for system parameters that only read data from the world.
- **Usage**: Implement this trait for parameters that do not modify the world state.

### Resource
- **Description**: A trait for types that can be stored as resources in the Bevy world.
- **Usage**: Derive this trait for any struct that should be treated as a resource.

### SystemBuffer
- **Description**: A trait for types that can be used to store deferred mutations to the world.
- **Usage**: Implement this trait for types that need to queue changes to be applied later.

## Examples

### Using Res and ResMut
