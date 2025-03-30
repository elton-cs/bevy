# Bevy Plugin Group Documentation

This document provides a comprehensive overview of the public API available in the `plugin_group` module of the `bevy_app` library. It includes details on macros, structs, traits, and functions that can be utilized to create and manage groups of plugins in a Bevy application or game.

## Macros

### `plugin_group!`
- **Description**: A macro for generating a well-documented `PluginGroup` from a list of `Plugin` paths.
- **Usage**:
  - Each plugin must implement the `Default` trait.
  - The macro allows for the inclusion of documentation comments and annotations for each plugin.
  - It supports conditional compilation for plugins based on feature flags.
- **Example**:
  ```rust
  plugin_group! {
      /// This is a group of physics-related plugins.
      #[derive(Debug)]
      pub struct PhysicsPlugins {
          :TickratePlugin,
          collision::capsule:::CapsuleCollisionPlugin,
          velocity:::VelocityPlugin,
          #[cfg(feature = "external_forces")]
          features:::ForcePlugin,
          #[custom(cfg(target_arch = "wasm32"))]
          web:::WebCompatibilityPlugin,
          #[plugin_group]
          audio:::AudioPlugins,
          #[doc(hidden)]
          internal:::InternalPlugin
      }
  }
  ```

## Traits

### `PluginGroup`
- **Description**: A trait that combines multiple `Plugin`s into a single unit.
- **Methods**:
  - `build(self) -> PluginGroupBuilder`: Configures the `Plugin`s that are to be added.
  - `name() -> String`: Configures a name for the `PluginGroup`, primarily used for debugging.
  - `set<T: Plugin>(self, plugin: T) -> PluginGroupBuilder`: Sets the value of the given `Plugin`, if it exists.
- **Usage**: Implement this trait to create custom plugin groups that encapsulate related plugins.

## Structs

### `PluginGroupBuilder`
- **Description**: Facilitates the creation and configuration of a `PluginGroup`.
- **Fields**:
  - `group_name`: The name of the plugin group.
  - `plugins`: A map of plugin entries.
  - `order`: A vector defining the order of plugins.
- **Methods**:
  - `start<PG: PluginGroup>() -> Self`: Starts a new builder for the `PluginGroup`.
  - `add<T: Plugin>(mut self, plugin: T) -> Self`: Adds a `Plugin` at the end of the group.
  - `add_group(mut self, group: impl PluginGroup) -> Self`: Adds a `PluginGroup` at the end of this builder.
  - `add_before<Target: Plugin>(mut self, plugin: impl Plugin) -> Self`: Adds a `Plugin` before the specified target.
  - `add_after<Target: Plugin>(mut self, plugin: impl Plugin) -> Self`: Adds a `Plugin` after the specified target.
  - `enable<T: Plugin>(mut self) -> Self`: Enables a `Plugin`.
  - `disable<T: Plugin>(mut self) -> Self`: Disables a `Plugin`.
  - `finish(mut self, app: &mut App)`: Consumes the builder and builds the contained `Plugin`s in the specified order.
- **Usage**: Use this struct to manage the addition and configuration of plugins in a group.

### `NoopPluginGroup`
- **Description**: A plugin group that does nothing. Useful for examples.
- **Usage**: Can be used as a placeholder for minimal plugin groups in examples.
- **Example**:
  ```rust
  use bevy_app::NoopPluginGroup as MinimalPlugins;

  fn main() {
      App::new().add_plugins(MinimalPlugins).run();
  }
  ```

## Example Usage

```rust
use bevy_app::{App, PluginGroup, PluginGroupBuilder};

fn main() {
    let group = PluginGroupBuilder::start::<NoopPluginGroup>()
        .add(PluginA)
        .add(PluginB)
        .add(PluginC);

    // Build and use the plugin group
    let app = App::new();
    group.finish(&mut app);
    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `plugin_group` module of the `bevy_app` library to manage groups of plugins in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.