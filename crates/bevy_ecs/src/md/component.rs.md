# Bevy Component Documentation

This document provides a comprehensive overview of the public API available in the `component` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to declare and manage components in a Bevy application or game.

## Constants

### `CHECK_TICK_THRESHOLD`
- **Description**: The minimum number of world tick increments between `check_tick` scans.
- **Usage**: This constant defines how often change detection can occur, ensuring that changes are not missed due to timing issues.

### `MAX_CHANGE_AGE`
- **Description**: The maximum change tick difference that won't overflow before the next `check_tick` scan.
- **Usage**: This constant helps manage the age of changes to prevent overflow and false positives in change detection.

## Traits

### `Component`
- **Description**: A trait for types that can be used to store data for an entity.
- **Key Points**:
  - Components are typically structs but can also be enums or zero-sized types.
  - Components must implement `Send + Sync + 'static`.
  - Components can specify required components that will be automatically initialized when the component is added to an entity.
  - Components can choose their storage type (e.g., `Table` or `SparseSet`) using the `#[component(storage = "...")]` attribute.

### `DetectChanges`
- **Description**: A trait for types that can read change detection information.
- **Key Methods**:
  - `is_added(&self) -> bool`: Returns `true` if the value was added after the last system run.
  - `is_changed(&self) -> bool`: Returns `true` if the value was added or mutably dereferenced since the last system run.
  - `last_changed(&self) -> Tick`: Returns the change tick recording the last time this data was changed.

### `DetectChangesMut`
- **Description**: A trait for types that implement reliable change detection.
- **Key Methods**:
  - `set_changed(&mut self)`: Flags the value as having been changed.
  - `set_last_changed(&mut self, last_changed: Tick)`: Manually sets the change tick for when the data was last mutated.
  - `bypass_change_detection(&mut self) -> &mut Self::Inner`: Allows mutation without updating the change tick.

## Structs

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

### `ComponentHooks`
- **Description**: Stores lifecycle hooks for components.
- **Fields**:
  - `on_add`: Hook for when a component is added.
  - `on_insert`: Hook for when a component is inserted.
  - `on_replace`: Hook for when a component is replaced.
  - `on_remove`: Hook for when a component is removed.

### `ComponentTicks`
- **Description**: Records when a component or resource was added and when it was last mutably dereferenced.
- **Fields**:
  - `added`: Tick recording the time this component was added.
  - `changed`: Tick recording the time this component was last changed.
- **Methods**:
  - `is_added(&self, last_run: Tick, this_run: Tick) -> bool`: Checks if the component was added after the last system run.
  - `is_changed(&self, last_run: Tick, this_run: Tick) -> bool`: Checks if the component was changed after the last system run.

## Example Usage

### Defining a Component
```rust
use bevy_ecs::component::Component;

#[derive(Component)]
struct Position(f32, f32);

#[derive(Component)]
struct Velocity(f32, f32);
```

### Using Required Components
```rust
use bevy_ecs::prelude::*;

#[derive(Component)]
#[require(B)]
struct A;

#[derive(Component, Default)]
struct B(usize);

fn setup(mut commands: Commands) {
    commands.spawn(A); // Automatically inserts B with its Default constructor
}
```

### Accessing Component Metadata
```rust
use bevy_ecs::prelude::*;

fn print_component_info(components: &Components) {
    if let Some(info) = components.get_info(component_id::<MyComponent>()) {
        println!("Component Name: {}", info.name());
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `component` module of the `bevy_ecs` library to manage components in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.