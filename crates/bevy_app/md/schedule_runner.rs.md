# Bevy Schedule Runner Documentation

This document provides a comprehensive overview of the public API available in the `schedule_runner` module of the `bevy_app` library. It includes details on structs, enums, and functions that can be utilized to manage the execution of an application's schedule in a Bevy application or game.

## Enums

### `RunMode`
- **Description**: Determines the method used to run an `App`'s `Schedule`.
- **Variants**:
  - `Loop { wait: Option<Duration> }`: Indicates that the schedule should run repeatedly, with an optional wait duration after each completion.
  - `Once`: Indicates that the schedule should run only once.
- **Usage**: Use this enum to configure how the app's schedule is executed, either in a loop or just once.

## Structs

### `ScheduleRunnerPlugin`
- **Description**: Configures an `App` to run its `Schedule` according to a given `RunMode`.
- **Fields**:
  - `run_mode`: Determines whether the schedule is run once or repeatedly.
- **Methods**:
  - `run_once() -> Self`: Creates a `ScheduleRunnerPlugin` that runs the schedule once.
  - `run_loop(wait_duration: Duration) -> Self`: Creates a `ScheduleRunnerPlugin` that runs the schedule in a loop with a specified wait duration.
- **Usage**: Add this plugin to your app to control how the schedule is executed.

## Example Usage

### Basic Schedule Runner Plugin Implementation
```rust
use bevy_app::{App, ScheduleRunnerPlugin};

fn main() {
    App::new()
        .add_plugins(ScheduleRunnerPlugin::run_loop(Duration::from_millis(16))) // Run at ~60 FPS
        .run();
}
```

### Running the Schedule Once
```rust
use bevy_app::{App, ScheduleRunnerPlugin};

fn main() {
    App::new()
        .add_plugins(ScheduleRunnerPlugin::run_once())
        .run();
}
```

## Lifecycle of a Plugin
- When adding a plugin to an `App`:
  - The app calls `Plugin::build` immediately to register the plugin.
  - Once the app starts, it waits for all registered `Plugin::ready` methods to return `true`.
  - It then calls all registered `Plugin::finish` methods.
  - Finally, it calls all registered `Plugin::cleanup` methods.

This documentation serves as a comprehensive guide for developers looking to utilize the `schedule_runner` module of the `bevy_app` library to manage the execution of schedules in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.