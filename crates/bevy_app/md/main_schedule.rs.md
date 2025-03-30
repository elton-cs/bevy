# Bevy Main Schedule Documentation

This document provides a comprehensive overview of the public API available in the `main_schedule` module of the `bevy_app` library. It includes details on structs, enums, and functions that can be utilized to manage the scheduling of systems in a Bevy application or game.

## Structs

### `Main`
- **Description**: Represents the main schedule that contains the app logic evaluated each tick of `App::update()`.
- **Usage**: This schedule runs various phases of the application lifecycle, including startup and update phases.

### `PreStartup`
- **Description**: Schedule that runs before the `Startup` phase.
- **Usage**: Use this schedule to set up any necessary resources or states before the application starts.

### `Startup`
- **Description**: Schedule that runs once when the app starts.
- **Usage**: Place initialization logic that should only run once at the beginning of the application.

### `PostStartup`
- **Description**: Schedule that runs once after the `Startup` phase.
- **Usage**: Use this for any setup that depends on the completion of the `Startup` phase.

### `First`
- **Description**: Schedule that runs first in the main schedule.
- **Usage**: Use this for any systems that need to execute before all other systems.

### `PreUpdate`
- **Description**: Schedule that runs before the `Update` phase.
- **Usage**: Ideal for systems that prepare data or state for the main update logic.

### `RunFixedMainLoop`
- **Description**: Schedule that runs the `FixedMain` schedule in a loop until all relevant elapsed time has been consumed.
- **Usage**: Use this to manage fixed updates, ensuring that time-sensitive logic runs at a consistent rate.

### `FixedFirst`
- **Description**: Schedule that runs first in the `FixedMain` schedule.
- **Usage**: Use this for systems that need to execute before fixed updates.

### `FixedPreUpdate`
- **Description**: Schedule that runs before the `FixedUpdate` phase.
- **Usage**: Ideal for preparing data or state for fixed updates.

### `FixedUpdate`
- **Description**: Schedule that contains most gameplay logic, running at a fixed rate.
- **Usage**: Use this for systems that require deterministic behavior, such as physics and AI.

### `FixedPostUpdate`
- **Description**: Schedule that runs after the `FixedUpdate` phase.
- **Usage**: Use this for systems that need to react to changes made during the fixed update.

### `FixedLast`
- **Description**: Schedule that runs last in the `FixedMain` schedule.
- **Usage**: Use this for any cleanup or finalization logic after fixed updates.

### `FixedMain`
- **Description**: Schedule that contains logic that runs at a fixed rate.
- **Usage**: Use this for systems that need to run consistently regardless of frame rate.

### `Update`
- **Description**: Schedule that contains systems which run once per render frame.
- **Usage**: Use this for UI updates, input handling, and other frame-dependent logic.

### `SpawnScene`
- **Description**: Schedule that contains logic for spawning scenes.
- **Usage**: Use this for systems that manage scene transitions or instantiation.

### `PostUpdate`
- **Description**: Schedule that runs after the `Update` phase.
- **Usage**: Use this for systems that need to synchronize or react to changes made during the update.

### `Last`
- **Description**: Schedule that runs last in the main schedule.
- **Usage**: Use this for any final cleanup or processing after all other schedules.

### `MainScheduleOrder`
- **Description**: Defines the order of schedules to be run for the main schedule.
- **Fields**:
  - `labels`: A vector of schedule labels for the main phase.
  - `startup_labels`: A vector of schedule labels for the startup phase.
- **Usage**: Use this struct to customize the order of execution for schedules.

### `FixedMainScheduleOrder`
- **Description**: Defines the order of schedules to be run for the fixed main schedule.
- **Fields**:
  - `labels`: A vector of schedule labels for the fixed main phase.
- **Usage**: Use this struct to customize the order of execution for fixed schedules.

## Enums

### `RunFixedMainLoopSystem`
- **Description**: Enum for systems that run inside the `RunFixedMainLoop`, allowing for ordering of systems.
- **Variants**:
  - `BeforeFixedMainLoop`: Runs before the fixed update logic.
  - `FixedMainLoop`: Contains the fixed update logic.
  - `AfterFixedMainLoop`: Runs after the fixed update logic.
- **Usage**: Use this enum to categorize systems based on when they should run in relation to fixed updates.

## Functions

### `MainSchedulePlugin`
- **Description**: A plugin that initializes the main schedule and its associated resources.
- **Usage**: Add this plugin to your app to set up the main scheduling system.

### `Main::run_main`
- **Description**: A system that runs the main schedule.
- **Usage**: This function is called to execute the main application logic during each update.

### `FixedMain::run_fixed_main`
- **Description**: A system that runs the fixed main schedule.
- **Usage**: This function is called to execute fixed updates during each frame.

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

This documentation serves as a comprehensive guide for developers looking to utilize the `main_schedule` module of the `bevy_app` library to manage scheduling in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.