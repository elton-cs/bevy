# Bevy ECS Schedule Config Module Documentation

This document provides a comprehensive overview of the public API available in the `schedule/config` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to configure systems and their execution in a Bevy application or game.

## Overview

- **Purpose**: This module provides types and functions that enable the configuration of systems and their scheduling within the Bevy ECS framework. It allows for the definition of conditions under which systems should run and manages dependencies between systems.

## Types

### `BoxedCondition<In>`
- **Description**: A type-erased run condition stored in a `Box`.
- **Key Points**:
  - Represents a condition that can be evaluated to determine if a system should run.
  - Can be used with functions and closures that convert into `System<Out=bool>`.

### `NodeConfig<T>`
- **Description**: Stores configuration for a single generic node (a system or a system set).
- **Key Points**:
  - Includes the node itself, scheduling metadata (hierarchy and dependencies), and run conditions associated with the node.

### `SystemConfig`
- **Description**: Stores configuration for a single system.
- **Key Points**:
  - A type alias for `NodeConfig<BoxedSystem>`.

### `NodeConfigs<T>`
- **Description**: A collection of generic `NodeConfig`s.
- **Key Points**:
  - Can represent either a single node configuration or a tuple of nested configurations.

### `SystemSetConfig`
- **Description**: A `SystemSet` with scheduling metadata.
- **Key Points**:
  - Used to manage sets of systems and their execution order.

### `SystemSetConfigs`
- **Description**: A collection of `SystemSetConfig`.
- **Key Points**:
  - Allows for the management of multiple system sets and their configurations.

## Traits

### `IntoSystemConfigs<Marker>`
- **Description**: A trait for types that can convert into a `SystemConfigs`.
- **Key Points**:
  - Implemented for systems and tuples thereof.
  - Provides methods for adding systems to sets, running conditions, and managing dependencies.

#### Methods

1. **`into_configs(self) -> SystemConfigs`**
   - **Description**: Convert into a `SystemConfigs`.
   - **Usage**: Required for types implementing this trait.

2. **`in_set(self, set: impl SystemSet) -> SystemConfigs`**
   - **Description**: Add these systems to the provided set.
   - **Usage**: Useful for organizing systems into specific sets for execution.

3. **`before<M>(self, set: impl IntoSystemSet<M>) -> SystemConfigs`**
   - **Description**: Run before all systems in the specified set.
   - **Usage**: Useful for managing execution order.

4. **`after<M>(self, set: impl IntoSystemSet<M>) -> SystemConfigs`**
   - **Description**: Run after all systems in the specified set.
   - **Usage**: Useful for managing execution order.

5. **`run_if<M>(self, condition: impl Condition<M>) -> SystemConfigs`**
   - **Description**: Run the systems only if the specified condition is true.
   - **Usage**: Useful for conditional execution of systems.

6. **`ambiguous_with<M>(self, set: impl IntoSystemSet<M>) -> SystemConfigs`**
   - **Description**: Suppress warnings and errors for ambiguities with systems in the specified set.
   - **Usage**: Useful for managing conflicting access.

7. **`chain(self) -> SystemSetConfigs`**
   - **Description**: Treat this collection as a sequence of systems.
   - **Usage**: Useful for applying ordering constraints between systems.

## Example Usage

### Creating a System Configuration
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::schedule::IntoSystemConfigs;

fn my_system() {
    // System logic here
}

fn setup_systems() {
    let mut app = App::build();
    app.add_systems((my_system,).in_set(SystemSet::new()));
}
```

### Using Conditions with Systems
```rust
fn conditional_system() {
    // System logic here
}

fn setup_conditional_systems() {
    let mut app = App::build();
    app.add_systems(conditional_system.run_if(some_condition()));
}
```

### Chaining System Configurations
```rust
fn another_system() {
    // System logic here
}

fn setup_chained_systems() {
    let mut app = App::build();
    app.add_systems((my_system, another_system).chain());
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `schedule/config` module of the `bevy_ecs` library to manage system configurations in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.