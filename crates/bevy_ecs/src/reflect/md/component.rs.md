# Bevy ECS Reflect Component Module Documentation

This document provides a comprehensive overview of the public API available in the `reflect/component` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to manage components in a Bevy application or game.

## Overview

- **Purpose**: This module allows for the insertion, updating, removal, and general interaction with components whose types are only known at runtime. It provides reflection capabilities for components, enabling dynamic manipulation of entity components.

## Structs

### `ReflectComponent`
- **Description**: A struct used to operate on the reflected `Component` trait of a type.
- **Key Points**:
  - Can be obtained via `bevy_reflect::TypeRegistration::data`.
  - Wraps a `ReflectComponentFns` struct, providing methods to interact with components dynamically.

### `ReflectComponentFns`
- **Description**: Contains raw function pointers needed to implement a `ReflectComponent`.
- **Key Points**:
  - Includes function pointers for operations such as `insert`, `apply`, `apply_or_insert`, `remove`, `contains`, `reflect`, `reflect_mut`, `reflect_unchecked_mut`, `copy`, and `register_component`.
  - Allows for the creation of custom implementations of `ReflectComponent`.

## Implementations

### `ReflectComponentFns::new`
- **Description**: Creates a default set of `ReflectComponentFns` for a specific component type using its `FromType` implementation.
- **Usage**: Useful for starting with default implementations before overriding specific functions for custom behavior.

### `ReflectComponent::insert`
- **Description**: Inserts a reflected `Component` into an entity.
- **Parameters**:
  - `entity`: The entity to which the component will be inserted.
  - `component`: The reflected component to insert.
  - `registry`: The type registry for type information.
- **Usage**: Call this method to add a component to an entity dynamically.

### `ReflectComponent::apply`
- **Description**: Sets the value of a `Component` type in the entity to the given value.
- **Panics**: Will panic if there is no `Component` of the given type.
- **Usage**: Use this method to update an existing component in an entity.

### `ReflectComponent::apply_or_insert`
- **Description**: Sets the value of a `Component` type in the entity to the given value or inserts a new one if it does not exist.
- **Usage**: This method is useful for ensuring that a component is present in an entity, either by updating it or inserting it if absent.

### `ReflectComponent::remove`
- **Description**: Removes a `Component` type from the entity.
- **Usage**: Call this method to remove a component from an entity. It does nothing if the component does not exist.

### `ReflectComponent::contains`
- **Description**: Returns whether the entity contains this `Component`.
- **Parameters**:
  - `entity`: The entity to check for the component.
- **Usage**: Use this method to verify the presence of a component in an entity.

### `ReflectComponent::reflect`
- **Description**: Gets the value of this `Component` type from the entity as a reflected reference.
- **Parameters**:
  - `entity`: The entity to retrieve the component from.
- **Returns**: An `Option<&dyn Reflect>` containing the reflected component if it exists.
- **Usage**: Use this method to access the component's value in a reflected manner.

### `ReflectComponent::reflect_mut`
- **Description**: Gets the value of this `Component` type from the entity as a mutable reflected reference.
- **Parameters**:
  - `entity`: The entity to retrieve the component from.
- **Returns**: An `Option<Mut<dyn Reflect>>` containing a mutable reference to the reflected component if it exists.
- **Usage**: Use this method to modify the component's value in a reflected manner.

### `ReflectComponent::reflect_unchecked_mut`
- **Description**: Gets the value of this `Component` type from the entity as a mutable reference without safety checks.
- **Parameters**:
  - `entity`: An `UnsafeEntityCell` for the entity.
- **Usage**: This method should be used with caution, ensuring that it does not violate Rust's aliasing rules.

### `ReflectComponent::copy`
- **Description**: Copies the value of this `Component` type from one entity to another across different worlds.
- **Parameters**:
  - `source_world`: The world containing the source entity.
  - `destination_world`: The world containing the destination entity.
  - `source_entity`: The entity to copy from.
  - `destination_entity`: The entity to copy to.
  - `registry`: The type registry for type information.
- **Usage**: Use this method to duplicate component values between entities in different worlds.

### `ReflectComponent::register_component`
- **Description**: Registers the type of this `Component` in the `World`, returning its `ComponentId`.
- **Parameters**:
  - `world`: The world in which to register the component.
- **Usage**: Call this method to register a new component type in the ECS.

### `ReflectComponent::new`
- **Description**: Creates a custom implementation of `ReflectComponent`.
- **Usage**: This is an advanced feature useful for scripting implementations, allowing for dynamic component creation at runtime.

### `ReflectComponent::fn_pointers`
- **Description**: Returns the underlying function pointers implementing methods on `ReflectComponent`.
- **Usage**: Useful for accessing specific function pointers without repeatedly querying the type registry.

## Example Usage

### Inserting a Reflect Component
```rust
use bevy_ecs::prelude::*;
use bevy_reflect::TypeRegistry;

fn insert_component(entity: &mut EntityWorldMut, component: &dyn PartialReflect, registry: &TypeRegistry) {
    let reflect_component = ReflectComponent::new(ReflectComponentFns::new::<MyComponent>());
    reflect_component.insert(entity, component, registry);
}
```

### Applying a Reflect Component
```rust
fn apply_component(entity: EntityMut, component: &dyn PartialReflect) {
    let reflect_component = ReflectComponent::new(ReflectComponentFns::new::<MyComponent>());
    reflect_component.apply(entity, component);
}
```

### Removing a Reflect Component
```rust
fn remove_component(entity: &mut EntityWorldMut) {
    let reflect_component = ReflectComponent::new(ReflectComponentFns::new::<MyComponent>());
    reflect_component.remove(entity);
}
```

### Checking for a Reflect Component
```rust
fn has_component(entity: &FilteredEntityRef) -> bool {
    let reflect_component = ReflectComponent::new(ReflectComponentFns::new::<MyComponent>());
    reflect_component.contains(entity)
}
```

### Reflecting a Component
```rust
fn reflect_component(entity: &FilteredEntityRef) -> Option<&dyn Reflect> {
    let reflect_component = ReflectComponent::new(ReflectComponentFns::new::<MyComponent>());
    reflect_component.reflect(entity)
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `reflect/component` module of the `bevy_ecs` library to manage components in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.