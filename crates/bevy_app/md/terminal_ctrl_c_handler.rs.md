# Bevy Terminal Ctrl+C Handler Documentation

This document provides a comprehensive overview of the public API available in the `terminal_ctrl_c_handler` module of the `bevy_app` library. It includes details on structs, functions, and implementations that can be utilized to manage graceful exit behavior when a user presses `Ctrl+C` in a terminal running a Bevy application.

## Structs

### `TerminalCtrlCHandlerPlugin`
- **Description**: A plugin that gracefully handles `Ctrl+C` events by emitting an `AppExit` event. This plugin is part of the `DefaultPlugins`.
- **Methods**:
  - `gracefully_exit()`: Sends the `AppExit` event to all apps using this plugin to make them gracefully exit.
  - `exit_on_flag(mut events: EventWriter<AppExit>)`: Checks if the exit flag is set and sends an `AppExit` event with code 130 if it is.
- **Usage**: Add this plugin to your app to handle `Ctrl+C` events gracefully.
- **Example**:
  ```rust
  use bevy_app::{App, NoopPluginGroup as MinimalPlugins, TerminalCtrlCHandlerPlugin};

  fn main() {
      App::new()
          .add_plugins(MinimalPlugins)
          .add_plugins(TerminalCtrlCHandlerPlugin)
          .run();
  }
  ```

## Functions

### `gracefully_exit`
- **Description**: A static method that sets a flag indicating that the application should exit gracefully.
- **Usage**: Call this method in your custom `Ctrl+C` handler to ensure Bevy exits properly.
- **Example**:
  ```rust
  ctrlc::set_handler(move || {
      // Other cleanup code...
      TerminalCtrlCHandlerPlugin::gracefully_exit();
  });
  ```

### `exit_on_flag`
- **Description**: A system function that checks the exit flag and sends an `AppExit` event if the flag is set.
- **Usage**: This function is added to the app's update schedule to monitor for exit requests.
- **Example**:
  ```rust
  app.add_systems(Update, TerminalCtrlCHandlerPlugin::exit_on_flag);
  ```

## Example Usage

### Custom Ctrl+C Handler
```rust
use bevy_app::{App, NoopPluginGroup as DefaultPlugins, TerminalCtrlCHandlerPlugin, ctrlc};

fn main() {
    // Custom Ctrl+C handler
    ctrlc::set_handler(move || {
        // Other cleanup code...
        TerminalCtrlCHandlerPlugin::gracefully_exit();
    }).expect("Error setting Ctrl+C handler");

    App::new()
        .add_plugins(DefaultPlugins)
        .run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `terminal_ctrl_c_handler` module of the `bevy_app` library to manage graceful exit behavior in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.