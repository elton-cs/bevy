# Bevy Plugin Documentation

This document provides a comprehensive overview of the public API available in the `plugin` module of the `bevy_app` library. It includes details on traits, structs, enums, and functions that can be utilized to create and manage plugins in a Bevy application or game.

## Traits

### `Plugin`
- **Description**: A trait that must be implemented by all plugins. It defines how plugins interact with the app.
- **Methods**:
  - `build(&self, app: &mut App)`: Configures the `App` to which this plugin is added. This method is called when the plugin is registered.
  - `ready(&self, _app: &App) -> bool`: Checks if the plugin has finished its setup. Returns `true` by default.
  - `finish(&self, _app: &mut App)`: Finalizes the plugin setup once all plugins are ready. Does nothing by default.
  - `cleanup(&self, _app: &mut App)`: Runs after all plugins are built and finished, but before the app schedule is executed. Does nothing by default.
  - `name(&self) -> &str`: Configures a name for the plugin, primarily used for checking uniqueness and debugging. Returns the type name by default.
  - `is_unique(&self) -> bool`: Indicates if the plugin can be instantiated multiple times in an `App`. Returns `true` by default.
- **Usage**: Implement this trait to create custom plugins that extend the functionality of the Bevy app.

## Structs

### `PlaceholderPlugin`
- **Description**: A dummy plugin that temporarily occupies an entry in an app's plugin registry.
- **Usage**: Used internally to manage plugin entries without actual functionality.

### `PluginsState`
- **Description**: Represents the state of plugins in the application.
- **Variants**:
  - `Adding`: Indicates that plugins are being added.
  - `Ready`: Indicates that all added plugins are ready.
  - `Finished`: Indicates that the `finish` method has been executed for all added plugins.
  - `Cleaned`: Indicates that the `cleanup` method has been executed for all added plugins.
- **Usage**: Use this enum to track the state of plugins during the application lifecycle.

## Traits

### `Plugins<Marker>`
- **Description**: A trait representing a set of `Plugin`s.
- **Usage**: Implemented for all types that implement `Plugin`, `PluginGroup`, and tuples over `Plugins`.

## Example Usage

### Basic Plugin Implementation
```rust
use bevy_app::{App, Plugin};

pub struct MyPlugin;

impl Plugin for MyPlugin {
    fn build(&self, app: &mut App) {
        app.add_systems(Update, my_system);
    }
}

fn my_system() {
    // Your system logic here
}

fn main() {
    App::new()
        .add_plugins(MyPlugin)
        .run();
}
```

### Using the `ready`, `finish`, and `cleanup` Methods
```rust
pub struct MyAdvancedPlugin;

impl Plugin for MyAdvancedPlugin {
    fn build(&self, app: &mut App) {
        // Setup logic
    }

    fn ready(&self, app: &App) -> bool {
        // Check if ready
        true
    }

    fn finish(&self, app: &mut App) {
        // Finalize setup
    }

    fn cleanup(&self, app: &mut App) {
        // Cleanup resources
    }
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `plugin` module of the `bevy_app` library to manage plugins in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.