# Cheat Sheet for ```github.com/stretchr/testify/require```

The require package provides a set of helpful assertions for unit tests that stop test execution immediately when a condition is not met (as opposed to assert, which allows the test to continue).


# Index

- [Cheat Sheet for `github.com/stretchr/testify/require`](#cheat-sheet-for-githubcomstretchrtestifyrequire)
- [Install](#install)
- [Importing](#importing)
- [Collection Assertions](#collection-assertions)
  - [Length Checks](#length-checks)
  - [Contains/Element Checks](#containselement-checks)
  - [Map/Subset Checks](#mapsubset-checks)
  - [File System](#file-system)
  - [JSON](#json)
  - [Panics](#panics)
  - [Type Assertions](#type-assertions)
- [Comparison](#comparison)
- [Custom Comparison](#custom-comparison)
- [Special Cases](#special-cases)
- [Notes](#notes)


# Install

```bush
go get github.com/stretchr/testify/
```

# Importing

```go
import (
    "testing"
    "github.com/stretchr/testify/require"
)
```


# Collection Assertions

## Length Checks

```go
require.Len(t, object, length, msgAndArgs...)
```

## Contains/Element Checks

```go
require.Contains(t, container, item, msgAndArgs...)
require.NotContains(t, container, item, msgAndArgs...)
require.ElementsMatch(t, expected, actual, msgAndArgs...) // Checks same elements, order doesn't matter
```

## Map/Subset Checks

```go
require.Subset(t, superset, subset, msgAndArgs...)
require.NotSubset(t, superset, subset, msgAndArgs...)
```

## File System


```go
require.FileExists(t, path, msgAndArgs...)
require.NoFileExists(t, path, msgAndArgs...)
require.DirExists(t, path, msgAndArgs...)
```

## JSON

```go
require.JSONEq(t, expected, actual, msgAndArgs...)
```

## Panics

```go
require.Panics(t, func(){...}, msgAndArgs...)
require.PanicsWithValue(t, expected, func(){...}, msgAndArgs...)
require.NotPanics(t, func(){...}, msgAndArgs...)
```

## Type Assertions


```go
require.IsType(t, expectedType, object, msgAndArgs...)
require.Implements(t, interfaceObject, object, msgAndArgs...)
```

# Comparison

```go
require.Greater(t, a, b, msgAndArgs...)
require.GreaterOrEqual(t, a, b, msgAndArgs...)
require.Less(t, a, b, msgAndArgs...)
require.LessOrEqual(t, a, b, msgAndArgs...)
```
# Custom Comparison

```go
require.Greater(t, a, b, msgAndArgs...)
require.GreaterOrEqual(t, a, b, msgAndArgs...)
require.Less(t, a, b, msgAndArgs...)
require.LessOrEqual(t, a, b, msgAndArgs...)
```

# Special Cases

```go
require.Eventually(t, conditionFunc, waitFor, tick, msgAndArgs...)    // Tests condition becomes true within waitFor
require.Never(t, conditionFunc, waitFor, tick, msgAndArgs...)          // Tests condition never becomes true
```

# Notes

- All require functions immediately fail the test when the assertion is not met
- Each function takes optional ```msgAndArgs...``` for custom error messages
- t is the ```*testing.T``` object from your test function
- Prefer ```require``` over ```assert``` when you want to stop test execution on failure