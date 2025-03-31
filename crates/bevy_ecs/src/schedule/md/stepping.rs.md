# Bevy ECS Schedule Stepping Module Documentation

This document provides a comprehensive overview of the public API available in the `schedule/stepping` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to control system stepping behavior in a Bevy application or game.

## Overview

- **Purpose**: This module provides functionality for managing the execution of systems in a controlled manner, allowing for stepping through systems frame by frame. This is particularly useful for debugging and testing purposes.

## Enums

### `Action`
- **Description**: Represents the current action of the stepping system.
- **Variants**:
  - `RunAll`: Stepping is disabled; run all systems.
  - `Waiting`: Stepping is enabled, but only required systems are run this frame.
  - `Continue`: Stepping is enabled; run all systems until the end of the frame or until a breakpoint is encountered.
  - `Step`: Stepping is enabled; only run the next system in the step list.
- **Key Points**:
  - Controls how systems are executed during the stepping process.

### `SystemBehavior`
- **Description**: Specifies the behavior of a system during stepping.
- **Variants**:
  - `AlwaysRun`: The system will always run regardless of stepping action.
  - `NeverRun`: The system will never run while stepping is enabled.
  - `Break`: The system will stop execution when stepping is enabled.
  - `Continue`: The system will run when stepping is enabled.

## Structs

### `Cursor`
- **Description**: Represents the current position in the stepping frame.
- **Fields**:
  - `schedule`: Index within the `Stepping::schedule_order`.
  - `system`: Index within the schedule's system list.
- **Usage**: Used to track the current system being executed during stepping.

### `Stepping`
- **Description**: A resource for controlling system stepping behavior.
- **Fields**:
  - `schedule_states`: A map of `ScheduleState` for each schedule with stepping enabled.
  - `schedule_order`: A dynamically generated order of schedules.
  - `cursor`: The current position in the stepping frame.
  - `previous_schedule`: Index of the last schedule to call `skipped_systems()`.
  - `action`: The action to perform during the current render frame.
  - `updates`: A list of updates to apply at the start of the next render frame.
- **Usage**: Used to manage and control the execution of systems based on stepping behavior.

## Implementations

### `Stepping::new`
- **Description**: Creates a new instance of the `Stepping` resource.
- **Usage**: Call this method to initialize a new `Stepping` resource.

### `Stepping::begin_frame`
- **Description**: System to call denoting that a new render frame has begun.
- **Parameters**:
  - `stepping`: An optional mutable reference to the `Stepping` resource.
- **Usage**: Call this method at the start of each frame to manage stepping behavior.

### `Stepping::schedules`
- **Description**: Returns the list of schedules with stepping enabled in the order they are executed.
- **Returns**: A result containing a reference to the list of schedules or an error if not ready.
- **Usage**: Use this method to retrieve the current order of schedules during stepping.

### `Stepping::cursor`
- **Description**: Returns the current position within the stepping frame.
- **Returns**: An optional tuple containing the current schedule label and node ID.
- **Usage**: Use this method to track the current system being executed during stepping.

### `Stepping::add_schedule`
- **Description**: Enables stepping for the provided schedule.
- **Parameters**:
  - `schedule`: The schedule to enable stepping for.
- **Usage**: Call this method to add a schedule to the stepping system.

### `Stepping::remove_schedule`
- **Description**: Disables stepping for the provided schedule.
- **Parameters**:
  - `schedule`: The schedule to disable stepping for.
- **Usage**: Call this method to remove a schedule from the stepping system.

### `Stepping::set_breakpoint`
- **Description**: Ensures that a system always runs when stepping is enabled.
- **Parameters**:
  - `schedule`: The schedule containing the system.
  - `system`: The system to set a breakpoint for.
- **Usage**: Call this method to mark a system to always run during stepping.

### `Stepping::clear_breakpoint`
- **Description**: Clears a breakpoint for the specified system.
- **Parameters**:
  - `schedule`: The schedule containing the system.
  - `system`: The system to clear the breakpoint for.
- **Usage**: Call this method to remove a breakpoint from a system.

### `Stepping::step_frame`
- **Description**: Runs the next system during the next render frame.
- **Usage**: Call this method to step through the systems one at a time.

### `Stepping::continue_frame`
- **Description**: Runs all remaining systems in the stepping frame during the next render frame.
- **Usage**: Call this method to continue executing systems until the end of the frame.

## Example Usage

### Managing Stepping Behavior
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::schedule::Stepping;

fn main() {
    let mut world = World::new();
    let mut stepping = Stepping::new();

    // Enable stepping for a schedule
    stepping.add_schedule(TestSchedule).enable();

    // Run the next frame
    stepping.step_frame();
}
```

### Setting Breakpoints
```rust
fn my_system() {
    // System logic here
}

fn setup_stepping(stepping: &mut Stepping) {
    stepping.add_schedule(TestSchedule).set_breakpoint(TestSchedule, my_system);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `schedule/stepping` module of the `bevy_ecs` library to manage system execution behavior in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.