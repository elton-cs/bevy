# Bevy ECS Error Module Documentation

This document provides a comprehensive overview of the public API available in the `world/error` module of the `bevy_ecs` library. It includes details on error types that can be used to handle various error scenarios in a Bevy application.

## Overview

- **Purpose**: This module defines error types that are returned by various operations in the Bevy ECS framework, particularly related to schedules and entity component retrieval.

## Error Types

### `TryRunScheduleError`
- **Description**: The error type returned by `World::try_run_schedule` if the provided schedule does not exist.
- **Fields**:
  - `InternedScheduleLabel`: The label of the schedule that was not found.
- **Usage**:
  - This error can be used to handle cases where a schedule is attempted to be run but does not exist in the world.
  
```rust
use crate::world::error::TryRunScheduleError;

fn run_schedule(world: &mut World, label: InternedScheduleLabel) {
    if let Err(e) = world.try_run_schedule(label) {
        match e {
            TryRunScheduleError(schedule_label) => {
                println!("Schedule not found: {:?}", schedule_label);
            }
        }
    }
}
```

### `EntityComponentError`
- **Description**: An error that occurs when dynamically retrieving components from an entity.
- **Variants**:
  - `MissingComponent(ComponentId)`: Indicates that the component with the given `ComponentId` does not exist on the entity.
  - `AliasedMutability(ComponentId)`: Indicates that the component with the given `ComponentId` was requested mutably more than once.
- **Usage**:
  - This error can be used to handle scenarios where an attempt is made to access a component that does not exist or to enforce unique mutable access to components.

```rust
use crate::world::error::EntityComponentError;

fn get_component(entity: Entity, component_id: ComponentId) -> Result<&Component, EntityComponentError> {
    // Attempt to retrieve the component
    if !entity.has_component(component_id) {
        return Err(EntityComponentError::MissingComponent(component_id));
    }
    // Logic to retrieve the component...
}
```

### `EntityFetchError`
- **Description**: An error that occurs when fetching entities mutably from a world.
- **Variants**:
  - `NoSuchEntity(Entity)`: Indicates that the entity with the given ID does not exist.
  - `AliasedMutability(Entity)`: Indicates that the entity with the given ID was requested mutably more than once.
- **Usage**:
  - This error can be used to handle cases where an attempt is made to fetch an entity that does not exist or to enforce unique mutable access to entities.

```rust
use crate::world::error::EntityFetchError;

fn fetch_entity(world: &World, entity_id: Entity) -> Result<&Entity, EntityFetchError> {
    if !world.contains_entity(entity_id) {
        return Err(EntityFetchError::NoSuchEntity(entity_id));
    }
    // Logic to fetch the entity...
}
```

## Example Usage

### Handling Errors in Schedule Execution
```rust
fn execute_schedule(world: &mut World, schedule_label: InternedScheduleLabel) {
    match world.try_run_schedule(schedule_label) {
        Ok(_) => println!("Schedule executed successfully."),
        Err(e) => match e {
            TryRunScheduleError(label) => {
                println!("Failed to execute schedule: {:?}", label);
            }
        },
    }
}
```

### Handling Component Retrieval Errors
```rust
fn access_component(entity: Entity, component_id: ComponentId) {
    match get_component(entity, component_id) {
        Ok(component) => {
            // Use the component
        }
        Err(EntityComponentError::MissingComponent(id)) => {
            println!("Component with ID {:?} is missing.", id);
        }
        Err(EntityComponentError::AliasedMutability(id)) => {
            println!("Component with ID {:?} is already borrowed mutably.", id);
        }
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `world/error` module of the `bevy_ecs` library to manage error handling in their applications or games. It provides insights into the available error types and their usage, enabling effective development with Bevy.