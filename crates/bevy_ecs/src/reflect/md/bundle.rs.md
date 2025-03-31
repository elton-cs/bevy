# Bevy ECS Reflect Bundle Module Documentation

This document provides a comprehensive overview of the public API available in the `reflect/bundle` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage, that can be utilized to manage bundles in a Bevy application or game.

## Overview

- **Purpose**: This module allows for the insertion, updating, and removal of bundles whose types are only known at runtime. It provides reflection capabilities for bundles, enabling dynamic manipulation of entity components.

## Structs

### `ReflectBundle`
- **Description**: A struct used to operate on the reflected `Bundle` trait of a type.
- **Key Points**:
  - Can be obtained via `bevy_reflect::TypeRegistration::data`.
  - Provides methods to insert, apply, and remove bundles from entities.
  
### `ReflectBundleFns`
- **Description**: Contains raw function pointers needed to implement a `ReflectBundle`.
- **Key Points**:
  - Includes function pointers for operations such as `insert`, `apply`, `apply_or_insert`, `remove`, and `take`.
  - Allows for the creation of custom implementations of `ReflectBundle`.

## Implementations

### `ReflectBundleFns::new`
- **Description**: Creates a default set of `ReflectBundleFns` for a specific bundle type using its `FromType` implementation.
- **Usage**: Useful for starting with default implementations before overriding specific functions for custom behavior.

### `ReflectBundle::insert`
- **Description**: Inserts a reflected `Bundle` into an entity.
- **Parameters**:
  - `entity`: The entity to which the bundle will be inserted.
  - `bundle`: The reflected bundle to insert.
  - `registry`: The type registry for type information.
- **Usage**: Call this method to add a bundle to an entity dynamically.

### `ReflectBundle::apply`
- **Description**: Sets the value of a `Bundle` type in the entity to the given value.
- **Panics**: Will panic if there is no `Bundle` of the given type.
- **Usage**: Use this method to update an existing bundle in an entity.

### `ReflectBundle::apply_or_insert`
- **Description**: Sets the value of a `Bundle` type in the entity to the given value or inserts a new one if it does not exist.
- **Usage**: This method is useful for ensuring that a bundle is present in an entity, either by updating it or inserting it if absent.

### `ReflectBundle::remove`
- **Description**: Removes a `Bundle` type from the entity.
- **Usage**: Call this method to remove a bundle from an entity. It does nothing if the bundle does not exist.

### `ReflectBundle::take`
- **Description**: Removes all components in the `Bundle` from the entity and returns their previous values.
- **Returns**: An `Option<Box<dyn Reflect>>` containing the previous values of the components.
- **Usage**: Use this method to safely remove a bundle and retrieve its previous state.

### `ReflectBundle::fn_pointers`
- **Description**: Returns the underlying function pointers implementing methods on `ReflectBundle`.
- **Usage**: Useful for accessing specific function pointers without repeatedly querying the type registry.

## Example Usage

### Inserting a Reflect Bundle
```rust
use bevy_ecs::prelude::*;
use bevy_reflect::TypeRegistry;

fn insert_bundle(entity: &mut EntityWorldMut, bundle: &dyn PartialReflect, registry: &TypeRegistry) {
    let reflect_bundle = ReflectBundle::new(ReflectBundleFns::new::<MyBundle>());
    reflect_bundle.insert(entity, bundle, registry);
}
```

### Applying a Reflect Bundle
```rust
fn apply_bundle(entity: EntityMut, bundle: &dyn PartialReflect, registry: &TypeRegistry) {
    let reflect_bundle = ReflectBundle::new(ReflectBundleFns::new::<MyBundle>());
    reflect_bundle.apply(entity, bundle, registry);
}
```

### Removing a Reflect Bundle
```rust
fn remove_bundle(entity: &mut EntityWorldMut) {
    let reflect_bundle = ReflectBundle::new(ReflectBundleFns::new::<MyBundle>());
    reflect_bundle.remove(entity);
}
```

### Taking a Reflect Bundle
```rust
fn take_bundle(entity: &mut EntityWorldMut) -> Option<Box<dyn Reflect>> {
    let reflect_bundle = ReflectBundle::new(ReflectBundleFns::new::<MyBundle>());
    reflect_bundle.take(entity)
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `reflect/bundle` module of the `bevy_ecs` library to manage bundles in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.
