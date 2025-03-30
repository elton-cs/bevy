# Bevy Intern Documentation

This document provides a comprehensive overview of the public API available in the `intern` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to intern immutable values in a Bevy application or game.

## Constants

### `CHECK_TICK_THRESHOLD`
- **Description**: The minimum number of world tick increments between `check_tick` scans.
- **Usage**: This constant defines how often change detection can occur, ensuring that changes are not missed due to timing issues.

### `MAX_CHANGE_AGE`
- **Description**: The maximum change tick difference that won't overflow before the next `check_tick` scan.
- **Usage**: This constant helps manage the age of changes to prevent overflow and false positives in change detection.

## Traits

### `Internable`
- **Description**: A trait for values that can be interned.
- **Key Methods**:
  - `leak(&self) -> &'static Self`: Creates a static reference to `self`, possibly leaking memory.
  - `ref_eq(&self, other: &Self) -> bool`: Returns `true` if the two references point to the same value.
  - `ref_hash<H: core::hash::Hasher>(&self, state: &mut H)`: Feeds the reference to the hasher.

## Structs

### `Interned<T>`
- **Description**: An interned value that remains valid until the end of the program and will not drop.
- **Fields**:
  - `0`: A static reference to the interned value.
- **Methods**:
  - `deref(&self) -> &Self::Target`: Dereferences the interned value.
  - `clone(&self) -> Self`: Clones the interned value.
  - `eq(&self, other: &Self) -> bool`: Compares two interned values for equality.
  - `hash<H: core::hash::Hasher>(&self, state: &mut H)`: Hashes the interned value.

### `Interner<T>`
- **Description**: A thread-safe interner used to create `Interned<T>` from `&T`.
- **Fields**:
  - `0`: A `OnceLock` containing a `RwLock` for a set of interned values.
- **Methods**:
  - `new() -> Self`: Creates a new empty interner.
  - `intern(&self, value: &T) -> Interned<T>`: Returns the `Interned<T>` corresponding to `value`.

### `Ticks<'a>`
- **Description**: Stores tick information for change detection.
- **Fields**:
  - `added`: A reference to the tick when the value was added.
  - `changed`: A reference to the tick when the value was last changed.

### `TicksMut<'a>`
- **Description**: Mutable version of `Ticks` for change detection.
- **Fields**:
  - `added`: A mutable reference to the tick when the value was added.
  - `changed`: A mutable reference to the tick when the value was last changed.

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
  - `drop`: A function for cleaning up values of the underlying component type.

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
  - `component_id<T: Component>(&self) -> Option<ComponentId>`: Returns the `ComponentId` of the given component type.

## Example Usage

### Interning Values
```rust
use bevy_ecs::intern::{Interner, Interned};

#[derive(PartialEq, Eq, Hash, Debug)]
struct Value(i32);

impl Internable for Value {
    fn leak(&self) -> &'static Self {
        Box::leak(Box::new(Value(self.0)))
    }

    fn ref_eq(&self, other: &Self) -> bool {
        std::ptr::eq(self, other)
    }

    fn ref_hash<H: std::hash::Hasher>(&self, state: &mut H) {
        std::ptr::hash(self, state);
    }
}

fn main() {
    let interner = Interner::new();
    let x = interner.intern(&Value(42));
    let y = interner.intern(&Value(42));
    assert_eq!(x, y); // Same interned value
}
```

### Using Interned Strings
```rust
use bevy_ecs::intern::{Interner, Interned};

fn main() {
    let interner = Interner::new();
    let a = interner.intern("Hello");
    let b = interner.intern("Hello");
    assert_eq!(a, b); // Same interned string
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `intern` module of the `bevy_ecs` library to manage interned values in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.