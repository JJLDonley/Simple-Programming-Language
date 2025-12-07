# Simple Language Test Suite Checklist

## Phase 1: Lexer/Tokenizer Tests

### Keywords
- [ ] `while`, `break`, `return`, `skip`
- [ ] `Mod`
- [ ] `true`, `false`, `null`

### Operators
- [ ] Arithmetic: `+`, `-`, `*`, `/`, `%`, `++`, `--`
- [ ] Bitwise: `&`, `|`, `^`, `<<`, `>>`
- [ ] Comparison: `==`, `!=`, `<`, `>`, `<=`, `>=`
- [ ] Logical: `&&`, `||`, `!`
- [ ] Assignment: `=`, `:`, `::`
- [ ] Access: `.`, `[]`
- [ ] Range: `..`
- [ ] Chain: `|>`

### Literals
- [ ] Integer: `123`, `-456`, `0`
- [ ] Float: `3.14`, `-0.5`, `0.0`
- [ ] String: `"hello"`, `'world'`
- [ ] Boolean: `true`, `false`
- [ ] Null: `null`

### Identifiers
- [ ] Valid: `x`, `_value`, `myVar123`
- [ ] Invalid: `123abc`, `my-var`

### Comments
- [ ] Single-line: `// comment`
- [ ] Ignore comments in output

### Whitespace
- [ ] Spaces, tabs, newlines properly ignored
- [ ] Proper handling in strings

---

## Phase 2: Parser Tests

### Variable Declarations

#### Immutable Variables
- [ ] `x :: int = 10`
- [ ] `name :: string = "Alice"`
- [ ] `PI :: float = 3.14`
- [ ] Error: `x :: int` (no value)

#### Mutable Variables
- [ ] `y : int = 20`
- [ ] `count : int = 0`
- [ ] Error: `y : int` (no value for mutable)

#### Assignment
- [ ] `x = 5` (to mutable variable)
- [ ] Error: `x = 5` (to immutable variable)

### Arrays and Lists

#### Fixed Arrays
- [ ] `arr : [5]int = [1, 2, 3, 4, 5]`
- [ ] `coords :: [3]float = [0.0, 0.0, 0.0]`
- [ ] Error: `arr : [3]int = [1, 2, 3, 4, 5]` (size mismatch)

#### Dynamic Lists
- [ ] `list : [int] = [1, 2, 3]`
- [ ] `names :: [string] = ["Alice", "Bob"]`
- [ ] Empty list: `items : [int] = []`

#### Array/List Access
- [ ] `arr[1]` (1-based indexing)
- [ ] `arr[5]` (last element)
- [ ] Error: `arr[0]` (invalid index)

### Procedures

#### Basic Procedures
- [ ] `greet(name: string): string { return "Hello " + name }`
- [ ] `add(a: int, b: int): int { return a + b }`
- [ ] `print_msg(msg: string): void { }`

#### Default Parameters
- [ ] `greet(name: string = "World"): string { }`
- [ ] Call with default: `greet()`
- [ ] Call with argument: `greet("Alice")`

#### First-Class Procedures
- [ ] Assign to variable: `callback :: (int, int): int = add`
- [ ] Pass as parameter
- [ ] Return from procedure

#### Error Cases
- [ ] Missing return type
- [ ] Missing parameter types
- [ ] Type mismatch in return

### Artifacts

#### Data Structures
- [ ] Basic artifact with properties
- [ ] Immutable properties: `name :: string = ""`
- [ ] Mutable properties: `age : int = 0`
- [ ] Methods with return types

#### Property Access
- [ ] `.property` syntax inside methods
- [ ] Error: accessing property without `.` prefix

#### Instantiation
- [ ] Constructor method: `p :: Person = Person.new("Alice", 25)`
- [ ] Struct-style: `p : Person = { .name = "Bob", .age = 30 }`
- [ ] Constructor returns artifact type
- [ ] All properties initialized in struct-style

#### Static/Namespace Usage
- [ ] Access without instantiation: `Math.PI`
- [ ] Call static method: `Math.abs(-5.0)`

#### Enums
- [ ] Sequential values: `Red = 0, Blue, Green`
- [ ] Explicit values: `Pending = 1, Active = 2`
- [ ] Access: `Status.Active`

### Control Flow

#### Standalone Conditionals
- [ ] `x > 10 { print("large") }`
- [ ] Nested blocks

#### Chained Conditionals
- [ ] `|> x > 10 { } |> x > 5 { } |> true { }`
- [ ] First match wins (short-circuit)
- [ ] Must use `|>` prefix for chains

#### While Loops
- [ ] `while x < 10 { x = x + 1 }`
- [ ] `break` statement
- [ ] `skip` statement

#### Range Iteration
- [ ] `i, 1 .. 10 { }` (literal range)
- [ ] `i, 1 .. arr { }` (array length)
- [ ] `i, 3 .. arr { }` (skip first 2)

### Expressions

#### Arithmetic
- [ ] `2 + 3 * 4` (precedence)
- [ ] `(2 + 3) * 4` (parentheses)
- [ ] Unary: `-x`, `++i`, `i++`, `--i`, `i--`

#### Logical
- [ ] `x && y || z`
- [ ] `!x`

#### Comparison
- [ ] `x == y`, `x != y`
- [ ] `x < y`, `x <= y`, `x > y`, `x >= y`

#### Bitwise
- [ ] `a & b`, `a | b`, `a ^ b`
- [ ] `a << 2`, `a >> 2`

#### Member Access
- [ ] `obj.property`
- [ ] `obj.method()`

#### Array/List Indexing
- [ ] `arr[1]`
- [ ] `arr[i + 1]`

### Modules
- [ ] `Mod utils;`
- [ ] `Mod src/game;`
- [ ] `Mod lib/math/vec;`
- [ ] Error: circular imports
- [ ] Error: file not found

---

## Phase 3: Type Checker Tests

### Type Inference
- [ ] Literal types: `10` → `int`, `3.14` → `float`
- [ ] String concatenation with any type
- [ ] Arithmetic promotion: `int + float` → `float`

### Type Checking

#### Variables
- [ ] Assignment type matches declaration
- [ ] Error: `x : int = "string"`
- [ ] Error: assign wrong type to variable

#### Procedures
- [ ] Parameter types match
- [ ] Return type matches actual return
- [ ] Error: return wrong type
- [ ] Error: missing return in non-void

#### Arrays/Lists
- [ ] Element types match declaration
- [ ] Error: `arr : [int] = [1, "two", 3]`

#### Artifacts
- [ ] Property types match
- [ ] Method return types match
- [ ] Constructor arguments match properties

### Type Coercion

#### Division Rules
- [ ] `int / int` → `int`
- [ ] `float / float` → `float`
- [ ] `int / float` → `float`
- [ ] `float / int` → `float`

#### Numeric Operations
- [ ] `int + int` → `int`
- [ ] `int + float` → `float`
- [ ] Same for `-`, `*`, `%`

#### String Concatenation
- [ ] `"Age: " + 25` → `"Age: 25"`
- [ ] `"Pi: " + 3.14` → `"Pi: 3.14"`
- [ ] `"Active: " + true` → `"Active: true"`

---

## Phase 4: Semantic Analysis Tests

### Scope Rules
- [ ] Block-level scope
- [ ] Variable shadowing
- [ ] Global variable access
- [ ] Error: use before declaration
- [ ] Error: undefined variable

### Mutability
- [ ] Assign to mutable variable
- [ ] Error: assign to immutable variable
- [ ] Immutable array/list modification
- [ ] Mutable array/list modification

### Control Flow
- [ ] `return` exits procedure
- [ ] `break` exits loop
- [ ] `skip` continues loop
- [ ] Error: `break` outside loop
- [ ] Error: `skip` outside loop
- [ ] Error: `return` outside procedure

### Array Bounds
- [ ] Valid index access (1-based)
- [ ] Error: index 0
- [ ] Error: index > length
- [ ] Error: negative index

---

## Phase 5: Code Generation (C) Tests

### Variable Translation
- [ ] Immutable → `const type name`
- [ ] Mutable → `type name`
- [ ] Proper initialization

### Array/List Translation
- [ ] Fixed array → `type name[size]`
- [ ] Dynamic list → `struct { type* data; int length; int capacity; }`
- [ ] List operations → helper functions

### Procedure Translation
- [ ] `proc(params): type { }` → `type proc(params) { }`
- [ ] Default parameters → overloaded functions or macro
- [ ] First-class procedures → function pointers

### Artifact Translation
- [ ] Artifact → `struct`
- [ ] Properties → struct fields
- [ ] Methods → functions with struct pointer
- [ ] `.property` → `self->property`

### Control Flow Translation
- [ ] `while` → `while`
- [ ] Range iteration → `for` loop (adjust for 1-based)
- [ ] `break` → `break`
- [ ] `skip` → `continue`
- [ ] `return` → `return`

### Conditional Translation
- [ ] Standalone: `expr { }` → `if (expr) { }`
- [ ] Chained: `|> expr { }` → `if (expr) { } else if (...) { }`

### Expression Translation
- [ ] Operators map 1-to-1
- [ ] String concatenation → helper function
- [ ] Type casting → C casts
- [ ] 1-based indexing → subtract 1 in generated code

### Memory Management
- [ ] Stack allocation for arrays
- [ ] Heap allocation for lists (malloc/free)
- [ ] RAII-style cleanup where possible
- [ ] Proper memory cleanup on scope exit

---

## Phase 6: Integration Tests

### Complete Programs

#### Hello World
```
main(): void {
    print("Hello, World!")
}
```

#### FizzBuzz
```
main(): void {
    i, 1 .. 100 {
        |> i % 15 == 0 { print("FizzBuzz") }
        |> i % 3 == 0 { print("Fizz") }
        |> i % 5 == 0 { print("Buzz") }
        |> true { print(str(i)) }
    }
}
```

#### Data Structure
```
Point {
    x :: float = 0.0
    y :: float = 0.0
    
    new(x: float, y: float): Point {
        return { .x = x, .y = y }
    }
    
    distance(): float {
        return sqrt(.x * .x + .y * .y)
    }
}

main(): void {
    p :: Point = Point.new(3.0, 4.0)
    d :: float = p.distance()
    
    p2 : Point = { .x = 5.0, .y = 12.0 }
}
```

#### Array Processing
```
sum(arr: [int]): int {
    total : int = 0
    i, 1 .. arr {
        total = total + arr[i]
    }
    return total
}

main(): void {
    numbers :: [int] = [10, 20, 30, 40, 50]
    result :: int = sum(numbers)
}
```

#### Composition Example
```
Transform {
    x : float = 0.0
    y : float = 0.0
    
    new(): Transform {
        return { .x = 0.0, .y = 0.0 }
    }
}

Velocity {
    dx : float = 0.0
    dy : float = 0.0
    
    new(): Velocity {
        return { .dx = 0.0, .dy = 0.0 }
    }
}

Entity {
    transform : Transform = { .x = 0.0, .y = 0.0 }
    velocity : Velocity = { .dx = 0.0, .dy = 0.0 }
    
    new(): Entity {
        return { .transform = Transform.new(), .velocity = Velocity.new() }
    }
    
    update(delta: float): void {
        .transform.x = .transform.x + .velocity.dx * delta
        .transform.y = .transform.y + .velocity.dy * delta
    }
}

main(): void {
    entities : [Entity] = []
    
    i, 1 .. 100 {
        e :: Entity = Entity.new()
        entities.push(e)
    }
    
    i, 1 .. entities {
        entities[i].update(0.016)
    }
}
```

---

## Phase 7: Error Handling Tests

### Lexer Errors
- [ ] Unclosed string
- [ ] Invalid character
- [ ] Malformed number

### Parser Errors
- [ ] Missing semicolon (if required)
- [ ] Mismatched braces
- [ ] Invalid syntax
- [ ] Unexpected token

### Type Errors
- [ ] Type mismatch in assignment
- [ ] Type mismatch in operation
- [ ] Invalid type in declaration
- [ ] Return type mismatch

### Semantic Errors
- [ ] Undefined variable
- [ ] Undefined function
- [ ] Undefined artifact
- [ ] Redeclaration error
- [ ] Immutability violation
- [ ] Scope violation

---

## Phase 8: Optimization Tests

### Dead Code Elimination
- [ ] Unreachable code after `return`
- [ ] Unused variables
- [ ] Unused procedures

### Constant Folding
- [ ] `2 + 3` → `5` at compile time
- [ ] `10 * 0` → `0`

### Loop Optimization
- [ ] Loop unrolling for fixed ranges
- [ ] Strength reduction

---

## Phase 9: C Output Validation

### Compilation
- [ ] Generated C compiles with `gcc`
- [ ] Generated C compiles with `clang`
- [ ] No warnings at `-Wall -Wextra`

### Runtime
- [ ] Programs execute correctly
- [ ] No memory leaks (valgrind)
- [ ] No undefined behavior (sanitizers)

### Performance
- [ ] Array access is O(1)
- [ ] List operations match expectations
- [ ] Stack allocation for fixed arrays

---

## Phase 10: Edge Cases

### Numeric Edge Cases
- [ ] Integer overflow
- [ ] Division by zero
- [ ] Float precision

### String Edge Cases
- [ ] Empty strings
- [ ] Escape sequences
- [ ] Unicode handling

### Collection Edge Cases
- [ ] Empty arrays/lists
- [ ] Single element
- [ ] Large collections
- [ ] Out of bounds access

### Recursion
- [ ] Simple recursion
- [ ] Mutual recursion
- [ ] Tail recursion
- [ ] Stack overflow protection

### Module System
- [ ] Circular dependencies
- [ ] Deep module trees
- [ ] Name conflicts across modules

---

## Test Execution Checklist

### Unit Tests
- [ ] Lexer unit tests
- [ ] Parser unit tests
- [ ] Type checker unit tests
- [ ] Semantic analyzer unit tests
- [ ] Code generator unit tests

### Integration Tests
- [ ] End-to-end compilation
- [ ] Multi-file programs
- [ ] Module imports

### Regression Tests
- [ ] Previously fixed bugs don't reappear
- [ ] Maintain test suite for all fixes

### Performance Tests
- [ ] Compilation speed benchmarks
- [ ] Generated code performance
- [ ] Memory usage analysis

---

## Documentation Tests

### Language Specification
- [ ] All examples compile
- [ ] All syntax rules validated
- [ ] Grammar is unambiguous

### Error Messages
- [ ] Clear error descriptions
- [ ] Line/column information
- [ ] Helpful suggestions

### Standard Library
- [ ] All built-in functions tested
- [ ] Type conversions work
- [ ] String operations work
