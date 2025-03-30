# Bevy SubApp Documentation

This document provides a comprehensive overview of the public API available in the `sub_app` module of the `bevy_app` library. It includes details on structs, enums, and functions that can be utilized to manage sub-applications in a Bevy application or game.

## Structs

### `SubApp`
- **Description**: Represents a secondary application with its own `World`. SubApps can run independently of the main application.
- **Fields**:
  - `world`: The data of this application, represented as a `World`.
  - `plugin_registry`: A list of plugins that have been added to this sub-app.
  - `plugin_names`: A set of names for the plugins that have been added, used to track duplicates.
  - `plugin_build_depth`: A counter to prevent updates while plugins are being built.
  - `plugins_state`: The current state of the plugins (e.g., Adding, Ready, Finished, Cleaned).
  - `update_schedule`: An optional schedule that will be run by the `update` method.
  - `extract`: An optional function for extracting data from the main world to the sub-app's world.
- **Methods**:
  - `new() -> Self`: Returns a default, empty `SubApp`.
  - `world(&self) -> &World`: Returns a reference to the `World`.
  - `world_mut(&mut self) -> &mut World`: Returns a mutable reference to the `World`.
  - `run_default_schedule(&mut self)`: Runs the default schedule without clearing internal trackers.
  - `update(&mut self)`: Runs the default schedule and updates internal component trackers.
  - `extract(&mut self, world: &mut World)`: Extracts data from the main world into the sub-app's world using the registered extract method.
  - `set_extract<F>(&mut self, extract: F) -> &mut Self`: Sets the method that will be called by `extract`.
  - `take_extract(&mut self) -> Option<ExtractFn>`: Takes the extract function out of the app, if any was set.
  - `insert_resource<R: Resource>(&mut self, resource: R) -> &mut Self`: Inserts a resource into the sub-app's world.
  - `init_resource<R: Resource + FromWorld>(&mut self) -> &mut Self`: Initializes a resource in the sub-app's world.
  - `add_systems<M>(&mut self, schedule: impl ScheduleLabel, systems: impl IntoSystemConfigs<M>) -> &mut Self`: Adds systems to the specified schedule.
  - `register_system<I, O, M>(&mut self, system: impl IntoSystem<I, O, M> + 'static) -> SystemId<I, O>`: Registers a system and returns its ID.
  - `configure_sets(&mut self, schedule: impl ScheduleLabel, sets: impl IntoSystemSetConfigs) -> &mut Self`: Configures system sets in the provided schedule.
  - `add_schedule(&mut self, schedule: Schedule) -> &mut Self`: Adds a schedule to the sub-app.
  - `init_schedule(&mut self, label: impl ScheduleLabel) -> &mut Self`: Initializes a schedule under the provided label.
  - `get_schedule(&self, label: impl ScheduleLabel) -> Option<&Schedule>`: Returns a reference to the specified schedule.
  - `get_schedule_mut(&mut self, label: impl ScheduleLabel) -> Option<&mut Schedule>`: Returns a mutable reference to the specified schedule.
  - `edit_schedule(&mut self, label: impl ScheduleLabel, f: impl FnMut(&mut Schedule)) -> &mut Self`: Edits the specified schedule.
  - `configure_schedules(&mut self, schedule_build_settings: ScheduleBuildSettings) -> &mut Self`: Configures schedules in the sub-app.
  - `allow_ambiguous_component<T: Component>(&mut self) -> &mut Self`: Allows ambiguous components in the sub-app.
  - `allow_ambiguous_resource<T: Resource>(&mut self) -> &mut Self`: Allows ambiguous resources in the sub-app.
  - `ignore_ambiguity<M1, M2, S1, S2>(&mut self, schedule: impl ScheduleLabel, a: S1, b: S2) -> &mut Self`: Ignores ambiguity between two system sets.
  - `add_event<T>(&mut self) -> &mut Self`: Adds an event to the sub-app.
  - `add_plugins<M>(&mut self, plugins: impl Plugins<M>) -> &mut Self`: Adds plugins to the sub-app.
  - `is_plugin_added<T>(&self) -> bool`: Checks if a plugin has been added to the sub-app.
  - `get_added_plugins<T>(&self) -> Vec<&T>`: Returns a vector of references to all added plugins of type `T`.
  - `is_building_plugins(&self) -> bool`: Returns `true` if any plugins are currently being built.
  - `plugins_state(&mut self) -> PluginsState`: Returns the state of plugins in the sub-app.
  - `finish(&mut self)`: Runs the `finish` method for each plugin.
  - `cleanup(&mut self)`: Runs the `cleanup` method for each plugin.
  - `register_type<T: bevy_reflect::GetTypeRegistration>(&mut self) -> &mut Self`: Registers a type in the sub-app.
  - `register_type_data<T: bevy_reflect::Reflect + bevy_reflect::TypePath, D: bevy_reflect::TypeData + bevy_reflect::FromType<T>>(&mut self) -> &mut Self`: Registers type data in the sub-app.
  - `register_function<F, Marker>(&mut self, function: F) -> &mut Self`: Registers a function in the sub-app.
  - `register_function_with_name<F, Marker>(&mut self, name: impl Into<alloc::borrow::Cow<'static, str>>, function: F) -> &mut Self`: Registers a function with a name in the sub-app.

### `SubApps`
- **Description**: A collection of sub-apps that belong to an `App`.
- **Fields**:
  - `main`: The primary sub-app that contains the "main" world.
  - `sub_apps`: A map of labeled sub-apps.
- **Methods**:
  - `update(&mut self)`: Calls `update` for the main sub-app and extracts data for other sub-apps.
  - `iter(&self)`: Returns an iterator over the sub-apps, starting with the main one.
  - `iter_mut(&mut self)`: Returns a mutable iterator over the sub-apps, starting with the main one.
  - `update_subapp_by_label(&mut self, label: impl AppLabel)`: Updates a sub-app by its label.

## Example Usage

### Creating and Using a SubApp
```rust
use bevy_app::{App, AppLabel, SubApp, Main};
use bevy_ecs::prelude::*;

#[derive(Resource, Default)]
struct Val(pub i32);

#[derive(Debug, Clone, Copy, Hash, PartialEq, Eq, AppLabel)]
struct ExampleApp;

fn main() {
    let mut app = App::new();
    app.insert_resource(Val(10));

    let mut sub_app = SubApp::new();
    sub_app.insert_resource(Val(100));

    sub_app.set_extract(|main_world, sub_world| {
        sub_world.resource_mut::<Val>().0 = main_world.resource::<Val>().0;
    });

    sub_app.add_systems(Main, |counter: Res<Val>| {
        assert_eq!(counter.0, 10);
    });

    app.insert_sub_app(ExampleApp, sub_app);
    app.run();
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `sub_app` module of the `bevy_app` library to manage sub-applications in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.