# Go Test Framework Cheat Sheet

## Index
- [Commands and Recommended Folder Structures](#commands-and-recommended-folder-structures)
- [Basic Test Structure](#basic-test-structure)
- [Running Tests](#running-tests)
- [Test Functions](#test-functions)
- [Table-Driven Tests](#table-driven-tests)
- [Test Helpers](#test-helpers)
  - [Setup/Teardown](#setupteardown)
  - [Helper Functions](#helper-functions)
- [Subtests](#subtests)
- [Benchmark Tests](#benchmark-tests)
- [Mocking/Stubbing](#mockingstubbing)
- [Test Utilities](#test-utilities)
- [HTTP Testing](#http-testing)
- [Common Packages for Testing](#common-packages-for-testing)
- [Golden Files](#golden-files)
- [Parallel Tests](#parallel-tests)
- [Skip Tests](#skip-tests)
- [Test Flags](#test-flags)
- [Useful Assertion Libraries](#useful-assertion-libraries)

---

## Commands and Recommended Folder Structures

([LINK](./CommandsAndRecommendedFolderStructuresForTesting.md))


## Basic Test Structure

```go
package mypackage

import "testing"

func TestSomething(t *testing.T) {
    // Test implementation
}
```

# Running Tests

```bash
go test                 # Run all tests in current directory
go test -v              # Verbose mode
go test -run TestName   # Run specific test
go test -cover          # Show coverage
go test -coverprofile=coverage.out && go tool cover -html=coverage.out  # HTML coverage report
```

# Test Functions
## Basic Assertions

```go
if got != want {
    t.Errorf("got %v, want %v", got, want)
}

if err != nil {
    t.Fatalf("unexpected error: %v", err)
}
```

# Table-Driven Tests

```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name string
        a, b int
        want int
    }{
        {"positive", 2, 3, 5},
        {"negative", -1, -1, -2},
        {"zero", 0, 0, 0},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            if got := Add(tt.a, tt.b); got != tt.want {
                t.Errorf("Add() = %v, want %v", got, tt.want)
            }
        })
    }
}
```

# Test Helpers
## Setup/Teardown
```go

func setup() {
    // Initialize test resources
}

func teardown() {
    // Clean up
}

func TestMain(m *testing.M) {
    setup()
    code := m.Run()
    teardown()
    os.Exit(code)
}

```

# Helper Functions

```go
func assertEqual(t *testing.T, got, want interface{}) {
    t.Helper()
    if got != want {
        t.Errorf("got %v, want %v", got, want)
    }
}

// Usage:
assertEqual(t, result, expected)
```

# Subtests

```go
func TestSomething(t *testing.T) {
    t.Run("case1", func(t *testing.T) {
        // Test case 1
    })
    
    t.Run("case2", func(t *testing.T) {
        // Test case 2
    })
}
```

# Benchmark Tests

```go
func BenchmarkSomething(b *testing.B) {
    for i := 0; i < b.N; i++ {
        // Code to benchmark
    }
}

// Run benchmarks:
// go test -bench=.

```

# Mocking/Stubbing
## Interface-based Mocking

```go
type DB interface {
    Get(key string) (string, error)
}

func TestService(t *testing.T) {
    mockDB := struct {
        GetFunc func(key string) (string, error)
    }{
        GetFunc: func(key string) (string, error) {
            return "mocked", nil
        },
    }

    result, err := Service(mockDB)
    // assertions...
}
```

# Test Utilities
## Temporary Files/Dirs

```go
func TestWithTempFile(t *testing.T) {
    tmpfile, err := os.CreateTemp("", "example")
    if err != nil {
        t.Fatal(err)
    }
    defer os.Remove(tmpfile.Name())
    
    // Use tmpfile...
}
```

# HTTP Testing

```go
func TestHTTPHandler(t *testing.T) {
    req := httptest.NewRequest("GET", "/test", nil)
    w := httptest.NewRecorder()

    handler(w, req)

    resp := w.Result()
    if resp.StatusCode != http.StatusOK {
        t.Errorf("expected status OK, got %v", resp.Status)
    }
}
```

# Common Packages for Testing

```go
import (
    "testing"
    "net/http/httptest"
    "os"
    "io/ioutil"
    "bytes"
    "reflect"
)
```

# Golden Files

```go
func TestGolden(t *testing.T) {
    got := doSomething()
    
    golden := filepath.Join("testdata", t.Name()+".golden")
    if *update {
        ioutil.WriteFile(golden, []byte(got), 0644)
    }
    
    want, _ := ioutil.ReadFile(golden)
    if got != string(want) {
        t.Errorf("got %q, want %q", got, want)
    }
}
```

# Parallel Tests

```go
func TestParallel(t *testing.T) {
    t.Parallel()
    // Test code...
}
```

# Skip Tests

```go
func TestMaybeSkip(t *testing.T) {
    if testing.Short() {
        t.Skip("skipping test in short mode")
    }
    // Long test...
}
```

# Test Flags

```go
var update = flag.Bool("update", false, "update golden files")

func TestMain(m *testing.M) {
    flag.Parse()
    os.Exit(m.Run())
}
```

Here's the correctly formatted Markdown for your list of useful Go assertion libraries:

---

# Useful Assertion Libraries

1.  `github.com/stretchr/testify/assert` ([GoDoc](https://pkg.go.dev/github.com/stretchr/testify/assert)) ([Cheat Sheet](./CheatSheetforStretchrTestifyAssert.md))
2.  `github.com/stretchr/testify/require` ([GoDoc](https://pkg.go.dev/github.com/stretchr/testify/require)) ([Cheat Sheet](./CheatSheetforStretchrTestifyRequire.md))
3.  `github.com/google/go-cmp/cmp` ([GoDoc](https://pkg.go.dev/github.com/google/go-cmp/cmp)) ([Cheat Sheet](./CheatSheetGoogleGo-cmpCmp.md))