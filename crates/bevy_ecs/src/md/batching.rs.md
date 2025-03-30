# Bevy Batching Documentation

This document provides a comprehensive overview of the public API available in the `batching` module of the `bevy_ecs` library. It includes details on structs and functions that can be utilized to control batching behavior during parallel processing in a Bevy application or game.

## Structs

### `BatchingStrategy`
- **Description**: Dictates how a parallel operation chunks up large quantities during iteration.
- **Fields**:
  - `batch_size_limits`: A range that defines the upper and lower limits for a batch of entities.
    - **Usage**: Setting the bounds to the same value will result in a fixed batch size. Defaults to `[1, usize::MAX]`.
  - `batches_per_thread`: The number of batches per thread in the `ComputeTaskPool`.
    - **Usage**: Increasing this value will decrease the batch size, which may increase the scheduling overhead for the iteration. Defaults to `1`.
- **Methods**:
  - `new() -> Self`: Creates a new unconstrained default batching strategy.
    - **Usage**: Call this method to create a default batching strategy.
  - `fixed(batch_size: usize) -> Self`: Declares a batching strategy with a fixed batch size.
    - **Usage**: Use this method to create a batching strategy that enforces a specific batch size.
  - `min_batch_size(mut self, batch_size: usize) -> Self`: Configures the minimum allowed batch size of this instance.
    - **Usage**: Call this method to set a minimum batch size.
  - `max_batch_size(mut self, batch_size: usize) -> Self`: Configures the maximum allowed batch size of this instance.
    - **Usage**: Call this method to set a maximum batch size.
  - `batches_per_thread(mut self, batches_per_thread: usize) -> Self`: Configures the number of batches to assign to each thread for this instance.
    - **Usage**: Call this method to set the number of batches per thread.
  - `calc_batch_size(&self, max_items: impl FnOnce() -> usize, thread_count: usize) -> usize`: Calculates the batch size according to the given thread count and max item count.
    - **Parameters**:
      - `max_items`: A closure that returns the maximum number of items.
      - `thread_count`: The number of threads to use for processing.
    - **Returns**: The calculated batch size.
    - **Usage**: Call this method to determine the optimal batch size for processing based on the current workload and thread count.

## Example Usage

### Creating a Batching Strategy
```rust
use bevy_ecs::batching::BatchingStrategy;

fn main() {
    let batching_strategy = BatchingStrategy::new()
        .min_batch_size(10)
        .max_batch_size(100)
        .batches_per_thread(2);
}
```

### Using a Fixed Batching Strategy
```rust
use bevy_ecs::batching::BatchingStrategy;

fn main() {
    let fixed_batching_strategy = BatchingStrategy::fixed(50);
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `batching` module of the `bevy_ecs` library to manage batching behavior in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.