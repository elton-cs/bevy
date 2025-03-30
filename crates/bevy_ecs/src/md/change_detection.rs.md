# Bevy Change Detection Documentation

This document provides a comprehensive overview of the public API available in the `change_detection` module of the `bevy_ecs` library. It includes details on structs, traits, and their usage that can be utilized to detect changes in data within a Bevy application or game.

## Constants

### `CHECK_TICK_THRESHOLD`
- **Description**: The minimum number of world tick increments between `check_tick` scans.
- **Usage**: This constant defines how often change detection can occur, ensuring that changes are not missed due to timing issues.

### `MAX_CHANGE_AGE`
- **Description**: The maximum change tick difference that won't overflow before the next `check_tick` scan.
- **Usage**: This constant helps manage the age of changes to prevent overflow and false positives in change detection.

## Traits

### `DetectChanges`
- **Description**: A trait for types that can read change detection information.
- **Key Methods**:
  - `is_added(&self) -> bool`: Returns `true` if the value was added after the last system run.
  - `is_changed(&self) -> bool`: Returns `true` if the value was added or mutably dereferenced since the last system run.
  - `last_changed(&self) -> Tick`: Returns the change tick recording the last time this data was changed.
  - `changed_by(&self) -> &'static Location<'static>` (optional): Returns the location that last caused this to change (only available if the feature is enabled).

### `DetectChangesMut`
- **Description**: A trait for types that implement reliable change detection.
- **Key Methods**:
  - `set_changed(&mut self)`: Flags the value as having been changed.
  - `set_last_changed(&mut self, last_changed: Tick)`: Manually sets the change tick for when the data was last mutated.
  - `bypass_change_detection(&mut self) -> &mut Self::Inner`: Allows mutation without updating the change tick.
  - `set_if_neq(&mut self, value: Self::Inner) -> bool`: Overwrites the value if it differs from the current value, triggering change detection.
  - `replace_if_neq(&mut self, value: Self::Inner) -> Option<Self::Inner>`: Overwrites the value if it differs, returning the previous value.

## Structs

### `Ticks<'w>`
- **Description**: Stores tick information for change detection.
- **Fields**:
  - `added`: A reference to the tick when the value was added.
  - `changed`: A reference to the tick when the value was last changed.
  - `last_run`: The tick of the last system run.
  - `this_run`: The tick of the current run.

### `TicksMut<'w>`
- **Description**: Mutable version of `Ticks` for change detection.
- **Fields**:
  - `added`: A mutable reference to the tick when the value was added.
  - `changed`: A mutable reference to the tick when the value was last changed.
  - `last_run`: The tick of the last system run.
  - `this_run`: The tick of the current run.

### `Res<'w, T>`
- **Description**: Shared borrow of a resource with change detection.
- **Fields**:
  - `value`: A reference to the resource.
  - `ticks`: The tick information for change detection.
  - `changed_by`: The location that last caused this resource to change (if tracking is enabled).

### `ResMut<'w, T>`
- **Description**: Unique mutable borrow of a resource with change detection.
- **Fields**:
  - `value`: A mutable reference to the resource.
  - `ticks`: The mutable tick information for change detection.
  - `changed_by`: The location that last caused this resource to change (if tracking is enabled).

### `NonSendMut<'w, T>`
- **Description**: Unique mutable borrow of a non-`Send` resource.
- **Fields**:
  - `value`: A mutable reference to the resource.
  - `ticks`: The mutable tick information for change detection.
  - `changed_by`: The location that last caused this resource to change (if tracking is enabled).

### `Ref<'w, T>`
- **Description**: Shared borrow of an entity's component with access to change detection.
- **Fields**:
  - `value`: A reference to the component.
  - `ticks`: The tick information for change detection.
  - `changed_by`: The location that last caused this component to change (if tracking is enabled).

### `Mut<'w, T>`
- **Description**: Unique mutable borrow of an entity's component or resource with change detection.
- **Fields**:
  - `value`: A mutable reference to the component or resource.
  - `ticks`: The mutable tick information for change detection.
  - `changed_by`: The location that last caused this component or resource to change (if tracking is enabled).

### `MutUntyped<'w>`
- **Description**: Unique mutable borrow of resources or an entity's component without type safety.
- **Fields**:
  - `value`: A pointer to the value.
  - `ticks`: The mutable tick information for change detection.
  - `changed_by`: The location that last caused this resource to change (if tracking is enabled).

## Example Usage

### Using Change Detection in a System
```rust
use bevy_ecs::prelude::*;

#[derive(Resource)]
struct MyResource(u32);

fn my_system(mut resource: Res<MyResource>) {
    if resource.is_changed() {
        println!("My resource was mutated!");
    }
}
```

### Mutating a Resource with Change Detection
```rust
use bevy_ecs::prelude::*;

#[derive(Resource)]
struct MyResource(u32);

fn update_resource(mut resource: ResMut<MyResource>) {
    resource.set_if_neq(MyResource(42)); // Only triggers change detection if the value changes
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `change_detection` module of the `bevy_ecs` library to manage change detection in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.