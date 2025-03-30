# Bevy Label Documentation

This document provides a comprehensive overview of the public API available in the `label` module of the `bevy_ecs` library. It includes details on traits and their usage that can be utilized to define and manage labels in a Bevy application or game.

## Traits

### `DynEq`
- **Description**: An object-safe version of the `Eq` trait.
- **Key Methods**:
  - `as_any(&self) -> &dyn Any`: Casts the type to `dyn Any`.
  - `dyn_eq(&self, other: &dyn DynEq) -> bool`: Tests for equality between `self` and `other`.

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
- **Methods**:
  - `deref(&self) -> &Self::Target`: Dereferences the interned value.
  - `clone(&self) -> Self`: Clones the interned value.
  - `dyn_eq(&self, other: &Self) -> bool`: Compares two interned values for equality.
  - `dyn_hash(&self, state: &mut dyn Hasher)`: Hashes the interned value.

### `Interner<T>`
- **Description**: A thread-safe interner used to create `Interned<T>` from `&T`.
- **Fields**:
  - `0`: A `OnceLock` containing a `RwLock` for a set of interned values.
- **Methods**:
  - `new() -> Self`: Creates a new empty interner.
  - `intern(&self, value: &T) -> Interned<T>`: Returns the `Interned<T>` corresponding to `value`.

## Macros

### `define_label!`
- **Description**: A macro to define a new label trait.
- **Usage**:
  - Allows the creation of label traits with optional extra methods.
  - Automatically implements the necessary traits for the defined label.

## Example Usage

### Defining a Label Trait
```rust
use bevy_ecs::define_label;

define_label!(
    /// Documentation of label trait
    MyNewLabelTrait,
    MY_NEW_LABEL_TRAIT_INTERNER
);
```

### Using Interned Values
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

This documentation serves as a comprehensive guide for developers looking to utilize the `label` module of the `bevy_ecs` library to manage labels in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.