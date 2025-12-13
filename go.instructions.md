---
description: 'Golang Best Practices, Idioms, and System Programming.'
applyTo: '**.go, **.mod, **.sum'
---

# Golang Development Guidelines & Standards

## Your Mission
Write idiomatic, efficient, and robust Go code. You prioritize explicit error handling, safe concurrency patterns, and the "Go Way" of doing things over cleverness.

## Introduction
Go prefers simplicity and readability. These instructions enforce strict adherence to *Effective Go*, robust error handling, and safe system interaction patterns, especially for cross-platform compatibility.

## Golang Fundamentals & Architecture

### Idiomatic Go
- **Format:** `gofmt` is not a suggestion; it is the law.
- **Style:** Follow *Effective Go*. Code should be boring and predictable.
- **Simplicity:** Prefer simple, synchronous code over complex concurrency unless necessary.

### Concurrency
- **Channels & Goroutines:** Use them safely. Always ensure goroutines have a way to exit (prevent leaks).
- **Sync:** Use `sync.Mutex` for shared state protection where channels are overkill.
- **Context:** Always propagate `context.Context` for cancellation and timeouts.

## System & Path Safety

### Error Handling
- **Explicit is Better:** Handle errors explicitly (`if err != nil`).
- **No Swallowing:** ❌ Never use `_` to ignore errors unless you are 100% sure it's safe.
- **Wrapping:** Use `fmt.Errorf("...: %w", err)` to wrap errors with context.

### Path Safety
- **Cross-Platform:** Use `filepath.Join` for all path construction. It handles standardizing separators (`\` vs `/`).

**Example - Safe File Writer:**
```go
import (
    "path/filepath"
    "os"
)

func SaveDataSafe(dir string, filename string, data []byte) error {
    // 🛡️ Handles Windows backslashes automatically
    fullPath := filepath.Join(dir, filename) 
    return os.WriteFile(fullPath, data, 0644)
}
```

## Testing & Quality Assurance

### Framework
- Use the built-in `testing` package.

### Table-Driven Tests
- **Mandatory:** Use Table-Driven Tests for almost everything. It is the idiomatic way to test multiple scenarios cleanly.
- **Subtests:** Use `t.Run()` for clear reporting.

**Example - Table-Driven Test:**
```go
func TestAdd(t *testing.T) {
    tests := []struct {
        name string
        a, b int
        want int
    }{
        {"positive", 1, 2, 3},
        {"negative", -1, -1, -2},
        {"mixed", -1, 1, 0},
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

## Anti-patterns
- **Panic:** Do not use `panic()` in libraries or normal logic. Return errors instead.
- **Global State:** Avoid function-scoped global variables (`var`).
- **Interface Pollution:** Do not define interfaces on the *implementer* side; define them on the *consumer* side.
