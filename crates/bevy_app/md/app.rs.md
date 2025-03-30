# Bevy App Documentation

This document provides a comprehensive overview of the public API available in the `bevy_app` library. It includes details on structs, enums, traits, and functions that can be utilized to build applications or games using Bevy.

## Structs

### `App`
- **Description**: The primary API for writing user applications in Bevy. It automates the setup of a standard lifecycle and provides interface glue for plugins.
- **Usage**:
  - Create a new app instance using `App::new()`.
  - Add systems, plugins, and events to the app.
  - Run the app using `app.run()`.

### `AppExit`
- **Description**: An event that indicates the app should exit. It can represent either a successful exit or an error exit with a specific code.
- **Variants**:
  - `Success`: Indicates the app exited without problems.
  - `Error(NonZero<u8>)`: Indicates an error occurred, holding the exit code.
- **Usage**: Use `AppExit::error()` to create an error exit and `AppExit::from_code(code)` to create an exit based on a specific code.

### `AppError`
- **Description**: An error type used within the app for handling plugin-related errors.
- **Variants**:
  - `DuplicatePlugin { plugin_name: String }`: Indicates a plugin was added more than once.
- **Usage**: Handle errors when adding plugins to the app.

## Enums

### `PluginsState`
- **Description**: Represents the state of plugins within the app.
- **Variants**:
  - `Adding`: Indicates plugins are currently being added.
  - `Ready`: Indicates plugins are ready to be used.
  - `Finished`: Indicates all plugins have been processed.
  - `Cleaned`: Indicates the app has been cleaned up after finishing.
- **Usage**: Check the state of plugins using `app.plugins_state()`.

## Traits

### `Plugin`
- **Description**: A trait that must be implemented by all plugins. It defines how plugins interact with the app.
- **Methods**:
  - `build(&self, app: &mut App)`: Called to configure the app with the plugin's functionality.
- **Usage**: Implement this trait to create custom plugins that extend the functionality of the Bevy app.

### `AppLabel`
- **Description**: A trait for strongly-typed labels used to identify an `App`.
- **Usage**: Implement this trait for custom labels to be used with sub-apps.

## Public Functions

### `App::new()`
- **Description**: Creates a new `App` instance with default settings.
- **Usage**: Call this function to start building a new Bevy application.

### `App::empty()`
- **Description**: Creates a new empty `App` with minimal configuration.
- **Usage**: Use this when you want to customize the app's scheduling and exit handling.

### `App::add_plugins()`
- **Description**: Installs a collection of plugins into the app.
- **Usage**: Pass a plugin or a tuple of plugins to this function to add them to the app.

### `App::add_systems()`
- **Description**: Adds one or more systems to the specified schedule in the app's schedules.
- **Usage**: Use this to define the behavior of your app by adding systems that will run during updates.

### `App::run()`
- **Description**: Runs the app by calling its runner function.
- **Usage**: Call this method to start the app's main loop.

### `App::update()`
- **Description**: Runs the default schedules of all sub-apps once.
- **Usage**: Call this method to update the app's state and process events.

### `App::set_runner()`
- **Description**: Sets the function that will be called when the app is run.
- **Usage**: Use this to define a custom runner function for the app.

### `App::add_event()`
- **Description**: Initializes event handling by inserting an event queue resource.
- **Usage**: Call this method to define custom events that your app will handle.

### `App::insert_resource()`
- **Description**: Inserts a resource into the app, overwriting any existing resource of the same type.
- **Usage**: Use this to manage shared state across systems.

### `App::init_resource()`
- **Description**: Inserts a resource initialized with its default value into the app if there is no existing instance.
- **Usage**: Call this to ensure a resource is available with default settings.

### `App::finish()`
- **Description**: Runs the `finish` method for each plugin.
- **Usage**: Call this method to finalize the app's state before cleanup.

### `App::cleanup()`
- **Description**: Runs the `cleanup` method for each plugin.
- **Usage**: Call this method to clean up resources after the app has finished running.

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