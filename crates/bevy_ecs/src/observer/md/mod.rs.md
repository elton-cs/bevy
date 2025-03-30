# Bevy Observer Module Documentation

This document provides a comprehensive overview of the public API available in the `observer` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to manage observers in a Bevy application or game.

## Modules

### `entity_observer`
- **Description**: Contains the `ObservedBy` struct, which tracks entities that observe a specific entity.
- **Key Types**:
  - `ObservedBy`: A component that holds a list of entities observing the entity it is attached to.

### `runner`
- **Description**: Contains the `ObserverState` struct and related functionality for managing observer behavior.
- **Key Types**:
  - `ObserverState`: Holds the state and behavior of an observer, including its descriptor and runner function.

### `trigger_event`
- **Description**: Contains functionality for triggering events and managing event targets.
- **Key Types**:
  - `TriggerEvent`: A command struct that triggers an event for specified targets.
  - `EmitDynamicTrigger`: A struct for emitting triggers for dynamic component IDs.

## Structs

### `Trigger<'w, E, B: Bundle = ()>`
- **Description**: A struct that contains information about a triggered event, including the event data and propagation information.
- **Fields**:
  - `event`: A mutable reference to the event being triggered.
  - `propagate`: A mutable reference to a boolean indicating whether the event should propagate.
  - `trigger`: An instance of `ObserverTrigger` that contains metadata about the trigger.
  - `_marker`: A phantom data marker for the bundle type.
- **Key Points**:
  - This struct is used to encapsulate the event and its propagation behavior, allowing observers to respond to events dynamically.

### `ObserverDescriptor`
- **Description**: A struct that describes what an observer observes, including events, components, and entities.
- **Fields**:
  - `events`: A vector of `ComponentId` representing the events the observer is watching.
  - `components`: A vector of `ComponentId` representing the components the observer is watching.
  - `entities`: A vector of `Entity` representing the entities the observer is watching.
- **Key Points**:
  - This struct is essential for defining the behavior of observers and what they should respond to.

### `ObserverTrigger`
- **Description**: A struct that contains metadata for a triggered observer, including the observer entity and the event type.
- **Fields**:
  - `observer`: The `Entity` of the observer handling the trigger.
  - `event_type`: The `ComponentId` of the event type being triggered.
  - `components`: A vector of `ComponentId` representing the components that triggered the observer.
  - `entity`: The `Entity` that triggered the observer.
- **Key Points**:
  - This struct provides the necessary context for observers to handle events appropriately.

## Implementations

### `Trigger` Methods
- **`new(event: &'w mut E, propagate: &'w mut bool, trigger: ObserverTrigger) -> Self`**:
  - **Description**: Creates a new trigger for the given event and observer information.
  - **Parameters**:
    - `event`: A mutable reference to the event data.
    - `propagate`: A mutable reference to a boolean indicating propagation.
    - `trigger`: An instance of `ObserverTrigger`.
  
- **`event_type(&self) -> ComponentId`**:
  - **Description**: Returns the event type of this trigger.
  
- **`event(&self) -> &E`**:
  - **Description**: Returns a reference to the triggered event.
  
- **`event_mut(&mut self) -> &mut E`**:
  - **Description**: Returns a mutable reference to the triggered event.
  
- **`entity(&self) -> Entity`**:
  - **Description**: Returns the `Entity` that triggered the observer.
  
- **`propagate(&mut self, should_propagate: bool)`**:
  - **Description**: Enables or disables event propagation for the trigger.
  
- **`get_propagate(&self) -> bool`**:
  - **Description**: Returns the current value of the propagation flag.

### `Observer` Methods
- **`new<E: Event, B: Bundle, M>(system: impl IntoObserverSystem<E, B, M>) -> Self`**:
  - **Description**: Creates a new observer that listens for a specific event.
  
- **`with_entity(mut self, entity: Entity) -> Self`**:
  - **Description**: Adds a specific entity to the observer's watch list.
  
- **`watch_entity(&mut self, entity: Entity)`**:
  - **Description**: Adds a specific entity to the observer's watch list after the observer has been spawned.

## Example Usage

### Creating and Using an Observer
```rust
use bevy_ecs::prelude::*;

#[derive(Event)]
struct MyEvent {
    message: String,
}

fn setup(world: &mut World) {
    world.add_observer(|trigger: Trigger<MyEvent>| {
        println!("{}", trigger.event().message);
    });

    // Ensure observers are registered
    world.flush();
}

fn trigger_event(world: &mut World) {
    world.trigger(MyEvent {
        message: "Hello, Bevy!".into(),
    });
}
```

### Adding an Observer to an Entity
```rust
fn add_observer_to_entity(world: &mut World, entity: Entity) {
    let observer = Observer::new(|trigger: Trigger<MyEvent>| {
        println!("Observer triggered: {}", trigger.event().message);
    });
    world.entity_mut(entity).insert(observer);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `observer` module of the `bevy_ecs` library to manage observers in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.
