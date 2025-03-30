# Bevy App Library Documentation

This document provides a comprehensive overview of the public API available in the `bevy_app` library. It includes details on modules, structs, enums, traits, and functions that can be utilized to build applications or games using Bevy.

## Modules

### `app`
- **Description**: Contains the core application structure and functionality.
- **Public Items**: 
  - `App`: The main application struct.
  - `AppExit`: Enum for application exit states.
  - `AppError`: Enum for handling application errors.

### `main_schedule`
- **Description**: Defines the main scheduling system for the application.
- **Public Items**:
  - Various scheduling labels such as `First`, `Last`, `Update`, etc., which are used to organize and control the execution order of systems.

### `panic_handler`
- **Description**: Manages panic handling within the application.
- **Public Items**: Functions and types related to handling panics gracefully.

### `plugin`
- **Description**: Contains the plugin system for extending application functionality.
- **Public Items**:
  - `Plugin`: Trait for creating plugins.
  - `PluginGroup`: Struct for grouping multiple plugins.

### `plugin_group`
- **Description**: Defines groups of plugins for easier management.
- **Public Items**: Functions and types related to managing collections of plugins.

### `schedule_runner`
- **Description**: Manages the execution of schedules within the application.
- **Public Items**: Functions and types related to running schedules.

### `sub_app`
- **Description**: Manages sub-applications within the main application.
- **Public Items**: 
  - `SubApp`: Struct for creating and managing sub-applications.

### `terminal_ctrl_c_handler`
- **Description**: Handles terminal control signals (only available on non-WASM targets).
- **Public Items**: Functions for managing Ctrl+C signals in terminal applications.

## Public Functions and Types

### `prelude`
- **Description**: A module that re-exports the most common types and functions for convenience.
- **Public Items**:
  - `App`: The main application struct.
  - `AppExit`: Enum for application exit states.
  - Scheduling labels: `First`, `FixedFirst`, `FixedLast`, `FixedPostUpdate`, `FixedPreUpdate`, `FixedUpdate`, `Last`, `Main`, `PostStartup`, `PostUpdate`, `PreStartup`, `PreUpdate`, `RunFixedMainLoop`, `RunFixedMainLoopSystem`, `SpawnScene`, `Startup`, `Update`.
  - `SubApp`: Struct for managing sub-applications.
  - `Plugin`: Trait for creating plugins.
  - `PluginGroup`: Struct for grouping plugins.

## Example Usage

```rust
use bevy_app::prelude::*;

fn main() {
    App::new()
        .add_plugins(DefaultPlugins)
        .add_systems(Update, my_system)
        .run();
}

fn my_system() {
    // Your system logic here
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `bevy_app` library to build applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.