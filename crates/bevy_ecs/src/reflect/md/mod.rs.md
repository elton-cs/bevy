# Bevy ECS Reflect Module Documentation

This document provides a comprehensive overview of the public API available in the `reflect` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to enable reflection support in a Bevy application or game.

## Overview

- **Purpose**: This module provides types and functions that enable reflection support for components, resources, and bundles in Bevy ECS. It allows for dynamic manipulation of types whose structure is only known at runtime.

## Structs

### `AppTypeRegistry`
- **Description**: A resource that stores a `TypeRegistry` for type registrations relevant to the entire application.
- **Key Points**:
  - Implements `Deref` and `DerefMut` traits to provide easy access to the underlying `TypeRegistry`.
  - Used to register and retrieve types for reflection.

### `AppFunctionRegistry` (Conditional)
- **Description**: A resource that stores a `FunctionRegistry` for function registrations relevant to the entire application.
- **Key Points**:
  - Only available if the `reflect_functions` feature is enabled.
  - Implements `Deref` and `DerefMut` traits for easy access to the underlying `FunctionRegistry`.

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

### `ReflectFromWorld`
- **Description**: A struct used to operate on the reflected `FromWorld` trait of a type.
- **Key Points**:
  - Can be obtained via `bevy_reflect::TypeRegistration::data`.
  - Provides methods to create instances of types that require a `&mut World` for initialization.

### `ReflectFromWorldFns`
- **Description**: Contains raw function pointers needed to implement a `ReflectFromWorld`.
- **Key Points**:
  - Includes a function pointer for the `from_world` method, which creates an instance of the type from a mutable reference to `World`.

### `ReflectBundle`
- **Description**: A struct used to operate on the reflected `Bundle` trait of a type.
- **Key Points**:
  - Can be obtained via `bevy_reflect::TypeRegistration::data`.
  - Provides methods to insert, apply, and remove bundles from entities.

### `ReflectComponent`
- **Description**: A struct used to operate on the reflected `Component` trait of a type.
- **Key Points**:
  - Can be obtained via `bevy_reflect::TypeRegistration::data`.
  - Provides methods to insert, apply, and remove components from entities.

### `ReflectVisitEntities`
- **Description**: A struct that facilitates applying an operation to all contained entities in a reflected value.
- **Key Points**:
  - Contains a function pointer for visiting entities in reflected components.

### `ReflectVisitEntitiesMut`
- **Description**: A struct that facilitates applying operations to mutable references of all contained entities in a reflected value.
- **Key Points**:
  - Contains a function pointer for visiting entities in reflected components with mutable access.

### `ReflectMapEntities`
- **Description**: A struct that facilitates the mapping of entities in a reflected value using an `EntityMapper`.
- **Key Points**:
  - Contains a function pointer for mapping entities in reflected components.

## Functions

### `from_reflect_with_fallback`
- **Description**: Creates an instance of type `T` from a `&dyn PartialReflect`.
- **Key Points**:
  - Tries to use the reflected `FromReflect`, `Default`, or `FromWorld` traits in that order.
  - Panics if none of the strategies succeed or if the reflected type does not match.

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

### Creating an Instance from Reflect
```rust
fn create_instance(world: &mut World) -> MyResource {
    let reflect_from_world = ReflectFromWorld::new(ReflectFromWorldFns::new::<MyResource>());
    reflect_from_world.from_world(world)
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `reflect` module of the `bevy_ecs` library to manage reflection capabilities in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.