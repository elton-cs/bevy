# Bevy ECS Input System Documentation

This documentation provides an overview of the public functions, structs, traits, and implementations available in the `input.rs` file of the Bevy ECS library. It serves as a guide for developers to effectively utilize these components when building a Bevy application or game.

## Traits

### `SystemInput`
- **Description**: A trait for types that can be used as input to [`System`]s.
- **Provided Implementations**:
  - `()`: Represents no input.
  - [`In<T>`]: For values.
  - [`InRef<T>`]: For read-only references to values.
  - [`InMut<T>`]: For mutable references to values.
  - [`Trigger<E, B>`]: For [`ObserverSystem`]s.
  - [`StaticSystemInput<I>`]: For arbitrary [`SystemInput`]s in generic contexts.
- **Associated Types**:
  - `Param<'i>`: The wrapper input type defined as the first argument to [`FunctionSystem`]s.
  - `Inner<'i>`: The inner input type passed to functions that run systems, such as [`System::run`].
- **Methods**:
  - `wrap(this: Self::Inner<'_>) -> Self::Param<'_>`: Converts a [`SystemInput::Inner`] into a [`SystemInput::Param`].

## Type Aliases

### `SystemIn<'a, S>`
- **Description**: A shorthand way to get the [`System::In`] for a [`System`] as a [`SystemInput::Inner`].

## Structs

### `In<T>`
- **Description**: A [`SystemInput`] type that denotes a [`System`] receives an input value of type `T` from its caller.
- **Usage**: Systems may take an optional input which they require to be passed to them when they are being [`run`](System::run). Only the first parameter of a function may be tagged as an input, meaning a system can only have one or zero input parameters.
- **Example**:
  ```rust
  fn square(In(input): In<usize>) -> usize {
      input * input
  }
  ```

### `InRef<'i, T: ?Sized>`
- **Description**: A [`SystemInput`] type that denotes a [`System`] receives a read-only reference to a value of type `T`.
- **Usage**: Similar to [`In`], but takes a reference to a value instead of the value itself.
- **Example**:
  ```rust
  fn log(InRef(msg): InRef<str>, mut log: ResMut<Log>) {
      writeln!(log.0, "{}", msg).unwrap();
  }
  ```

### `InMut<'a, T: ?Sized>`
- **Description**: A [`SystemInput`] type that denotes a [`System`] receives a mutable reference to a value of type `T`.
- **Usage**: Similar to [`In`], but takes a mutable reference to a value instead of the value itself.
- **Example**:
  ```rust
  fn square(InMut(input): InMut<usize>) {
      *input *= *input;
  }
  ```

### `StaticSystemInput<'a, I: SystemInput>`
- **Description**: A helper for using [`SystemInput`]s in generic contexts. This type is a [`SystemInput`] adapter which always has `Self::Param == Self`, regardless of the argument [`SystemInput`] (`I`).
- **Usage**: Useful for having arbitrary [`SystemInput`]s in function systems.

## Implementations

### Implementations for `SystemInput`
- **For `()`**: Represents no input.
- **For `In<T>`**: Wraps a value of type `T`.
- **For `InRef<'i, T>`**: Wraps a read-only reference to a value of type `T`.
- **For `InMut<'a, T>`**: Wraps a mutable reference to a value of type `T`.
- **For `Trigger<'_, E, B>`**: Used for [`ObserverSystem`]s.
- **For `StaticSystemInput<'a, I>`**: Adapts arbitrary [`SystemInput`]s.

This documentation serves as a comprehensive guide for developers looking to utilize the `input` module of the `bevy_ecs` library to customize system behavior in their applications or games. It provides insights into the available constructs and their usage, enabling effective development with Bevy.
