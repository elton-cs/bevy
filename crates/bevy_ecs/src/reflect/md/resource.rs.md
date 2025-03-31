# Bevy ECS Reflect Resource Module Documentation

This document provides a comprehensive overview of the public API available in the `reflect/resource` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to manage resources in a Bevy application or game.

## Overview

- **Purpose**: This module allows for the insertion, updating, removal, and general interaction with resources whose types are only known at runtime. It provides reflection capabilities for resources, enabling dynamic manipulation of resource data.

## Structs

### `ReflectResource`
- **Description**: A struct used to operate on the reflected `Resource` trait of a type.
- **Key Points**:
  - Can be obtained via `bevy_reflect::TypeRegistration::data`.
  - Wraps a `ReflectResourceFns` struct, providing methods to interact with resources dynamically.

### `ReflectResourceFns`
- **Description**: Contains raw function pointers needed to implement a `ReflectResource`.
- **Key Points**:
  - Includes function pointers for operations such as `insert`, `apply`, `apply_or_insert`, `remove`, `reflect`, `reflect_unchecked_mut`, `copy`, and `register_resource`.
  - Allows for the creation of custom implementations of `ReflectResource`.

## Implementations

### `ReflectResourceFns::new`
- **Description**: Creates a default set of `ReflectResourceFns` for a specific resource type using its `FromType` implementation.
- **Usage**: Useful for starting with default implementations before overriding specific functions for custom behavior.

### `ReflectResource::insert`
- **Description**: Inserts a reflected `Resource` into the world.
- **Parameters**:
  - `world`: A mutable reference to the `World` where the resource will be inserted.
  - `resource`: The reflected resource to insert.
  - `registry`: The type registry for type information.
- **Usage**: Call this method to add a resource to the world dynamically.

### `ReflectResource::apply`
- **Description**: Sets the value of a `Resource` type in the world to the given value.
- **Panics**: Will panic if there is no `Resource` of the given type.
- **Usage**: Use this method to update an existing resource in the world.

### `ReflectResource::apply_or_insert`
- **Description**: Sets the value of a `Resource` type in the world to the given value or inserts a new one if it does not exist.
- **Usage**: This method is useful for ensuring that a resource is present in the world, either by updating it or inserting it if absent.

### `ReflectResource::remove`
- **Description**: Removes a `Resource` type from the world.
- **Usage**: Call this method to remove a resource from the world. It does nothing if the resource does not exist.

### `ReflectResource::reflect`
- **Description**: Gets the value of this `Resource` type from the world as a reflected reference.
- **Parameters**:
  - `world`: The world to retrieve the resource from.
- **Returns**: An `Option<&dyn Reflect>` containing the reflected resource if it exists.
- **Usage**: Use this method to access the resource's value in a reflected manner.

### `ReflectResource::reflect_mut`
- **Description**: Gets the value of this `Resource` type from the world as a mutable reflected reference.
- **Parameters**:
  - `world`: A mutable reference to the world to retrieve the resource from.
- **Returns**: An `Option<Mut<'a, dyn Reflect>>` containing a mutable reference to the reflected resource if it exists.
- **Usage**: Use this method to modify the resource's value in a reflected manner.

### `ReflectResource::reflect_unchecked_mut`
- **Description**: Gets the value of this `Resource` type from the world as a mutable reference without safety checks.
- **Parameters**:
  - `world`: An `UnsafeWorldCell` for the world.
- **Usage**: This method should be used with caution, ensuring that it does not violate Rust's aliasing rules.

### `ReflectResource::copy`
- **Description**: Copies the value of this `Resource` type from one world to another.
- **Parameters**:
  - `source_world`: The world containing the source resource.
  - `destination_world`: The world to which the resource will be copied.
  - `registry`: The type registry for type information.
- **Usage**: Use this method to duplicate resource values between worlds.

### `ReflectResource::register_resource`
- **Description**: Registers the type of this `Resource` in the `World`, returning its `ComponentId`.
- **Parameters**:
  - `world`: The world in which to register the resource.
- **Usage**: Call this method to register a new resource type in the ECS.

### `ReflectResource::new`
- **Description**: Creates a custom implementation of `ReflectResource`.
- **Usage**: This is an advanced feature useful for scripting implementations, allowing for dynamic resource creation at runtime.

### `ReflectResource::fn_pointers`
- **Description**: Returns the underlying function pointers implementing methods on `ReflectResource`.
- **Usage**: Useful for accessing specific function pointers without repeatedly querying the type registry.

## Example Usage

### Inserting a Reflect Resource
```rust
use bevy_ecs::prelude::*;
use bevy_reflect::{Reflect, FromReflect};

#[derive(Resource, Reflect)]
struct MyResource {
    value: u32,
}

fn insert_resource(world: &mut World, resource: &dyn PartialReflect, registry: &TypeRegistry) {
    let reflect_resource = ReflectResource::new(ReflectResourceFns::new::<MyResource>());
    reflect_resource.insert(world, resource, registry);
}
```

### Applying a Reflect Resource
```rust
fn apply_resource(world: &mut World, resource: &dyn PartialReflect) {
    let reflect_resource = ReflectResource::new(ReflectResourceFns::new::<MyResource>());
    reflect_resource.apply(world, resource);
}
```

### Removing a Reflect Resource
```rust
fn remove_resource(world: &mut World) {
    let reflect_resource = ReflectResource::new(ReflectResourceFns::new::<MyResource>());
    reflect_resource.remove(world);
}
```

### Reflecting a Resource
```rust
fn reflect_resource(world: &World) -> Option<&dyn Reflect> {
    let reflect_resource = ReflectResource::new(ReflectResourceFns::new::<MyResource>());
    reflect_resource.reflect(world)
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `reflect/resource` module of the `bevy_ecs` library to manage resources in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.