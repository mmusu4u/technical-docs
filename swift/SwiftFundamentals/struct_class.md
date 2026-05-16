# Swift Struct vs Class
## Staff / Principal iOS Architect Deep Dive
### Runtime • Compiler • Memory • Performance • Concurrency • SwiftUI

---

# 1. Introduction

## First-Sight Mental Model

| Struct | Class |
|---|---|
| Represents DATA | Represents IDENTITY |
| Value Semantics | Reference Semantics |
| Safer | Flexible |
| Faster | Dynamic |
| Better for Concurrency | Better for Shared Lifecycle |
| SwiftUI Preferred | UIKit/Common Services |

---

## Golden Rule

```text
Use Struct by default.

Move to Class only when:
- identity matters
- shared mutable state is required
- lifecycle management is needed
- Objective-C interoperability is needed
- polymorphism is essential
```

---

## Real Architectural Difference

```text
Structs optimize SYSTEMS.
Classes optimize RELATIONSHIPS.
```

---

# 2. Fundamental Difference

## Struct = Value Type

### Struct Example

```swift
struct User {
    var name: String
}
```

### Assignment Behavior

```swift
var a = User(name: "Musthafa")
var b = a

b.name = "Alex"
```

### Result

```text
a.name = Musthafa
b.name = Alex
```

### Key Characteristics

- Independent ownership
- Local mutation
- Predictable behavior
- No shared state

---

## Class = Reference Type

### Class Example

```swift
class User {
    var name: String

    init(name: String) {
        self.name = name
    }
}
```

### Assignment Behavior

```swift
let a = User(name: "Musthafa")
let b = a

b.name = "Alex"
```

### Result

```text
a.name = Alex
b.name = Alex
```

### Key Characteristics

- Shared ownership
- Shared mutation
- Reference semantics
- Global side effects possible

---

# 3. Visual Memory Comparison

## Struct Memory Visualization

### Stack Memory Layout

```text
Stack
-------------------------
a -> [Musthafa]

b -> [Alex]
```

### Explanation

Each struct variable stores its own independent value.

No shared memory.

---

## Class Memory Visualization

### Heap Memory Layout

```text
Stack
-------------------------
a -----\
         \
          ---> Heap Object [Alex]
         /
b -----/
```

### Explanation

Both variables point to SAME heap object.

Mutation affects all references.

---

# 4. Core Fundamentals Comparison

## Fundamental Comparison Table

| Area | Struct | Class | Why It Matters |
|---|---|---|---|
| Semantic Type | Value Type | Reference Type | Defines ownership behavior |
| Storage | Stack optimized | Heap allocated | Impacts performance |
| Assignment | Copies values | Copies references | Mutation behavior |
| ARC | No ARC | ARC managed | Runtime overhead |
| Identity | No identity | Has identity | Lifecycle management |
| Dispatch | Static dispatch | Dynamic dispatch | Method lookup performance |
| Thread Safety | Better | Riskier | Concurrency impact |
| Mutation | Local mutation | Shared mutation | Predictability |
| Performance | Faster | More overhead | System scalability |
| SwiftUI | Preferred | Less preferred | Rendering optimization |

---

## Architectural Insight

```text
Structs prioritize predictability and performance.

Classes prioritize flexibility and shared lifecycle.
```

---

# 5. Internal Working

# Struct Internal Working

## Struct Memory Layout

```text
Stack Frame
-------------------------
x = 10
y = 20
```

---

## Internal Characteristics

| Internal Area | Struct Behavior | Why Important |
|---|---|---|
| Storage | Inline storage | Better cache locality |
| Allocation | Stack allocation mostly | Extremely fast |
| Access | Direct memory access | No pointer chasing |
| ARC | No ARC tracking | Lower CPU overhead |
| Metadata | Minimal metadata | Smaller runtime cost |
| Indirection | None usually | Faster access |
| Ownership | Independent | Safer concurrency |
| Mutation | Local | Predictable behavior |

---

## Why Structs Are Fast

| Optimization | Benefit | Real-World Impact |
|---|---|---|
| Stack Allocation | Faster allocation | Better scrolling performance |
| Direct Access | No pointer dereference | Faster execution |
| Static Dispatch | Faster method calls | Better runtime efficiency |
| No ARC | No retain/release traffic | Lower CPU usage |
| Better Cache Locality | Contiguous memory | Better battery efficiency |

---

## Struct Example

```swift
struct Point {
    var x: Int
    var y: Int
}
```

### Compiler Optimization

Compiler may:
- store on stack
- inline values
- place values in CPU registers

---

# Class Internal Working

## Class Memory Layout

```text
Stack
-------------------------
Pointer
   |
   v

Heap Object
-------------------------
isa pointer
reference count
stored properties
```

---

## Internal Characteristics

| Internal Area | Class Behavior | Why Important |
|---|---|---|
| Storage | Heap object | Dynamic lifetime |
| Allocation | Heap allocation | More expensive |
| Access | Pointer dereference | Extra memory access |
| ARC | ARC tracking required | Runtime overhead |
| Metadata | Extensive runtime metadata | Higher memory usage |
| Indirection | Always present | Slower access |
| Ownership | Shared | Shared mutable state |
| Mutation | Global/shared | Side effects possible |

---

## Hidden Runtime Costs

| Hidden Cost | Why Expensive | Real-World Effect |
|---|---|---|
| Heap Allocation | Memory manager interaction | Slower object creation |
| ARC Traffic | retain/release calls | CPU spikes |
| Pointer Indirection | Extra memory lookup | Cache inefficiency |
| Dynamic Dispatch | Runtime method lookup | Slower execution |
| Synchronization | Shared state protection | Thread contention |

---

## Class Example

```swift
class UserManager {
    var users: [String] = []
}
```

### Architectural Meaning

Shared services require:
- shared identity
- shared lifecycle
- shared mutable state

---

# 6. Runtime Behavior

# Struct Runtime Behavior

## Struct Runtime Example

```swift
struct Counter {
    var value = 0
}

var a = Counter()
var b = a

b.value += 1
```

---

## Result

```text
a.value = 0
b.value = 1
```

---

## Runtime Characteristics

| Runtime Area | Struct Behavior | Architectural Benefit |
|---|---|---|
| Semantic Model | Value semantics | Safer ownership |
| Mutation Scope | Local only | Predictable updates |
| Side Effects | Minimal | Easier debugging |
| Sharing | No implicit sharing | Reduced coupling |
| Thread Safety | Better | Safer concurrency |
| Isolation | Natural | Independent state |
| Synchronization | Rarely required | Simpler threading |

---

## Runtime Advantages

- Reduced race conditions
- Better concurrency scalability
- Easier debugging
- Safer ownership model

---

# Class Runtime Behavior

## Class Runtime Example

```swift
class Counter {
    var value = 0
}

let a = Counter()
let b = a

b.value += 1
```

---

## Result

```text
a.value = 1
b.value = 1
```

---

## Runtime Characteristics

| Runtime Area | Class Behavior | Architectural Risk |
|---|---|---|
| Semantic Model | Reference semantics | Shared ownership |
| Mutation Scope | Shared/global | Hidden side effects |
| Side Effects | Common | Debugging difficulty |
| Sharing | Shared object graph | Coupling increases |
| Thread Safety | Riskier | Race conditions |
| Isolation | Manual | More complexity |
| Synchronization | Frequently needed | Locking overhead |

---

## Runtime Risks

- Race conditions
- Hidden mutations
- Synchronization complexity
- Shared state bugs

---

# 7. Compiler Behavior

# Struct Compiler Optimization

## Why Compiler Loves Structs

Structs provide:
- predictable ownership
- deterministic mutation
- simpler alias analysis

---

## Compiler Optimizations

| Optimization | What Compiler Does | Why Important |
|---|---|---|
| Stack Allocation | Places values on stack | Faster memory |
| Register Promotion | Uses CPU registers | Maximum performance |
| Copy Elision | Removes unnecessary copies | Better efficiency |
| Escape Analysis | Avoids heap allocation | Lower memory usage |
| Inlining | Inserts function body directly | Faster execution |
| Static Dispatch | Resolves methods at compile time | No runtime lookup |

---

## Static Dispatch Example

```swift
struct Calculator {
    func add() {}
}
```

### Dispatch Flow

```text
Compiler
   ↓
Direct Function Call
```

---

# Class Compiler Behavior

## Why Compiler Must Be Conservative

Classes introduce:
- shared references
- mutation uncertainty
- aliasing complexity

---

## Dynamic Dispatch Example

```swift
class Calculator {
    func add() {}
}
```

### Dispatch Flow

```text
Object
   ↓
Metadata
   ↓
Vtable
   ↓
Method Lookup
```

---

## Compiler Limitations

| Limitation | Why |
|---|---|
| Reduced Inlining | Runtime dispatch uncertainty |
| More ARC | Shared ownership |
| Less Predictability | Mutation can happen anywhere |
| Reduced Optimization | Alias analysis harder |

---

# 8. ARC Internals

# Struct ARC Behavior

## ARC Characteristics

```text
No retain/release calls.
```

### Benefits

- Lower CPU usage
- Reduced runtime overhead
- Better performance

---

# Class ARC Behavior

## ARC Workflow

```swift
class User {}

var a: User? = User()
a = nil
```

---

## Runtime Operations

```text
retain object
release object
deallocate object
```

---

## ARC Costs

| ARC Operation | Runtime Cost |
|---|---|
| retain | CPU overhead |
| release | CPU overhead |
| autorelease | Memory overhead |
| reference tracking | Runtime bookkeeping |

---

## ARC Storm

### Meaning

Too many retain/release calls causing:
- frame drops
- UI lag
- battery drain

### Common Cause

```text
Deeply nested class-heavy architectures
```

---

# 9. Copy-On-Write (COW)

## Why COW Exists

Large value copying is expensive.

Swift solves this using:
- shared storage
- delayed copying

---

## COW Example

```swift
var a = [1,2,3]
var b = a
```

---

## Before Mutation

```text
a ----\
        ---> Shared Buffer
b ----/
```

---

## Mutation

```swift
b.append(4)
```

---

## After Mutation

```text
a ---> Buffer A
b ---> Buffer B
```

---

## Internal Workflow

Swift checks:

```swift
isKnownUniquelyReferenced()
```

If shared:
- allocate new buffer
- copy values

---

## Benefits

| Benefit | Why Important |
|---|---|
| Value Semantics | Safer architecture |
| Shared Storage | Better memory efficiency |
| Delayed Copy | Better performance |

---

# 10. Stack vs Heap

## Stack Memory

### Characteristics

```text
- extremely fast
- automatic cleanup
- contiguous memory
- cache friendly
```

---

## Best Use Cases

- local values
- temporary objects
- small data structures

---

# Heap Memory

## Characteristics

```text
- dynamic lifetime
- flexible allocation
- slower access
- fragmentation possible
```

---

## Best Use Cases

- shared objects
- long-lived state
- complex object graphs

---

# 11. Concurrency Impact

# Struct Concurrency Advantages

## Key Principle

```text
Value semantics reduce shared mutable state.
```

---

## Concurrency Benefits

| Benefit | Why Important |
|---|---|
| Independent Ownership | No shared mutation |
| Reduced Race Conditions | Safer multithreading |
| Easier Sendable Compliance | Better async safety |
| Less Synchronization | Higher scalability |

---

# Class Concurrency Problems

## Common Problems

| Problem | Result |
|---|---|
| Shared Mutable State | Race conditions |
| Synchronization Need | Locking complexity |
| Thread Contention | Performance degradation |
| Hidden Mutation | Difficult debugging |

---

# 12. SwiftUI Perspective

## Why SwiftUI Prefers Structs

SwiftUI depends on:
- immutable rendering
- deterministic diffing
- predictable updates
- compiler optimization

---

## SwiftUI Example

```swift
struct ContentView: View
```

NOT:

```swift
class ContentView
```

---

# 13. When To Use Struct

## Best Struct Use Cases

| Use Case | Why Struct Works Better |
|---|---|
| DTOs | Pure immutable data |
| API Models | Independent ownership |
| SwiftUI Views | Rendering optimization |
| State Models | Safer updates |
| Configurations | Predictable behavior |

---

# 14. When To Use Class

## Best Class Use Cases

| Use Case | Why Class Works Better |
|---|---|
| Managers | Shared service lifecycle |
| Coordinators | Shared navigation state |
| Database Sessions | Shared connections |
| Networking Layer | Shared resources |
| Dependency Containers | Shared object graph |

---

# 15. Principal-Level Insights

## Insight 1

```text
Struct = DATA
Class  = IDENTITY
```

---

## Insight 2

```text
Classes model relationships.
Structs model values.
```

---

## Insight 3

```text
SwiftUI and Swift Concurrency are fundamentally value-oriented frameworks.
```

---

## Insight 4

```text
Most large-scale iOS performance problems come from excessive reference semantics.
```

---

# Final Staff-Level Summary

| Struct | Class |
|---|---|
| Value Semantics | Reference Semantics |
| Safer | Flexible |
| Faster | Dynamic |
| Compiler Optimized | Runtime Flexible |
| Better for Concurrency | Better for Shared Lifecycle |
| SwiftUI Preferred | UIKit/Common Services |

---

# Final One-Line Summary

```text
Use Struct by default.

Move to Class only when identity and shared mutable state are truly required.
```