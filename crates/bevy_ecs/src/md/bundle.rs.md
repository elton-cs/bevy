# Bevy Bundle Documentation

This document provides a comprehensive overview of the public API available in the `bundle` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to manage bundles of components in a Bevy application or game.

## Traits

### `Bundle`
- **Description**: The `Bundle` trait enables insertion and removal of components from an entity.
- **Key Points**:
  - Implementors of the `Bundle` trait are called 'bundles'.
  - Each bundle represents a static set of component types.
  - Adding a bundle to an entity will add the components it represents, overwriting existing values if necessary.
  - Bundles should not be treated as units of behavior; they are collections of components.
  - Manual implementations of this trait are unsupported; use `#[derive(Bundle)]` instead.

### `DynamicBundle`
- **Description**: The `DynamicBundle` trait provides methods for bundles that do not require statically knowing the components.
- **Key Points**:
  - It allows calling a function on each value in the bundle, passing ownership of the component values.
  - This trait is useful for bundles that may change at runtime.

## Structs

### `BundleId`
- **Description**: Stores a unique value identifying a type of registered bundle for a specific world.
- **Fields**:
  - `0`: The index of the associated bundle type.
- **Methods**:
  - `index() -> usize`: Returns the index of the associated bundle type.

### `BundleInfo`
- **Description**: Stores metadata associated with a specific type of bundle for a given world.
- **Fields**:
  - `id`: The unique identifier for the bundle.
  - `component_ids`: A list of all components contributed by the bundle, including required components.
  - `required_components`: A list of required components needed by this bundle.
  - `explicit_components_len`: The length of explicitly defined components in the bundle.
- **Methods**:
  - `explicit_components() -> &[ComponentId]`: Returns the IDs of explicitly defined components.
  - `required_components() -> &[ComponentId]`: Returns the IDs of required components.
  - `contributed_components() -> &[ComponentId]`: Returns the IDs of all components contributed by this bundle.
  - `iter_explicit_components()`: Returns an iterator over the IDs of explicitly defined components.
  - `iter_contributed_components()`: Returns an iterator over the IDs of all components contributed by this bundle.
  - `write_components(...)`: Writes components from a given bundle to the specified entity.

### `Bundles`
- **Description**: Metadata for bundles, storing a `BundleInfo` for each type of bundle in a given world.
- **Methods**:
  - `get(bundle_id: BundleId) -> Option<&BundleInfo>`: Gets the metadata associated with a specific type of bundle.
  - `get_id(type_id: TypeId) -> Option<BundleId>`: Gets the value identifying a specific type of bundle.
  - `register_info<T: Bundle>(...)`: Registers a new `BundleInfo` for a statically known type.
  - `register_contributed_bundle_info<T: Bundle>(...)`: Registers a new `BundleInfo` for a statically known type, including both explicit and required components.

## Example Usage

### Defining a Bundle
```rust
use bevy_ecs::{component::Component, bundle::Bundle};

#[derive(Component)]
struct Position(f32, f32);

#[derive(Component)]
struct Velocity(f32, f32);

#[derive(Bundle)]
struct PhysicsBundle {
    position: Position,
    velocity: Velocity,
}
```

### Using Bundles in a Bevy Application
```rust
use bevy_ecs::prelude::*;

fn main() {
    let mut world = World::new();
    world.spawn(PhysicsBundle {
        position: Position(0.0, 0.0),
        velocity: Velocity(1.0, 1.0),
    });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `bundle` module of the `bevy_ecs` library to manage component bundles in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.