# Bevy Query Iteration Module Documentation

This document provides a comprehensive overview of the public API available in the `iter` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to iterate over query results in a Bevy application or game.

## Structs

### `QueryIter<'w, 's, D: QueryData, F: QueryFilter>`
- **Description**: An iterator over query results of a `Query`.
- **Fields**:
  - `world`: A mutable reference to the `World` instance.
  - `tables`: A reference to the `Tables` in the world.
  - `archetypes`: A reference to the `Archetypes` in the world.
  - `query_state`: A reference to the `QueryState` for the query.
  - `cursor`: A cursor for tracking the current position in the query iteration.
- **Key Points**:
  - This struct is created by the `Query::iter` and `Query::iter_mut` methods, allowing for iteration over query results.

### `QuerySortedIter<'w, 's, D: QueryData, F: QueryFilter, I>`
- **Description**: An iterator over sorted query results of a `Query`.
- **Fields**:
  - `entity_iter`: An iterator over entities.
  - `entities`: A reference to the `Entities` in the world.
  - `tables`: A reference to the `Tables` in the world.
  - `archetypes`: A reference to the `Archetypes` in the world.
  - `fetch`: The fetch state for the query.
  - `query_state`: A reference to the `QueryState` for the query.
- **Key Points**:
  - This struct is created by various sorting methods on `QueryIter`, allowing for sorted access to query results.

### `QueryManyIter<'w, 's, D: QueryData, F: QueryFilter, I>`
- **Description**: An iterator over query items generated from an iterator of `Entity`s.
- **Fields**:
  - `entity_iter`: An iterator over entities.
  - `entities`: A reference to the `Entities` in the world.
  - `tables`: A reference to the `Tables` in the world.
  - `archetypes`: A reference to the `Archetypes` in the world.
  - `fetch`: The fetch state for the query.
  - `filter`: The filter state for the query.
  - `query_state`: A reference to the `QueryState` for the query.
- **Key Points**:
  - This struct is created by the `Query::iter_many` and `Query::iter_many_mut` methods, allowing for iteration over multiple entities.

### `QueryCombinationIter<'w, 's, D: QueryData, F: QueryFilter, const K: usize>`
- **Description**: An iterator over `K`-sized combinations of query items without repetition.
- **Fields**:
  - `tables`: A reference to the `Tables` in the world.
  - `archetypes`: A reference to the `Archetypes` in the world.
  - `query_state`: A reference to the `QueryState` for the query.
  - `cursors`: An array of cursors for tracking the current position in the query iteration.
- **Key Points**:
  - This struct allows for generating combinations of query results, enabling complex queries that require multiple components.

## Implementations

### `QueryIter` Methods
- **`new(world: UnsafeWorldCell<'w>, query_state: &'s QueryState<D, F>, last_run: Tick, this_run: Tick) -> Self`**:
  - **Description**: Creates a new `QueryIter` instance.
  - **Safety**: Requires that `world` has permission to access components registered in `query_state`.

- **`remaining(&self) -> QueryIter<'w, 's, D, F>`**:
  - **Description**: Creates a new iterator yielding the same remaining items of the current one.

- **`remaining_mut(&mut self) -> QueryIter<'_, 's, D, F>`**:
  - **Description**: Creates a new mutable iterator yielding the same remaining items of the current one.

- **`fetch_next(&mut self) -> Option<D::Item<'_>>`**:
  - **Description**: Retrieves the next result from the query.

### `QuerySortedIter` Methods
- **`new(world: UnsafeWorldCell<'w>, query_state: &'s QueryState<D, F>, entity_list: EntityList, last_run: Tick, this_run: Tick) -> Self`**:
  - **Description**: Creates a new `QuerySortedIter` instance.
  - **Safety**: Requires that `world` has permission to access components registered in `query_state`.

- **`fetch_next(&mut self) -> Option<D::Item<'_>>`**:
  - **Description**: Retrieves the next sorted result from the query.

### `QueryManyIter` Methods
- **`new(world: UnsafeWorldCell<'w>, query_state: &'s QueryState<D, F>, entity_list: EntityList, last_run: Tick, this_run: Tick) -> Self`**:
  - **Description**: Creates a new `QueryManyIter` instance.
  - **Safety**: Requires that `world` has permission to access components registered in `query_state`.

- **`fetch_next(&mut self) -> Option<D::Item<'_>>`**:
  - **Description**: Retrieves the next result from the query.

### `QueryCombinationIter` Methods
- **`new(world: UnsafeWorldCell<'w>, query_state: &'s QueryState<D, F>, last_run: Tick, this_run: Tick) -> Self`**:
  - **Description**: Creates a new `QueryCombinationIter` instance.
  - **Safety**: Requires that `world` has permission to access components registered in `query_state`.

- **`fetch_next(&mut self) -> Option<[D::Item<'_>; K]>`**:
  - **Description**: Retrieves the next combination of queried components.

## Example Usage

### Iterating Over Query Results
```rust
use bevy_ecs::prelude::*;

#[derive(Component)]
struct MyComponent;

fn my_system(query: Query<&MyComponent>) {
    for component in &query {
        // Process each component
    }
}
```

### Using Query Iterators
```rust
fn my_combination_system(query: Query<(&MyComponent, &AnotherComponent)>) {
    for combination in query.iter_combinations() {
        // Process each combination of components
    }
}
```

### Sorting Query Results
```rust
fn my_sorted_system(query: Query<&MyComponent>) {
    let sorted: Vec<_> = query.iter().sort().collect();
    // Process sorted components
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `iter` module of the `bevy_ecs` library to iterate over query results in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.