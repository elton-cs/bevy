# Bevy Query Fetch Module Documentation

This document provides a comprehensive overview of the public API available in the `fetch` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to fetch data from a `World` using queries in a Bevy application or game.

## Enums

### `QueryEntityError<'w>`
- **Description**: An error that occurs when retrieving a specific `Entity`'s query result from `Query` or `QueryState`.
- **Variants**:
  - **`QueryDoesNotMatch(Entity, UnsafeWorldCell<'w>)`**:
    - **Description**: Indicates that the given `Entity`'s components do not match the query.
    - **Key Points**: Raised when the entity lacks a requested component or has a component that the query filters out.
  - **`NoSuchEntity(Entity)`**:
    - **Description**: Indicates that the given `Entity` does not exist.
    - **Key Points**: Raised when attempting to access an entity that has been despawned or never existed.
  - **`AliasedMutability(Entity)`**:
    - **Description**: Indicates that the `Entity` was requested mutably more than once.
    - **Key Points**: Relevant when using `QueryState::get_many_mut`, which requires unique mutable access to entities.

### `QuerySingleError`
- **Description**: An error that occurs when evaluating a `Query` or `QueryState` as a single expected result via `get_single` or `get_single_mut`.
- **Variants**:
  - **`NoEntities(&'static str)`**:
    - **Description**: Indicates that no entity fits the query.
  - **`MultipleEntities(&'static str)`**:
    - **Description**: Indicates that multiple entities fit the query.

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

### `StorageSwitch<C: Component, T: Copy, S: Copy>`
- **Description**: A compile-time checked union of two different types that differs based on the `StorageType` of a given component.
- **Fields**:
  - `table`: The table variant, requires the component to be a table component.
  - `sparse_set`: The sparse set variant, requires the component to be a sparse set component.
  - `_marker`: A phantom data marker for the component type `C`.
- **Key Points**:
  - This struct is used to handle different storage types for components in a unified manner.

## Implementations

### `QueryBuilder` Methods
- **`new(world: &'w mut World) -> Self`**:
  - **Description**: Creates a new builder with the accesses required for `D` and `F`.
  
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
  
- **`without<T: Component>(&mut self) -> &mut Self`**:
  - **Description**: Adds a `Without<T>` filter to the `FilteredAccess`.
  
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
  
- **`build(&mut self) -> QueryState<D, F>`**:
  - **Description**: Creates a `QueryState` with the accesses of the builder.

### `WorldQuery` Implementations
- **`unsafe impl WorldQuery for Entity`**:
  - **Description**: Allows querying of entities directly.
  
- **`unsafe impl WorldQuery for EntityLocation`**:
  - **Description**: Allows querying of entity location metadata.
  
- **`unsafe impl WorldQuery for &T`**:
  - **Description**: Allows read-only access to components.
  
- **`unsafe impl WorldQuery for &mut T`**:
  - **Description**: Allows mutable access to components.
  
- **`unsafe impl WorldQuery for Option<T>`**:
  - **Description**: Allows querying with optional components.

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

This documentation serves as a comprehensive guide for developers looking to utilize the `fetch` module of the `bevy_ecs` library to fetch data from a `World` using queries in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.