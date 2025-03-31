# Bevy Query State Module Documentation

This document provides a comprehensive overview of the public API available in the `state` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to manage the state of queries in a Bevy application or game.

## Enums

### `QueryEntityError<'w>`
- **Description**: An error that occurs when retrieving a specific `Entity`'s query result from `Query` or `QueryState`.
- **Variants**:
  - **`QueryDoesNotMatch(Entity, UnsafeWorldCell<'w>)`**:
    - **Description**: Indicates that the given `Entity`'s components do not match the query.
  - **`NoSuchEntity(Entity)`**:
    - **Description**: Indicates that the given `Entity` does not exist.
  - **`AliasedMutability(Entity)`**:
    - **Description**: Indicates that the `Entity` was requested mutably more than once.

### `QuerySingleError`
- **Description**: An error that occurs when evaluating a `Query` or `QueryState` as a single expected result via `get_single` or `get_single_mut`.
- **Variants**:
  - **`NoEntities(&'static str)`**:
    - **Description**: Indicates that no entity fits the query.
  - **`MultipleEntities(&'static str)`**:
    - **Description**: Indicates that multiple entities fit the query.

## Structs

### `StorageId`
- **Description**: An ID for either a table or an archetype, used for query iteration.
- **Key Points**:
  - This union allows for efficient storage and access based on the type of query being executed (table or archetype).

### `QueryState<D: QueryData, F: QueryFilter>`
- **Description**: Provides scoped access to a `World` state according to a given `QueryData` and `QueryFilter`.
- **Fields**:
  - `world_id`: The ID of the world this query state is associated with.
  - `archetype_generation`: The generation of the archetype.
  - `matched_tables`: Metadata about the tables matched by this query.
  - `matched_archetypes`: Metadata about the archetypes matched by this query.
  - `component_access`: The filtered access computed by combining the access of `D` and `F`.
  - `matched_storage_ids`: A vector of matched storage IDs.
  - `is_dense`: A boolean indicating if the query iteration is dense.
  - `fetch_state`: The state needed to compute the fetch struct used to retrieve data.
  - `filter_state`: The state needed for the query filter.
- **Key Points**:
  - This struct caches metadata about which tables or archetypes are matched by the query, allowing for efficient query execution.

## Implementations

### `QueryState` Methods
- **`new(world: &mut World) -> Self`**:
  - **Description**: Creates a new `QueryState` from a given `World`.
  
- **`as_readonly(&self) -> &QueryState<D::ReadOnly, F>`**:
  - **Description**: Converts this `QueryState` reference to a read-only variant.
  
- **`as_nop(&self) -> &QueryState<NopWorldQuery<D>, F>`**:
  - **Description**: Converts this `QueryState` reference to a `QueryState` that does not return any data.
  
- **`transmute<'a, NewD: QueryData>(&self, world: impl Into<UnsafeWorldCell<'a>>) -> QueryState<NewD>`**:
  - **Description**: Transmutes the `QueryState` to a new type signature.
  
- **`join<'a, OtherD: QueryData, NewD: QueryData>(&self, world: impl Into<UnsafeWorldCell<'a>>, other: &QueryState<OtherD>) -> QueryState<NewD, ()>`**:
  - **Description**: Combines two queries, returning the intersection of their results.
  
- **`get<'w>(&mut self, world: &'w World, entity: Entity) -> Result<ROQueryItem<'w, D>, QueryEntityError<'w>>`**:
  - **Description**: Retrieves the query result for a specific entity.
  
- **`get_many<'w, const N: usize>(&mut self, world: &'w World, entities: [Entity; N]) -> Result<[ROQueryItem<'w, D>; N], QueryEntityError<'w>>`**:
  - **Description**: Retrieves the query results for a list of entities.

## Example Usage

### Creating a Query State
```rust
use bevy_ecs::prelude::*;

#[derive(Component)]
struct MyComponent;

fn my_system(world: &mut World) {
    let query_state = world.query::<&MyComponent>();
    // Use query_state to access components
}
```

### Using Transmute
```rust
fn my_transmute_system(world: &mut World) {
    let query_state = world.query::<&MyComponent>();
    let new_query_state = query_state.transmute::<&AnotherComponent>(&world);
    // Use new_query_state to access components
}
```

### Joining Queries
```rust
fn my_joined_system(world: &mut World) {
    let query_state_a = world.query::<&ComponentA>();
    let query_state_b = world.query::<&ComponentB>();
    let joined_query = query_state_a.join(&world, &query_state_b);
    // Use joined_query to access components from both queries
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `state` module of the `bevy_ecs` library to manage query states in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.