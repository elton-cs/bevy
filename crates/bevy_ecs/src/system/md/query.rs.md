# Bevy ECS Query Documentation

This documentation provides a detailed overview of the public functions, structs, and traits available in the Bevy ECS (Entity Component System) library, specifically focusing on the `Query` system. This information is intended to serve as a comprehensive guide for developers building applications or games using Bevy.

## Structs

### `Query<'world, 'state, D: QueryData, F: QueryFilter = ()>`
- **Description**: A system parameter that provides selective access to component data stored in a `World`.
- **Type Parameters**:
  - `D`: The type of data contained in the query item, which must implement the `QueryData` trait.
  - `F`: A set of conditions that determines whether query items should be kept or discarded, which must implement the `QueryFilter` trait (optional).
- **Usage**:
  - Enables access to entity identifiers and components without directly accessing the world.
  - Returns iterators and getter methods that yield query items.

## Public Functions

### `unsafe fn new(world: UnsafeWorldCell<'w>, state: &'s QueryState<D, F>, last_run: Tick, this_run: Tick) -> Self`
- **Description**: Creates a new query.
- **Panics**: If the world used to create `state` is not `world`.
- **Safety**: Must ensure unique mutable access to avoid memory safety violations.

### `pub fn to_readonly(&self) -> Query<'_, 's, D::ReadOnly, F>`
- **Description**: Returns another `Query` that fetches the read-only version of the query items.
- **Usage**: Useful for working around the borrow checker or reusing functionality between systems.

### `pub fn reborrow(&mut self) -> Query<'_, 's, D, F>`
- **Description**: Returns a new `Query` reborrowing the access from this one.
- **Usage**: Allows calling other methods or systems that require an owned `Query` without losing ownership.

### `pub fn iter(&self) -> QueryIter<'_, 's, D::ReadOnly, F>`
- **Description**: Returns an iterator over the read-only query items.
- **Guarantee**: Each matching entity is returned once and only once.

### `pub fn iter_mut(&mut self) -> QueryIter<'_, 's, D, F>`
- **Description**: Returns an iterator over the query items.
- **Guarantee**: Each matching entity is returned once and only once.

### `pub fn iter_combinations<const K: usize>(&self) -> QueryCombinationIter<'_, 's, D::ReadOnly, F, K>`
- **Description**: Returns an iterator over all combinations of `K` read-only query items without repetition.

### `pub fn iter_combinations_mut<const K: usize>(&mut self) -> QueryCombinationIter<'_, 's, D, F, K>`
- **Description**: Returns an iterator over all combinations of `K` query items without repetition.

### `pub fn iter_many<EntityList: IntoIterator<Item: Borrow<Entity>>>(&self, entities: EntityList) -> QueryManyIter<'_, 's, D::ReadOnly, F, EntityList::IntoIter>`
- **Description**: Returns an iterator over the read-only query items generated from an `Entity` list.

### `pub fn iter_many_mut<EntityList: IntoIterator<Item: Borrow<Entity>>>(&mut self, entities: EntityList) -> QueryManyIter<'_, 's, D, F, EntityList::IntoIter>`
- **Description**: Returns an iterator over the query items generated from an `Entity` list.

### `pub fn get(&self, entity: Entity) -> Result<ROQueryItem<'_, D>, QueryEntityError>`
- **Description**: Returns the read-only query item for the given `Entity`.
- **Guarantee**: Runs in `O(1)` time.

### `pub fn get_many<const N: usize>(&self, entities: [Entity; N]) -> Result<[ROQueryItem<'_, D>; N], QueryEntityError>`
- **Description**: Returns the read-only query items for the given array of `Entity`.
- **Panics**: If there is a query mismatch or a non-existing entity.

### `pub fn get_mut(&mut self, entity: Entity) -> Result<D::Item<'_>, QueryEntityError>`
- **Description**: Returns the query item for the given `Entity`.
- **Guarantee**: Runs in `O(1)` time.

### `pub fn get_many_mut<const N: usize>(&mut self, entities: [Entity; N]) -> Result<[D::Item<'_>; N], QueryEntityError>`
- **Description**: Returns the query items for the given array of `Entity`.

### `pub fn single(&self) -> ROQueryItem<'_, D>`
- **Description**: Returns a single read-only query item when there is exactly one entity matching the query.
- **Panics**: If the number of query items is not exactly one.

### `pub fn get_single(&self) -> Result<ROQueryItem<'_, D>, QuerySingleError>`
- **Description**: Returns a single read-only query item when there is exactly one entity matching the query.

### `pub fn single_mut(&mut self) -> D::Item<'_>`
- **Description**: Returns a single query item when there is exactly one entity matching the query.
- **Panics**: If the number of query items is not exactly one.

### `pub fn get_single_mut(&mut self) -> Result<D::Item<'_>, QuerySingleError>`
- **Description**: Returns a single query item when there is exactly one entity matching the query.

### `pub fn is_empty(&self) -> bool`
- **Description**: Returns `true` if there are no query items.

### `pub fn contains(&self, entity: Entity) -> bool`
- **Description**: Returns `true` if the given `Entity` matches the query.

### `pub fn transmute_lens<NewD: QueryData>(&mut self) -> QueryLens<'_, NewD>`
- **Description**: Returns a `QueryLens` that can be used to get a query with a more general fetch.

### `pub fn join<OtherD: QueryData, NewD: QueryData>(&mut self, other: &mut Query<OtherD>) -> QueryLens<'_, NewD>`
- **Description**: Returns a `QueryLens` that can be used to get a query with the combined fetch.

## Traits

### `IntoIterator`
- **Description**: Allows `Query` to be used in a `for` loop.
- **Implementations**:
  - For immutable references: Returns an iterator over read-only query items.
  - For mutable references: Returns an iterator over mutable query items.

## Conclusion
This documentation serves as a comprehensive guide to the `Query` system in Bevy ECS, providing developers with the necessary information to effectively utilize the library in their applications and games. Each function and struct is designed to facilitate efficient access and manipulation of component data within the ECS framework.
