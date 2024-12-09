# Typed-throws
Swift introduced the ability to specify exactly what types of errors a function can throw, known as "typed throws". **typed throws** refers to the idea of specifying the exact type of error that a function can throw. However, Swift currently only supports **untyped error propagation**, meaning functions that throw errors can throw any type that conforms to the `Error` protocol. You cannot explicitly declare the type of error a function throws in its signature, as you might in other languages like Java.

## Example of Untyped `throws`

```
enum MyError: Error {
    case invalidInput
    case networkFailure
}

func fetchData() throws {
    throw MyError.networkFailure
}
```

## Why Typed Throws is Not in Swift

Typed throws would allow you to specify the exact error type in the function signature, such as:

```
func fetchData() throws(MyError) {
    throw MyError.networkFailure
}
```

However, Swift intentionally avoids typed throws for several reasons:
- **Error Handling Flexibility:** Typed throws can make error handling less flexible, especially in scenarios involving functions that call other functions with different error types.
- **Code Complexity:** Restricting errors to specific types might complicate composability and make APIs harder to evolve.
- **Emphasis on Error Protocol:** Swift encourages developers to work with the Error protocol to enable polymorphic error handling.

## Workarounds for Typed Error Behavior

While Swift does not support typed throws directly, you can achieve similar behavior with patterns such as:

#### 1. Restricting Errors with Generics
You can use generics to simulate typed throws:

```
func performAction<E: Error>(_ action: () throws -> Void) throws(E) {
    try action()
}
```
#### 2. Encapsulation

You can wrap errors into a custom error type:

```
enum TypedError<E: Error>: Error {
    case wrapped(E)
}

func performAction() throws -> Result<String, MyError> {
    // Return a specific result type with error
    return .failure(MyError.invalidInput)
}
```

## Conclusion

While typed throws is a frequently discussed topic in the Swift community, the current design favors flexibility and simplicity over static type constraints for error handling. If you need more control over error types, consider using strategies like enums or results for encapsulating and managing errors explicitly.
