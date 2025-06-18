# Cheat Sheet for ```github.com/stretchr/testify/assert```

## Index

- [Basic Assertions](#basic-assertions)
- [Numeric Comparisons](#numeric-comparisons)
- [Collection Assertions](#collection-assertions)
- [Error Handling](#error-handling)
- [Type Assertions](#type-assertions)
- [String Assertions](#string-assertions)
- [File System](#file-system)
- [Advanced Assertions](#advanced-assertions)
- [Custom Messages](#custom-messages)
- [HTTP Testing](#http-testing)
- [Notes](#notes)


---

# Basic Assertions

```go
assert.Equal(t, expected, actual)       // Equality
assert.NotEqual(t, expected, actual)     // Inequality
assert.True(t, condition)                // Boolean true
assert.False(t, condition)               // Boolean false
assert.Nil(t, object)                    // Nil value
assert.NotNil(t, object)                 // Not nil
```

# Numeric Comparisons

```go
assert.Zero(t, value)                   // Value is zero
assert.NotZero(t, value)                // Value is not zero
assert.Greater(t, a, b)                 // a > b
assert.GreaterOrEqual(t, a, b)          // a >= b
assert.Less(t, a, b)                    // a < b
assert.LessOrEqual(t, a, b)             // a <= b              // Not nil
```

# Collection Assertions

```go
assert.Contains(t, collection, element)       // Collection contains element
assert.NotContains(t, collection, element)
assert.Len(t, collection, length)              // Collection has expected length
assert.Empty(t, collection)                    // Collection is empty
assert.NotEmpty(t, collection)                 // Collection is not empty
```

# Error Handling

```go
assert.Error(t, err)                    // Error is not nil
assert.NoError(t, err)                  // Error is nil
assert.ErrorIs(t, err, target)          // Error wraps target
assert.ErrorAs(t, err, target)          // Error is of type target
```

# Type Assertions

```go
assert.IsType(t, expectedType, object)  // Exact type match
assert.Implements(t, interfaceObj, object) // Implements interface
```

# String Assertions

```go
assert.Regexp(t, regex, str)               // String matches regex
assert.NotRegexp(t, regex, str)
assert.JSONEq(t, expectedJSON, actualJSON)  // JSON equality
```

# File System

```go
assert.FileExists(t, path)              // File exists
assert.DirExists(t, path)               // Directory exists
assert.NoFileExists(t, path)
assert.NoDirExists(t, path)
```

# Advanced Assertions

```go
assert.WithinDuration(t, expected, actual, delta)     // Time within delta
assert.Same(t, ptr1, ptr2)                            // Same pointer/reference
assert.NotSame(t, ptr1, ptr2)
assert.Panics(t, func())                               // Function panics
assert.NotPanics(t, func())
assert.Eventually(t, conditionFunc, wait, tick)        // Condition eventually true
assert.Never(t, conditionFunc, wait, tick)             // Condition never true
```

# Custom Messages

```go
assert.Equal(t, expected, actual, "message")                                // Add custom failure message
assert.Equal(t, expected, actual, "expected %v, got %v", expected, actual)
```

# HTTP Testing

```go
assert.Equal(t, expected, actual, "message")                                     // Add custom failure message
assert.Equal(t, expected, actual, "expected %v, got %v", expected, actual)
```

# Notes

- Most assertions accept optional message arguments (formatted with `fmt.Sprintf`)
- Use the `require` package instead of `assert` for immediate test termination on failure
- Many assertions have inverse versions (e.g., `Equal` / `NotEqual`)
- For floating point comparisons, use `assert.InDelta(t, expected, actual, delta)` for approximate equality