# Bevy ECS Schedule Set Module Documentation

This document provides a comprehensive overview of the public API available in the `schedule/set` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to manage system sets in a Bevy application or game.

## Overview

- **Purpose**: This module provides types and functions that enable the definition and management of system sets, which are logical groups of systems that can be scheduled together in Bevy ECS.

## Types

### `InternedSystemSet`
- **Description**: A shorthand for `Interned<dyn SystemSet>`.
- **Key Points**:
  - Used to store and manage system sets efficiently.
  - Allows for type-erased access to system sets.

### `InternedScheduleLabel`
- **Description**: A shorthand for `Interned<dyn ScheduleLabel>`.
- **Key Points**:
  - Used to store and manage schedule labels efficiently.
  - Allows for type-erased access to schedule labels.

### `SystemTypeSet<T>`
- **Description**: A struct representing a system set grouping instances of the same function.
- **Key Points**:
  - Automatically populated and has special rules (e.g., cannot manually add members).
  - Provides methods to check if the set is a system type or anonymous.

### `AnonymousSet`
- **Description**: A struct representing a system set that is implicitly created when using `Schedule::add_systems` or `Schedule::configure_sets`.
- **Key Points**:
  - Automatically created for systems that do not belong to a named set.
  - Provides methods to check if the set is anonymous.

## Traits

### `IntoSystemSet<Marker>`
- **Description**: A trait for types that can be converted into a `SystemSet`.
- **Key Points**:
  - Implemented for various types, including systems and tuples thereof.
  - Provides methods for converting instances into their associated `SystemSet` type.

#### Methods

1. **`into_system_set(self) -> Self::Set`**
   - **Description**: Converts this instance to its associated `SystemSet` type.
   - **Usage**: Required for types implementing this trait.

## Example Usage

### Creating a System Set
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::schedule::SystemSet;

#[derive(SystemSet, Debug, Clone, PartialEq, Eq, Hash)]
struct MySystemSet;

fn my_system() {
    // System logic here
}

fn setup_systems() {
    let mut app = App::build();
    app.add_systems(my_system.in_set(MySystemSet));
}
```

### Using Interned System Sets
```rust
fn add_system_to_set(schedule: &mut Schedule) {
    let set = MySystemSet.intern();
    schedule.add_systems(set, my_system);
}
```

### Checking System Set Properties
```rust
fn check_system_set_properties(set: &dyn SystemSet) {
    if set.is_anonymous() {
        println!("This is an anonymous system set.");
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `schedule/set` module of the `bevy_ecs` library to manage system sets in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.