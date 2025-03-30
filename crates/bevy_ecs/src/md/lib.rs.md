# Bevy ECS Library Documentation

This document provides a comprehensive overview of the public API available in the `bevy_ecs` library. It includes details on modules, traits, structs, and their usage that can be utilized to build applications or games using Bevy.

## Modules

### `archetype`
- **Description**: Contains types and functionality related to archetypes, which are used to manage entities and their components efficiently.

### `batching`
- **Description**: Provides types for controlling batching behavior during parallel processing.

### `bundle`
- **Description**: Contains the `Bundle` trait and helper types for managing groups of components.

### `change_detection`
- **Description**: Provides types and traits for detecting changes in components and resources.

### `component`
- **Description**: Contains types for declaring and storing components, which are the building blocks of entities.

### `entity`
- **Description**: Manages entities and their relationships with components.

### `event`
- **Description**: Provides functionality for event handling within the ECS.

### `identifier`
- **Description**: Contains types for unique identification of entities and components.

### `intern`
- **Description**: Provides types used to statically intern immutable values, optimizing memory usage and performance.

### `label`
- **Description**: Contains traits used by label implementations for categorizing entities and components.

### `observer`
- **Description**: Provides functionality for observing changes in entities and components.

### `query`
- **Description**: Contains types and functionality for querying entities and their components.

### `reflect`
- **Description**: Provides reflection capabilities for components and resources (enabled with the `bevy_reflect` feature).

### `removal_detection`
- **Description**: Provides types and functionality for detecting when components are removed from entities.

### `schedule`
- **Description**: Manages the scheduling of systems and their execution order.

### `storage`
- **Description**: Contains types for storing components, including tables and sparse sets.

### `system`
- **Description**: Provides types and functionality for defining and managing systems that operate on entities and components.

### `traversal`
- **Description**: Contains types and functionality for traversing entities and their components.

### `world`
- **Description**: Manages the overall state of the ECS, including entities, components, and systems.

## Traits

### `Component`
- **Description**: A trait for types that can be used to store data for an entity.
- **Key Points**:
  - Components must implement `Send + Sync + 'static`.
  - Components can specify required components that will be automatically initialized when the component is added to an entity.

### `DynEq`
- **Description**: An object-safe version of the `Eq` trait.
- **Key Methods**:
  - `as_any(&self) -> &dyn Any`: Casts the type to `dyn Any`.
  - `dyn_eq(&self, other: &dyn DynEq) -> bool`: Tests for equality between `self` and `other.

### `DynHash`
- **Description**: An object-safe version of the `Hash` trait.
- **Key Methods**:
  - `as_dyn_eq(&self) -> &dyn DynEq`: Casts the type to `dyn DynEq`.
  - `dyn_hash(&self, state: &mut dyn Hasher)`: Feeds this value into the given hasher.

## Structs

### `Interned<T>`
- **Description**: An interned value that remains valid until the end of the program and will not drop.
- **Fields**:
  - `0`: A static reference to the interned value.

### `Interner<T>`
- **Description**: A thread-safe interner used to create `Interned<T>` from `&T`.
- **Fields**:
  - `0`: A `OnceLock` containing a `RwLock` for a set of interned values.
- **Methods**:
  - `new() -> Self`: Creates a new empty interner.
  - `intern(&self, value: &T) -> Interned<T>`: Returns the `Interned<T>` corresponding to `value`.

### `ComponentId`
- **Description**: A value that uniquely identifies the type of a component or resource within a world.
- **Fields**:
  - `0`: The index of the component.
- **Methods**:
  - `new(index: usize) -> ComponentId`: Creates a new `ComponentId`.
  - `index(self) -> usize`: Returns the index of the current component.

### `ComponentDescriptor`
- **Description**: A value describing a component or resource, which may or may not correspond to a Rust type.
- **Fields**:
  - `name`: The name of the component.
  - `storage_type`: The storage strategy for the component.
  - `is_send_and_sync`: Indicates if the component can be freely shared between threads.
  - `type_id`: The `TypeId` of the underlying component type.
  - `layout`: The layout used to store values of this component in memory.

### `ComponentInfo`
- **Description**: Stores metadata associated with each kind of component in a given world.
- **Fields**:
  - `id`: The unique identifier for the component.
  - `descriptor`: The descriptor for the component.
  - `hooks`: The hooks associated with the component.
  - `required_components`: The required components for this component.
  - `required_by`: The components that require this component.

### `Components`
- **Description**: Stores metadata for a type of component or resource stored in a specific world.
- **Fields**:
  - `components`: A vector of `ComponentInfo`.
  - `indices`: A map of `TypeId` to `ComponentId`.
  - `resource_indices`: A map of `TypeId` to `ComponentId` for resources.
- **Methods**:
  - `register_component<T: Component>(&mut self, storages: &mut Storages) -> ComponentId`: Registers a component of type `T`.
  - `get_info(&self, id: ComponentId) -> Option<&ComponentInfo>`: Gets the metadata associated with the given component.

## Example Usage

### Defining a Component
```rust
use bevy_ecs::component::Component;

#[derive(Component)]
struct Position(f32, f32);

#[derive(Component)]
struct Velocity(f32, f32);
```

### Using the Interner
```rust
use bevy_ecs::intern::{Interner, Interned};

fn main() {
    let interner = Interner::new();
    let value = interner.intern("Hello");
    println!("{:?}", value);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `bevy_ecs` library to manage components and intern values in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.