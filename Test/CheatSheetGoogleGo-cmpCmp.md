# Cheat Sheet for ```github.com/google/go-cmp/cmp```


# Index

1. [Basic Usage](#basic-usage)
2. [Comparison Options](#comparison-options)
   - [Ignoring Fields](#ignoring-fields)
   - [Comparing Approximate Values](#comparing-approximate-values)
   - [Transforming Values](#transforming-values)
   - [Custom Comparers](#custom-comparers)
3. [Common Options](#common-options)
4. [Helper Functions](#helper-functions)
5. [Tips](#tips)


# Basic Usage
```go
import "github.com/google/go-cmp/cmp"

// Basic comparison
if diff := cmp.Diff(x, y); diff != "" {
    t.Errorf("Mismatch (-want +got):\n%s", diff)
}
```

# Comparison Options

## Ignoring Fields

```go
// Ignore specific fields
opt := cmp.FilterPath(func(p cmp.Path) bool {
    return p.Last().String() == ".CreatedAt"
}, cmp.Ignore())

cmp.Diff(x, y, opt)

// Ignore all unexported fields
opt := cmp.AllowUnexported(MyType{})
```

## Comparing Approximate Values

```go
// Compare floats with tolerance
opt := cmp.Comparer(func(x, y float64) bool {
    delta := math.Abs(x - y)
    mean := math.Abs(x + y) / 2.0
    return delta/mean < 0.00001
})
```

## Transforming Values
```go
// Normalize values before comparison
opt := cmp.Transformer("Normalize", func(in string) string {
    return strings.ToLower(strings.TrimSpace(in))
})
```


## Custom Comparers

```go
// Define custom comparison for specific types
opt := cmp.Comparer(func(x, y MyType) bool {
    return x.ID == y.ID // Only compare ID field
})
```
# Common Options

```go
// Combine multiple options
opts := cmp.Options{
    cmp.AllowUnexported(Person{}),
    cmp.FilterPath(func(p cmp.Path) bool {
        return p.Last().String() == ".privateField"
    }, cmp.Ignore()),
    cmp.Comparer(func(x, y float64) bool {
        return math.Abs(x-y) < 0.0001
    }),
}

cmp.Diff(a, b, opts)
```
# Helper Functions
```go
// Equality check (returns bool)
cmp.Equal(x, y, opts)

// Difference string (returns empty string if equal)
cmp.Diff(x, y, opts)
```

## Tips

- Use `cmp.Equal()` for simple boolean checks
- Use `cmp.Diff()` for detailed difference output in tests  
- Combine multiple options with `cmp.Options{}`
- For complex types, consider writing custom comparers
- Use `cmp.FilterPath` for precise control over which fields to ignore