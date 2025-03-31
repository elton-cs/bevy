# Bevy ECS Table Module Documentation

This document provides a comprehensive overview of the public API available in the `storage/table` module of the `bevy_ecs` library. It includes details on structs, enums, and their usage that can be utilized to manage columnar data storage in a Bevy application or game.

## Overview

- **Purpose**: This module provides a `Table` type that is used to store component data in a columnar format. It is designed for efficient iteration and management of component data associated with entities in an Entity-Component-System (ECS) architecture.

## Structs

### `TableId`
- **Description**: An opaque unique ID for a `Table` within a `World`.
- **Fields**:
  - `0`: A `u32` representing the table index.
- **Usage**: Used to uniquely identify tables within a world, allowing for efficient access and management.

### `TableRow`
- **Description**: A newtype for rows in `Table`s, specifying a single row in a specific table.
- **Fields**:
  - `0`: A `u32` representing the row index.
- **Usage**: Used to reference specific rows within a table, allowing for efficient access to component data.

### `TableBuilder`
- **Description**: A builder type for constructing `Table`s.
- **Fields**:
  - `columns`: A sparse set of columns for the table.
  - `capacity`: The initial capacity of the table.
- **Usage**: Used to construct tables by adding columns and finalizing the table creation.

### `Table`
- **Description**: A column-oriented structure for storing components of entities in a `World`.
- **Fields**:
  - `columns`: An immutable sparse set of columns.
  - `entities`: A vector of entities associated with the table.
- **Usage**: Used to manage and access component data for entities in a structured manner.

## Functions

### `TableId::from_u32`
- **Description**: Creates a new `TableId` from a `u32` index.
- **Parameters**:
  - `index`: The index to create the `TableId` from.
- **Returns**: A new `TableId`.
- **Usage**: Call this method to create a `TableId` from an existing index.

### `TableId::from_usize`
- **Description**: Creates a new `TableId` from a `usize` index.
- **Parameters**:
  - `index`: The index to create the `TableId` from.
- **Returns**: A new `TableId`.
- **Usage**: Call this method to create a `TableId` from a `usize` index.

### `TableId::as_u32`
- **Description**: Gets the underlying table index from the `TableId`.
- **Returns**: The underlying `u32` index.
- **Usage**: Use this method to retrieve the index associated with a `TableId`.

### `TableRow::from_u32`
- **Description**: Creates a `TableRow` from a `u32` index.
- **Parameters**:
  - `index`: The index to create the `TableRow` from.
- **Returns**: A new `TableRow`.
- **Usage**: Call this method to create a `TableRow` from an existing index.

### `TableRow::from_usize`
- **Description**: Creates a `TableRow` from a `usize` index.
- **Parameters**:
  - `index`: The index to create the `TableRow` from.
- **Returns**: A new `TableRow`.
- **Usage**: Call this method to create a `TableRow` from a `usize` index.

### `TableRow::as_usize`
- **Description**: Gets the index of the row as a `usize`.
- **Returns**: The underlying `usize` index.
- **Usage**: Use this method to retrieve the index associated with a `TableRow`.

### `TableBuilder::with_capacity`
- **Description**: Starts building a new `Table` with a specified column capacity and initial capacity.
- **Parameters**:
  - `capacity`: The initial capacity for the table.
  - `column_capacity`: The capacity for each column.
- **Returns**: A new `TableBuilder`.
- **Usage**: Call this method to initialize a builder for constructing a new table.

### `TableBuilder::add_column`
- **Description**: Adds a new column to the `Table`.
- **Parameters**:
  - `component_info`: Information about the component type to be stored in the column.
- **Returns**: The updated `TableBuilder`.
- **Usage**: Call this method to add columns for components to the table being constructed.

### `TableBuilder::build`
- **Description**: Builds the `Table`, finalizing the construction.
- **Returns**: A new `Table`.
- **Usage**: Call this method to create the table after adding all desired columns.

### `Table::entities`
- **Description**: Fetches a read-only slice of the entities stored within the `Table`.
- **Returns**: A slice of entities.
- **Usage**: Use this method to access the entities associated with the table.

### `Table::capacity`
- **Description**: Gets the capacity of the table in entities.
- **Returns**: The capacity of the table.
- **Usage**: Use this method to check how many entities the table can currently store.

### `Table::len`
- **Description**: Gets the number of entities currently being stored in the table.
- **Returns**: The number of entities.
- **Usage**: Use this method to check how many entities are currently in the table.

### `Table::is_empty`
- **Description**: Checks if the `Table` is empty.
- **Returns**: A boolean indicating if the table contains no entities.
- **Usage**: Use this method to determine if the table has any stored entities.

### `Table::swap_remove_unchecked`
- **Description**: Removes the entity at the given row and returns the entity swapped in to replace it.
- **Parameters**:
  - `row`: The row from which to remove the entity.
- **Returns**: An optional entity that was swapped in.
- **Usage**: Use this method to efficiently remove an entity while maintaining the integrity of the table.

### `Table::move_to_and_forget_missing_unchecked`
- **Description**: Moves component data out of the `Table` for shared columns.
- **Parameters**:
  - `row`: The row to move.
  - `new_table`: The new table to move the data to.
- **Returns**: A result containing the new row index and the swapped entity.
- **Usage**: Use this method to transfer data between tables while managing component ownership.

## Example Usage

### Creating and Using a Table
```rust
use bevy_ecs::prelude::*;
use bevy_ecs::storage::TableBuilder;

fn main() {
    let mut world = World::new();
    let mut table_builder = TableBuilder::with_capacity(10, 5);

    // Add columns to the table
    table_builder.add_column(&ComponentInfo::new(ComponentId::new(1), /* other details */));

    // Build the table
    let table = table_builder.build();

    // Use the table...
}
```

This documentation serves as a comprehensive guide for developers looking to utilize the `storage/table` module of the `bevy_ecs` library to manage columnar data storage in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.