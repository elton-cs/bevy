# Bevy Identifier Masks Documentation

This document provides a comprehensive overview of the public API available in the `identifier/masks` module of the `bevy_ecs` library. It includes details on constants and structs that can be utilized to manage identifier masks in a Bevy application or game.

## Constants

### `HIGH_MASK`
- **Description**: A constant mask for extracting the value portion of a 32-bit high segment.
- **Value**: `0x7FFF_FFFF`
- **Key Points**:
  - This mask yields 31 bits of total value, as the final bit (the most significant) is reserved as a flag bit.
  - It can be negated to extract the flag bit, allowing for flexible manipulation of identifier values.

## Structs

### `IdentifierMask`
- **Description**: An abstraction over masks needed to extract values/components of an `Identifier`.
- **Key Points**:
  - Provides utility functions for working with identifiers, including extracting and packing values.
  
#### Methods
- **`get_low(value: u64) -> u32`**:
  - **Description**: Returns the low component from a `u64` value.
  - **Usage**: This method truncates the value to the lowest 32 bits.

- **`get_high(value: u64) -> u32`**:
  - **Description**: Returns the high component from a `u64` value.
  - **Usage**: This method discards the lowest 32 bits.

- **`pack_into_u64(low: u32, high: u32) -> u64`**:
  - **Description**: Packs a low and high `u32` value into a single `u64` value.
  - **Usage**: This method combines the two components into a single identifier.

- **`pack_kind_into_high(value: u32, kind: IdKind) -> u32`**:
  - **Description**: Packs the `IdKind` bits into a high segment.
  - **Usage**: This method allows for the inclusion of identifier kind information into the high segment of an ID.

- **`extract_value_from_high(value: u32) -> u32`**:
  - **Description**: Extracts the value component from a high segment of an `Identifier`.
  - **Usage**: This method retrieves the actual value while ignoring the flag bit.

- **`extract_kind_from_high(value: u32) -> IdKind`**:
  - **Description**: Extracts the ID kind component from a high segment of an `Identifier`.
  - **Usage**: This method determines whether the ID is an `Entity` or a `Placeholder`.

- **`inc_masked_high_by(lhs: NonZero<u32>, rhs: u32) -> NonZero<u32>`**:
  - **Description**: Offsets a masked generation value by the specified amount, wrapping to 1 instead of 0.
  - **Usage**: This method ensures that the incremented value remains within valid bounds and does not exceed the maximum allowed.

## Example Usage

### Extracting Low and High Components
```rust
let value: u64 = 0x7FFF_FFFF_0000_000C;
let low = IdentifierMask::get_low(value);
let high = IdentifierMask::get_high(value);
```

### Packing and Unpacking Values
```rust
let packed = IdentifierMask::pack_into_u64(low, high);
let extracted_low = IdentifierMask::get_low(packed);
let extracted_high = IdentifierMask::get_high(packed);
```

### Using Identifier Kinds
```rust
let kind = IdentifierMask::extract_kind_from_high(high);
match kind {
    IdKind::Entity => println!("This is an entity ID."),
    IdKind::Placeholder => println!("This is a placeholder ID."),
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `identifier/masks` module of the `bevy_ecs` library to manage identifier masks in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.