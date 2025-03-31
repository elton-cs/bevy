# Bevy Query Parallel Iteration Module Documentation

This document provides a comprehensive overview of the public API available in the `par_iter` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to perform parallel iterations over query results in a Bevy application or game.

## Structs

### `QueryParIter<'w, 's, D: QueryData, F: QueryFilter>`
- **Description**: A parallel iterator over query results of a `Query`.
- **Fields**:
  - `world`: A mutable reference to the `World` instance.
  - `state`: A reference to the `QueryState` for the query.
  - `last_run`: A tick representing the last run of the query.
  - `this_run`: A tick representing the current run of the query.
  - `batching_strategy`: A strategy for batching the query results during iteration.
- **Key Points**:
  - This struct is created by the `Query::par_iter` and `Query::par_iter_mut` methods, allowing for parallel iteration over query results.

## Implementations

### `QueryParIter` Methods
- **`batching_strategy(mut self, strategy: BatchingStrategy) -> Self`**:
  - **Description**: Changes the batching strategy used when iterating.
  - **Key Points**: This allows customization of how results are batched during parallel iteration.

- **`for_each<FN: Fn(QueryItem<'w, D>) + Send + Sync + Clone>(self, func: FN)`**:
  - **Description**: Runs a function on each query result in parallel.
  - **Panics**: If the `ComputeTaskPool` is not initialized.

- **`for_each_init<FN, INIT, T>(self, init: INIT, func: FN)`**:
  - **Description**: Runs a function on each query result in parallel, using a value returned by an initialization function.
  - **Key Points**: This method allows for initialization of a local state for each thread.

## Example Usage

### Using QueryParIter
```rust
use bevy_ecs::prelude::*;
use bevy_tasks::ComputeTaskPool;

#[derive(Component)]
struct MyComponent;

fn my_parallel_system(query: Query<&MyComponent>) {
    query.par_iter().for_each(|component| {
        // Process each component in parallel
    });
}
```

### Custom Batching Strategy
```rust
fn my_custom_batching_system(query: Query<&MyComponent>) {
    let mut iter = query.par_iter();
    iter.batching_strategy(BatchingStrategy::default());
    iter.for_each(|component| {
        // Process each component with the custom batching strategy
    });
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `par_iter` module of the `bevy_ecs` library to perform parallel iterations over query results in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.