# Bevy Query Builder Module Documentation

This document provides a comprehensive overview of the public API available in the `builder` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to create and manage queries in a Bevy application or game.

## Structs

### `QueryBuilder<'w, D: QueryData = (), F: QueryFilter = ()>`
- **Description**: A builder struct to create `QueryState` instances at runtime.
- **Fields**:
  - `access`: A `FilteredAccess<ComponentId>` that tracks access permissions for components.
  - `world`: A mutable reference to the `World` instance.
  - `or`: A boolean indicating if the query should use logical OR for filters.
  - `first`: A boolean indicating if this is the first filter in an OR expression.
  - `_marker`: A phantom data marker for the types `D` and `F`.
- **Key Points**:
  - This struct allows for the dynamic construction of queries, enabling developers to specify which components to include or exclude.

## Implementations

### `QueryBuilder` Methods
- **`new(world: &'w mut World) -> Self`**:
  - **Description**: Creates a new builder with the accesses required for `D` and `F`.
  - **Parameters**:
    - `world`: A mutable reference to the `World` instance.
  
- **`is_dense(&self) -> bool`**:
  - **Description**: Checks if the components accessed by the query are stored densely.
  
- **`world(&self) -> &World`**:
  - **Description**: Returns a reference to the `World` passed to `Self::new`.
  
- **`world_mut(&mut self) -> &mut World`**:
  - **Description**: Returns a mutable reference to the `World` passed to `Self::new`.
  
- **`extend_access(&mut self, access: FilteredAccess<ComponentId>)`**:
  - **Description**: Adds access to the underlying `FilteredAccess`, respecting logical OR and AND conditions.
  
- **`data<T: QueryData>(&mut self) -> &mut Self`**:
  - **Description**: Adds accesses required for `T` to the builder.
  
- **`filter<T: QueryFilter>(&mut self) -> &mut Self`**:
  - **Description**: Adds a filter from `T` to the builder.
  
- **`with<T: Component>(&mut self) -> &mut Self`**:
  - **Description**: Adds a `With<T>` filter to the `FilteredAccess`.
  
- **`with_id(&mut self, id: ComponentId) -> &mut Self`**:
  - **Description**: Adds a `With<T>` filter from a runtime `ComponentId`.
  
- **`without<T: Component>(&mut self) -> &mut Self`**:
  - **Description**: Adds a `Without<T>` filter to the `FilteredAccess`.
  
- **`without_id(&mut self, id: ComponentId) -> &mut Self`**:
  - **Description**: Adds a `Without<T>` filter from a runtime `ComponentId`.
  
- **`ref_id(&mut self, id: ComponentId) -> &mut Self`**:
  - **Description**: Adds read access for a component identified by `id`.
  
- **`mut_id(&mut self, id: ComponentId) -> &mut Self`**:
  - **Description**: Adds write access for a component identified by `id`.
  
- **`optional(&mut self, f: impl Fn(&mut QueryBuilder)) -> &mut Self`**:
  - **Description**: Adds optional access based on a function that modifies a new `QueryBuilder`.
  
- **`and(&mut self, f: impl Fn(&mut QueryBuilder)) -> &mut Self`**:
  - **Description**: Adds access based on a function that modifies a new `QueryBuilder`, used in AND conditions.
  
- **`or(&mut self, f: impl Fn(&mut QueryBuilder)) -> &mut Self`**:
  - **Description**: Adds access based on a function that modifies a new `QueryBuilder`, used in OR conditions.
  
- **`access(&self) -> &FilteredAccess<ComponentId>`**:
  - **Description**: Returns a reference to the `FilteredAccess` that will be provided to the built `Query`.
  
- **`transmute<NewD: QueryData>(&mut self) -> &mut QueryBuilder<'w, NewD>`**:
  - **Description**: Transmutes the existing builder to add required accesses for a new query data type.
  
- **`transmute_filtered<NewD: QueryData, NewF: QueryFilter>(&mut self) -> &mut QueryBuilder<'w, NewD, NewF>`**:
  - **Description**: Transmutes the existing builder to add required accesses for a new query data type and filter.
  
- **`build(&mut self) -> QueryState<D, F>`**:
  - **Description**: Creates a `QueryState` with the accesses of the builder.

## Example Usage

### Creating a Query with Filters
```rust
use bevy_ecs::prelude::*;

#[derive(Component)]
struct A;

#[derive(Component)]
struct B;

let mut world = World::new();
let entity_a = world.spawn((A, B)).id();
let entity_b = world.spawn((A,)).id();

// Create a query that includes entities with component A and excludes those with component B
let query = QueryBuilder::<(Entity, &B)>::new(&mut world)
    .with::<A>()
    .without::<B>()
    .build();

// Consume the QueryState
let (entity, b) = query.single(&world);
```

### Using Optional Filters
```rust
let query = QueryBuilder::<Entity>::new(&mut world)
    .optional(|builder| {
        builder.with::<A>();
        builder.with::<B>();
    })
    .build();
```

### Using OR Conditions
```rust
let query = QueryBuilder::<Entity>::new(&mut world)
    .or(|builder| {
        builder.with::<A>();
        builder.with::<B>();
    })
    .build();
```

This documentation serves as a comprehensive guide for developers looking to utilize the `builder` module of the `bevy_ecs` library to create and manage queries in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.