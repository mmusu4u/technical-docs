# Swift/SwiftUI → Flutter/Dart Developer Guide

A comprehensive reference for iOS developers transitioning to Flutter. Covers syntax, types, UI, state, async patterns, and more.

---

## Table of Contents

1. [Variables & Constants](#variables--constants)
2. [Data Types](#data-types)
3. [Collections](#collections)
4. [Structs, Classes & Protocols](#structs-classes--protocols)
5. [Functions & Closures](#functions--closures)
6. [Async / Await](#async--await)
7. [UI Components](#ui-components)
8. [State Management](#state-management)
9. [Navigation](#navigation)
10. [Serialization / Codable](#serialization--codable)
11. [Miscellaneous](#miscellaneous)
12. [Tips for Transitioning](#tips-for-transitioning)

---

## Variables & Constants

| Swift          | Dart           | Notes                  |
| -------------- | -------------- | ---------------------- |
| `let x = 10`   | `final x = 10` | Immutable value        |
| `var y = 20`   | `var y = 20`   | Mutable value          |
| Type inference | Type inference | Both languages support |

---

## Data Types

| Swift               | Dart              | Notes                        |
| ------------------- | ----------------- | ---------------------------- |
| `Int`               | `int`             | Integer                      |
| `Double`            | `double`          | Floating point               |
| `Bool`              | `bool`            | Boolean                      |
| `String`            | `String`          | Strings                      |
| `Character`         | `String`          | Dart has no `Character` type |
| `Any`               | `dynamic`         | Any type                     |
| `Optional` (`Int?`) | Nullable (`int?`) | Swift `?` ↔ Dart `?`         |
| `nil`               | `null`            | Null value                   |

---

## Collections

| Swift                           | Dart               | Notes                       |
| ------------------------------- | ------------------ | --------------------------- |
| `[Int]`                         | `List<int>`        | Ordered list                |
| `[String: Int]`                 | `Map<String, int>` | Key-value pairs             |
| `Set<Int>`                      | `Set<int>`         | Unique unordered collection |
| Array methods (`map`, `filter`) | `List` methods     | Similar functional methods  |

---

## Structs, Classes & Protocols

| Swift       | Dart                           | Notes                           |
| ----------- | ------------------------------ | ------------------------------- |
| `struct`    | `class`                        | Dart uses reference types       |
| `class`     | `class`                        | Same                            |
| `protocol`  | `abstract class` / `interface` | Abstract class with methods     |
| `extension` | `extension`                    | Supported since Dart 2.7        |
| `enum`      | `enum`                         | Dart 2.17+ supports enum values |

---

## Functions & Closures

| Swift                                | Dart                       | Notes                          |
| ------------------------------------ | -------------------------- | ------------------------------ |
| `(Int) -> Void`                      | `void Function(int)`       | Function type                  |
| `(Int, Int) -> Int`                  | `int Function(int, int)`   | Multi-param functions          |
| `func sum(a: Int, b: Int) -> Int {}` | `int sum(int a, int b) {}` | Named params differ            |
| `throws`                             | `throw` + `try-catch`      | Exception handling             |
| `@escaping`                          | N/A                        | Dart functions are first-class |

---

## Async / Await

| Swift       | Dart            | Notes                      |
| ----------- | --------------- | -------------------------- |
| `async`     | `async`         | Same concept               |
| `await`     | `await`         | Same concept               |
| `Task {}`   | `Future(() {})` | Task ↔ Future              |
| `try await` | `try await`     | Exception handling similar |

---

## UI Components

| SwiftUI                 | Flutter                            | Notes                |
| ----------------------- | ---------------------------------- | -------------------- |
| `Text("Hello")`         | `Text("Hello")`                    | Direct mapping       |
| `Button(action: {}) {}` | `ElevatedButton(onPressed: () {})` | Button differences   |
| `Image("icon")`         | `Image.asset("assets/icon.png")`   | Asset paths differ   |
| `HStack {}`             | `Row(children: [])`                | Horizontal layout    |
| `VStack {}`             | `Column(children: [])`             | Vertical layout      |
| `ZStack {}`             | `Stack(children: [])`              | Overlay layout       |
| `Spacer()`              | `Spacer()`                         | Same concept         |
| `ScrollView {}`         | `SingleChildScrollView`            | Scrollable container |

---

## State Management

| SwiftUI                   | Flutter                               | Notes                        |
| ------------------------- | ------------------------------------- | ---------------------------- |
| `@State var count = 0`    | `StatefulWidget + setState()`         | Local state                  |
| `@Binding var value: Int` | `ValueNotifier / Provider / Riverpod` | Two-way binding              |
| `@EnvironmentObject`      | `Provider / Riverpod / GetX`          | Global state                 |
| `@ObservedObject`         | `ChangeNotifier`                      | Observable pattern           |
| `@Published`              | `notifyListeners()`                   | Reactivity in ChangeNotifier |

---

## Navigation

| SwiftUI                        | Flutter                                                                      | Notes                    |
| ------------------------------ | ---------------------------------------------------------------------------- | ------------------------ |
| `NavigationLink(destination:)` | `Navigator.push(context, MaterialPageRoute(builder: (context) => Screen()))` | Navigation logic differs |
| `sheet(isPresented:)`          | `showModalBottomSheet()`                                                     | Modal presentation       |
| `tabView`                      | `BottomNavigationBar + IndexedStack`                                         | Tab management           |

---

## Serialization / Codable

| Swift                       | Dart                  | Notes                            |
| --------------------------- | --------------------- | -------------------------------- |
| `Codable`                   | `json_serializable`   | Automatic JSON encoding/decoding |
| `JSONDecoder().decode(...)` | `fromJson` / `toJson` | Factory constructor mapping      |
| `JSONEncoder().encode(...)` | `toJson()`            | Serialization logic              |

---

## Miscellaneous

| Swift                  | Dart                          | Notes                                  |
| ---------------------- | ----------------------------- | -------------------------------------- |
| `guard`                | `if` + early return           | Swift-specific syntax                  |
| `defer`                | `try-finally`                 | Cleanup pattern                        |
| `typealias`            | `typedef`                     | Function type alias                    |
| `@objc`                | N/A                           | Objective-C interop not needed in Dart |
| `Combine`              | `Streams / StreamController`  | Reactive programming mapping           |
| `Timer.scheduledTimer` | `Timer.periodic`              | Timer API differs                      |
| `NotificationCenter`   | `EventBus / StreamController` | Event-driven pattern                   |

---

## Tips for Transitioning

1. **Think declaratively**: Flutter widgets ≈ SwiftUI views.
2. **Reference vs value types**: Dart is reference-based, so immutable `final` is your friend.
3. **State is explicit**: Use `StatefulWidget`, `Provider`, or `Riverpod`.
4. **Async is similar**: `async`/`await` translates directly.
5. **Layouts are widget trees**: Everything is a widget; HStack/VStack → Row/Column.
