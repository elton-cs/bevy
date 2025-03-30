# Bevy Identifier Module Documentation

This document provides a comprehensive overview of the public API available in the `identifier` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to manage identifiers in a Bevy application or game.

## Modules

### `error`
- **Description**: Contains error types for handling identifier conversions.
- **Key Types**:
  - `IdentifierError`: An enum representing various error conditions that can occur during identifier conversions.

### `kinds`
- **Description**: Defines the kinds of IDs that the `Identifier` can represent.
- **Key Types**:
  - `IdKind`: An enum that specifies different types of IDs, such as `Entity` and `Placeholder`.

### `masks`
- **Description**: Provides utility functions for working with identifier masks.
- **Key Types**:
  - `IdentifierMask`: A struct that contains methods for extracting and packing values from identifiers.

## Structs

### `Identifier`
- **Description**: A unified identifier for all entity and similar IDs.
- **Key Points**:
  - Has the same size as a `u64` integer, with a layout split between a 32-bit low segment and a 31-bit high segment, with the most significant bit reserved for type flags.
- **Fields**:
  - `low`: The low segment of the identifier.
  - `high`: The high segment of the identifier, stored as a `NonZero<u32>` to ensure it is never zero.
- **Methods**:
  - `fn new(low: u32, high: u32, kind: IdKind) -> Result<Self, IdentifierError>`: Constructs a new `Identifier`, returning an error if the high segment is invalid.
  - `fn low(self) -> u32`: Returns the value of the low segment.
  - `fn high(self) -> NonZero<u32>`: Returns the value of the high segment without masking.
  - `fn masked_high(self) -> u32`: Returns the masked value of the high segment, excluding flag bits.
  - `fn kind(self) -> IdKind`: Returns the kind of identifier from the high segment.
  - `fn to_bits(self) -> u64`: Converts the identifier into a `u64`.
  - `fn from_bits(value: u64) -> Self`: Converts a `u64` into an `Identifier`, panicking if the value is invalid.
  - `fn try_from_bits(value: u64) -> Result<Self, IdentifierError>`: Converts a `u64` into an `Identifier`, returning an error if the value is invalid.

## Implementations

### `PartialEq`, `Eq`, `PartialOrd`, `Ord`, and `Hash` Implementations
- **Description**: Implements comparison and hashing traits for the `Identifier` struct.
- **Key Points**:
  - These implementations allow identifiers to be compared and used in collections that require hashing, such as `HashMap`.

## Example Usage

### Creating an Identifier
```rust
use bevy_ecs::identifier::{Identifier, IdKind};

fn create_identifier() -> Identifier {
    Identifier::new(12, 55, IdKind::Entity).unwrap()
}
```

### Converting to and from Bits
```rust
let id = Identifier::new(12, 55, IdKind::Entity).unwrap();
let bits = id.to_bits();
let new_id = Identifier::from_bits(bits);
```

### Extracting Components
```rust
let low = id.low();
let high = id.high();
let kind = id.kind();
```

This documentation serves as a comprehensive guide for developers looking to utilize the `identifier` module of the `bevy_ecs` library to manage identifiers in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.