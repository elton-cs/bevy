# Bevy ECS Schedule Condition Module Documentation

This document provides a comprehensive overview of the public API available in the `schedule/condition` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to manage run conditions for systems in a Bevy application or game.

## Overview

- **Purpose**: This module provides types and functions that enable the definition of conditions under which systems should run. It allows for flexible and dynamic control over system execution based on various criteria.

## Types

### `BoxedCondition<In>`
- **Description**: A type-erased run condition stored in a `Box`.
- **Key Points**:
  - Represents a condition that can be evaluated to determine if a system should run.
  - Can be used with functions and closures that convert into `System<Out=bool>`.

### `ReflectCommandExt`
- **Description**: An extension trait for `EntityCommands` that adds reflection-related functions.
- **Key Points**:
  - Provides methods to insert and remove components or bundles using reflection.
  - Allows for dynamic manipulation of entity components without knowing their types at compile time.

## Traits

### `Condition<Marker, In>`
- **Description**: A trait for defining run conditions for systems.
- **Key Points**:
  - Implemented for functions and closures that convert into `System<Out=bool>`.
  - Provides methods for combining conditions using logical operators (AND, OR, NOT, etc.).

#### Methods

1. **`and<M, C: Condition<M, In>>(self, and: C)`**
   - **Description**: Combines this condition with another condition using logical AND.
   - **Usage**: Returns a new condition that is true only if both conditions are true.

2. **`and_then<M, C: Condition<M, In>>(self, and_then: C)`**
   - **Description**: Combines this condition with another condition using logical AND, but is deprecated in favor of `and`.
   - **Usage**: Returns a new condition that is true only if both conditions are true.

3. **`nand<M, C: Condition<M, In>>(self, nand: C)`**
   - **Description**: Combines this condition with another condition using logical NAND.
   - **Usage**: Returns a new condition that is true if at least one of the conditions is false.

4. **`nor<M, C: Condition<M, In>>(self, nor: C)`**
   - **Description**: Combines this condition with another condition using logical NOR.
   - **Usage**: Returns a new condition that is true only if both conditions are false.

5. **`or<M, C: Condition<M, In>>(self, or: C)`**
   - **Description**: Combines this condition with another condition using logical OR.
   - **Usage**: Returns a new condition that is true if at least one of the conditions is true.

6. **`xnor<M, C: Condition<M, In>>(self, xnor: C)`**
   - **Description**: Combines this condition with another condition using logical XNOR.
   - **Usage**: Returns a new condition that is true if both conditions are either true or false.

7. **`xor<M, C: Condition<M, In>>(self, xor: C)`**
   - **Description**: Combines this condition with another condition using logical XOR.
   - **Usage**: Returns a new condition that is true if exactly one of the conditions is true.

8. **`not<Marker, TOut, T>(condition: T)`**
   - **Description**: Generates a condition that inverses the result of the passed condition.
   - **Usage**: Returns a new condition that is true if the original condition is false.

9. **`condition_changed<Marker, CIn, C>(condition: C)`**
   - **Description**: Generates a condition that returns true when the passed condition changes.
   - **Usage**: Useful for detecting changes in conditions over time.

10. **`condition_changed_to<Marker, CIn, C>(to: bool, condition: C)`**
    - **Description**: Generates a condition that returns true when the passed condition changes to a specified value.
    - **Usage**: Useful for detecting transitions in conditions.

## Example Usage

### Defining a Run Condition
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::schedule::Condition;

fn my_condition() -> impl Condition<()> {
    // Define a condition that checks if a resource exists
    resource_exists::<MyResource>
}

fn my_system() {
    // System logic here
}
```

### Combining Conditions
```rust
fn combined_condition() -> impl Condition<()> {
    // Combine two conditions using AND
    my_condition().and(another_condition())
}
```

### Using Conditions in Systems
```rust
fn setup_system(commands: &mut Commands) {
    commands.spawn().insert_resource(MyResource);
}

fn run_if_condition() {
    let mut app = App::build();
    app.add_system(my_system.run_if(my_condition()));
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `schedule/condition` module of the `bevy_ecs` library to manage system execution conditions in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.