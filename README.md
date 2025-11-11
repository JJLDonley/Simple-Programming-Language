# Simple Language Grammar Specification

## Overview
**Simple** is a general-purpose programming language with clean syntax emphasizing composition over inheritance.

- **Artifacts:** Unified construct for data structures, enumerations, and namespaces
- **Procedures:** First-class functions with no keyword prefix
- **Minimal conditionals:** Expression-based with `|>` chain operator for short-circuiting
- **Minimal syntax:** No unnecessary keywords, clear and readable

**File Extension:** `.simple`

---

## Core Concepts

### Artifacts
An **Artifact** is Simple's unified construct that serves multiple purposes:
- **Data structures with methods** (similar to classes/structs in other languages)
- **Enumerations** (constant value definitions)
- **Namespaces** (static member collections)

All artifacts use the same syntax: `Identifier { body }` with no keyword prefix.

### Procedures
A **Procedure** is a callable function defined with no keyword prefix:
```
identifier(params) { body }
identifier(params): return_type { body }
```

Procedures are first-class citizens and can be assigned to variables, passed as arguments, and returned from other procedures.

### Conditionals
Conditionals are expression-based with no keywords:
- **Standalone:** `expr { body }` - executes if expression is true
- **Chained:** `|> expr { body }` - short-circuit chain, first match wins

Example:
```
|> score > 90 { return "A" }
|> score > 80 { return "B" }
|> true { return "F" }
```

---

## Keywords

### Control Flow
`while`, `break`, `return`, `skip`

### Declarations
`Mod`

### Literals
`true`, `false`, `null`

---

## Operators

### Arithmetic
`+`, `-`, `*`, `/`, `%`, `++`, `--`

### Bitwise
`&`, `|`, `^`, `<<`, `>>`

### Comparison
`==`, `!=`, `<`, `>`, `<=`, `>=`

### Logical
`&&`, `||`, `!`

### Assignment
`=` (assignment), `:=` (mutable declaration), `::=` (immutable declaration)

### Access
`.` (property access), `[]` (index access)

### Range
`..` (iteration)

### Chain
`|>` (conditional chain)

---

## Operator Precedence

From highest to lowest precedence:

1. `++`, `--` (postfix), `[]`, `.` (member access)
2. `!`, `-` (unary), `++`, `--` (prefix)
3. `*`, `/`, `%`
4. `+`, `-`
5. `<<`, `>>`
6. `<`, `>`, `<=`, `>=`
7. `==`, `!=`
8. `&` (bitwise AND)
9. `^` (bitwise XOR)
10. `|` (bitwise OR)
11. `&&` (logical AND)
12. `||` (logical OR)
13. `|>` (conditional chain)
14. `=`, `:=`, `::=` (assignment/declaration)

Examples:
```
2 + 3 * 4        // 14 (multiplication first)
x && y || z      // (x && y) || z
a & b == c       // a & (b == c)
```

---

## Literals

```
Integer:  123, -456, 0
Float:    3.14, -0.5, 0.0
String:   "hello", 'world'
Boolean:  true, false
Null:     null
```

---

## Variables

**Colon Pattern:**
- `::` (double colon) → immutable
- `:` (single colon) → mutable
- When combining type + value: use `:=` for immutable, `=` for mutable

### Syntax

| Pattern | Mutability | Example |
|---------|------------|---------|
| `ident :: type` | Immutable | `x :: int` |
| `ident ::= value` | Immutable | `x ::= 10` |
| `ident : type := value` | Immutable | `x : int := 10` |
| `ident : type` | Mutable | `y : int` |
| `ident := value` | Mutable | `y := 10` |
| `ident : type = value` | Mutable | `y : int = 10` |
| `ident = value` | Assignment | `y = 20` |

```
// Immutable (double colon)
<ident> :: <type>              // type declaration only
<ident> ::= <expr>             // inferred type
<ident> : <type> := <expr>     // explicit type with value

// Mutable (single colon)
<ident> : <type>               // type declaration only
<ident> := <expr>              // inferred type
<ident> : <type> = <expr>      // explicit type with value

// Assignment
<ident> = <expr>               // assign to existing variable
```

### Examples
```
// Immutable
x :: int               // type only
name ::= "Jae"         // inferred as string
age : int := 25        // explicit type

// Mutable
y : int                // type only
count := 0             // inferred as int
total : int = 100      // explicit type

// Assignment
count = 5              // OK: reassign mutable
name = "John"          // ERROR: cannot assign to immutable
```

---

## Procedures

### Syntax
```
<ident>(<params>) { <body> }
<ident>(<params>): <type> { <body> }
```

### Parameters
```
<params> ::= ε
           | <ident>
           | <ident>: <type>
           | <params>, <params>
```

### Examples
```
greet(name) {
    return "Hello " + name
}

add(a: int, b: int): int {
    return a + b
}

print_message(msg: string) {
    // void return
}
```

### Default Parameters
```
greet(name = "World") {
    return "Hello " + name
}

greet()           // "Hello World"
greet("Jae")      // "Hello Jae"

create_point(x: float = 0.0, y: float = 0.0): Point {
    return Point(x, y)
}
```

### First-Class Procedures
Procedures are first-class citizens and can be assigned to variables, passed as arguments, and returned from other procedures:

```
add(a: int, b: int): int {
    return a + b
}

apply(operation, x: int, y: int): int {
    return operation(x, y)
}

callback ::= add
result ::= apply(callback, 5, 3)  // 8

make_multiplier(factor: int) {
    return (x: int): int {
        return x * factor
    }
}

double ::= make_multiplier(2)
value ::= double(5)  // 10
```

---

## Artifacts

### Syntax
```
<ident> {
    <property>*
    <method>*
}
```

### Properties
```
// Immutable
<property> ::= <ident> :: <type>
             | <ident> ::= <expr>
             | <ident> : <type> := <expr>

// Mutable
<property> ::= <ident> : <type>
             | <ident> := <expr>
             | <ident> : <type> = <expr>
```

### Methods
```
<method> ::= <ident>(<params>) { <body> }
           | <ident>(<params>): <type> { <body> }
```

### Property Access
Use `.` prefix to reference artifact properties within methods:
```
.<property>
```

### Example
```
Person {
    name :: string               // immutable, no default
    age : int                    // mutable, no default
    title : string := "Mr."      // immutable with default
    count : int = 0              // mutable with default
    
    greet(): string {
        return "Hello, " + .name + "! Age: " + str(.age)
    }
    
    birthday() {
        .age = .age + 1
    }
}

p ::= Person("Jae", 25)
p.greet()
p.birthday()
```

### Composition
Artifacts support composition only. No inheritance.

### Instantiation
```
<instance> := <ArtifactName>(<args>)    // mutable instance
<instance> ::= <ArtifactName>(<args>)   // immutable instance
```

Arguments are passed to initialize properties in declaration order.

### Static Usage (Namespace Pattern)
Artifacts can also be used as namespaces without instantiation:

```
Math {
    PI : float := 3.14159
    E : float := 2.71828
    
    abs(x: float): float {
        |> x < 0.0 { return -x }
        |> true { return x }
    }
    
    sqrt(x: float): float {
        // implementation
    }
}

// Direct access without instantiation
value ::= Math.PI
result ::= Math.abs(-5.0)
```

---

## Artifacts (Enum Pattern)

### Syntax
```
<ident> {
    <ident> = <literal>,
    <ident>,
    ...
}
```

### Example
```
Colors {
    Red = 0,
    Blue,
    Green
}

Status {
    Pending = 1,
    Active = 2,
    Completed = 3
}
```

Artifacts can represent enumerated values by defining constant members.

---

## Collections

### Declaration
```
<ident> := [<expr>, <expr>, ...]    // mutable list
<ident> ::= [<expr>, <expr>, ...]   // immutable array
```

### Access
```
<ident>[<index>]   // 1-based indexing
```

### Examples
```
numbers := [10, 20, 30, 40, 50]     // mutable list
names ::= ["Alice", "Bob", "Charlie"] // immutable array

first ::= numbers[1]   // 10 (first element)
third ::= numbers[3]   // 30 (third element)
numbers[2] = 25        // OK: modify mutable list
names[1] = "Dave"      // ERROR: cannot modify immutable array
```

**Note:** Simple uses 1-based indexing. The first element is at index 1.

---

## Control Flow

### Conditional Expressions

#### Standalone Conditional
```
<expr> { <body> }
```

Executes body if expression evaluates to true.

#### Chained Conditionals (Short-circuit)
```
|> <expr> { <body> }
|> <expr> { <body> }
|> <expr> { <body> }
```

Each condition prefixed with `|>` forms a short-circuit chain. Only the first true condition executes.

### Examples
```
// Standalone conditional (independent, always evaluates)
age > 18 {
    print("Adult")
}

count > 0 {
    print("Has items")
}

// Chained conditionals (short-circuit, first match only)
|> x > 10 {
    return "large"
} 
|> x > 5 {
    return "medium"
}
|> true {
    return "small"
}

// In a procedure
grade(score: int): string {
    |> score > 90 { return "A" }
    |> score > 80 { return "B" }
    |> score > 70 { return "C" }
    |> score > 60 { return "D" }
    |> true { return "F" }
}

// Nested conditionals
process(value: int) {
    value > 0 {
        |> value > 100 { print("very large") }
        |> value > 50 { print("large") }
        |> true { print("positive") }
    }
}
```

---

## Loops

### While Loop
```
while <expr> { <body> }
```

### Example
```
count := 0
while count < 10 {
    count = count + 1
}
```

### Range Iteration
```
<ident>, <start> .. <end> { <body> }         // literal end value
<ident>, <start> .. <collection> { <body> }  // collection.length at compile time
```

### Examples
```
i, 1 .. 10 {
    // i goes from 1 to 10 (inclusive)
}

i, 5 .. 20 {
    // i goes from 5 to 20 (inclusive)
}

i, 1 .. items {
    // i goes from 1 to items.length
    value = items[i]
}

i, 3 .. names {
    // i goes from 3 to names.length
    // skips first 2 elements
}
```

**Note:** Ranges are inclusive and use 1-based indexing.

---

## Control Keywords

### break
Exit the current loop immediately.

### return
Exit the current function, optionally returning a value.

### skip
Skip to the next iteration of the loop (continue).

### Examples
```
i, 1 .. 10 {
    i == 5 {
        skip
    }
    i == 8 {
        break
    }
}

find(items, target) {
    i, 1 .. items {
        items[i] == target {
            return i
        }
    }
    return -1
}
```

---

## Modules

### Syntax
```
Mod <path>;
```

### Path Resolution
```
<path> ::= <ident>           // looks for <ident>.s in current directory
         | <path>/<ident>    // looks for <ident>.s in specified directory
```

### Examples
```
Mod utils;         // imports utils.s
Mod src/game;      // imports src/game.s
Mod lib/math/vec;  // imports lib/math/vec.s
```

### Behavior
Imports all declarations from the specified module.

---

## Comments

### Single Line
```
// This is a comment
```

---

## Built-in Types

```
int      // integer numbers
float    // floating point numbers
string   // text
bool     // true or false
```

---

## Type Coercion and Arithmetic Rules

### Division
```
int / int = int           // 10 / 3 = 3
float / float = float     // 10.0 / 3.0 = 3.333...
int / float = float       // 10 / 3.0 = 3.333...
float / int = float       // 10.0 / 3 = 3.333...
```

### String Concatenation
Any type concatenated with a string becomes a string:
```
"Age: " + 25              // "Age: 25"
"Value: " + 3.14          // "Value: 3.14"
"Status: " + true         // "Status: true"
```

### Numeric Operations
```
int + int = int
float + float = float
int + float = float
float + int = float
```

Same rules apply for `-`, `*`, `%`

---

## Built-in Functions

### Type Casting
```
int(x)     // convert to integer
float(x)   // convert to float
str(x)     // convert to string
bool(x)    // convert to boolean
```

### Examples
```
float(10) / 3        // 3.333... (force float division)
int(3.14)            // 3
str(42)              // "42"
bool(0)              // false
```

---

## Language Semantics

### Scope Rules
- **Block scope:** Variables declared in `{}` blocks are local to that block
- **Global access:** Procedures and blocks can access global variables
- **Shadowing allowed:** Variables can be redeclared with `:=` or `::=` to shadow existing names
- **Assignment:** `=` assigns to existing variables; requires mutable variable

```
x ::= 10          // global (immutable)

example() {
    x = 5         // ERROR: cannot assign to immutable x
    x := 5        // OK: declares new mutable x (shadows global)
    x = 10        // OK: assigns to the mutable local x
    
    y := 20       // local mutable
    y = 30        // OK: assigns to mutable y
    
    z ::= 40      // local immutable
    z = 50        // ERROR: cannot assign to immutable z
    
    true {
        x ::= 100 // OK: declares new immutable x (shadows local x)
        w := 60   // local to block
    }
    // w not accessible here (out of scope)
}
```

### Return Type Inference
When no return type is specified:
- Procedure has `return <expr>` → type inferred from expression
- Procedure has only `return` or no return → void
- Multiple different return types → compile error (explicit type required)

```
get_value() {
    return 42         // inferred as int
}

do_something() {
    return            // void
}

print_text() {
    // no return       // void
}
```

### Memory Model
- **Primitives** (`int`, `float`, `bool`) → pass by value
- **Artifacts, Arrays, Lists** → pass by reference
- **Mutability:** `:=` creates mutable references, `::=` creates immutable references (prevents modification, not copying)

### Entry Point
Programs execute from `main()` if present, otherwise execute top-level statements in order.

---

## Complete Example

```
Mod math/utils;

Status {
    Idle = 0,
    Running,
    Stopped
}

Vec2 {
    x :: float
    y :: float
    
    length(): float {
        return sqrt(.x * .x + .y * .y)
    }
    
    add(other: Vec2): Vec2 {
        return Vec2(.x + other.x, .y + other.y)
    }
}

Math {
    PI : float := 3.14159
    
    sqrt(x: float): float {
        // implementation
    }
}

classify(value: int): string {
    |> value > 100 { return "very large" }
    |> value > 50 { return "large" }
    |> value > 10 { return "medium" }
    |> value > 0 { return "small" }
    |> true { return "non-positive" }
}

main() {
    pos ::= Vec2(3.0, 4.0)
    vel ::= Vec2(1.0, 0.0)
    
    status ::= Status.Running
    
    status == Status.Running {
        newPos ::= pos.add(vel)
        distance ::= newPos.length()
    }
    
    items ::= [10, 20, 30, 40, 50]
    sum := 0
    
    i, 1 .. items {
        sum = sum + items[i]
    }
    
    i, 1 .. 10 {
        i % 2 == 0 {
            skip
        }
    }
    
    count := 0
    while count < 5 {
        count++
    }
    
    radius ::= 5.0
    area ::= Math.PI * radius * radius
}
```

---

## Grammar Rules (BNF-style)

```
<program> ::= <statement>*

<statement> ::= <var_decl>
              | <proc_decl>
              | <artifact_decl>
              | <mod_decl>
              | <conditional_stmt>
              | <while_stmt>
              | <for_stmt>
              | <return_stmt>
              | <break_stmt>
              | <skip_stmt>
              | <expr_stmt>

<var_decl> ::= <ident> "::" <type>
             | <ident> "::=" <expr>
             | <ident> ":" <type> ":=" <expr>
             | <ident> ":" <type>
             | <ident> ":=" <expr>
             | <ident> ":" <type> "=" <expr>
             | <ident> "=" <expr>

<proc_decl> ::= <ident> "(" <params> ")" <block>
              | <ident> "(" <params> ")" ":" <type> <block>

<params> ::= ε
           | <param> ("," <param>)*

<param> ::= <ident>
          | <ident> ":" <type>
          | <ident> "=" <expr>
          | <ident> ":" <type> "=" <expr>

<artifact_decl> ::= <ident> "{" <artifact_body> "}"

<artifact_body> ::= (<property_decl> | <proc_decl> | <enum_member>)*

<property_decl> ::= <ident> "::" <type>
                  | <ident> "::=" <expr>
                  | <ident> ":" <type> ":=" <expr>
                  | <ident> ":" <type>
                  | <ident> ":=" <expr>
                  | <ident> ":" <type> "=" <expr>

<enum_member> ::= <ident>
                | <ident> "=" <literal>

<mod_decl> ::= "Mod" <path> ";"

<path> ::= <ident> ("/" <ident>)*

<conditional_stmt> ::= <expr> <block>
                     | "|>" <expr> <block> ("|>" <expr> <block>)*

<while_stmt> ::= "while" <expr> <block>

<for_stmt> ::= <ident> "," <expr> ".." <expr> <block>

<return_stmt> ::= "return" <expr>?

<break_stmt> ::= "break"

<skip_stmt> ::= "skip"

<expr_stmt> ::= <expr>

<block> ::= "{" <statement>* "}"

<expr> ::= <literal>
         | <ident>
         | <expr> <binary_op> <expr>
         | <unary_op> <expr>
         | <expr> "." <ident>
         | <expr> "[" <expr> "]"
         | <expr> "(" <args> ")"
         | "(" <expr> ")"
         | "[" <list> "]"
         | <ident> "(" <args> ")"

<args> ::= ε
         | <expr> ("," <expr>)*

<list> ::= ε
         | <expr> ("," <expr>)*

<binary_op> ::= "+" | "-" | "*" | "/" | "%"
              | "==" | "!=" | "<" | ">" | "<=" | ">="
              | "&&" | "||"
              | "&" | "|" | "^" | "<<" | ">>"

<unary_op> ::= "-" | "!" | "++" | "--"

<type> ::= "int" | "float" | "string" | "bool" | <ident>

<literal> ::= <int> | <float> | <string> | <bool> | "null"

<ident> ::= [a-zA-Z_][a-zA-Z0-9_]*
```
