# Выражения

## Primary expressions

```text
literals
paths
grouped expressions
array literals
struct literals
struct/enum/module/type constructors
```

## Paths

```lena
cli::out
exit_status::SUCCESS
::SUCCESS
my_piano.price.value
@Julia.f
@LLVM.i32
@Lena.version
```

`::` — namespace/type/module access.
`.` — field access and UFCS/method call.
`@Name` — provider/backend/compiler namespace.

## Cast

Postfix cast:

```lena
expr:T
```

Примеры:

```lena
let x = cli::in("Number: "):i32
let raw = exit_status::SUCCESS : i32
```

## Array and struct literals

```lena
let a:i32[] = {10, 20, 30}
let d:i32[*] = {10, 20, 30}
```

Expected type решает, будет ли literal статическим или динамическим массивом.

Struct literal:

```lena
let c:credit = {
    .account_number = 10,
    .amount = 5000.0,
}
```

Пустой `{}` требует expected type:

```lena
let c:credit = {} # ok
let x = {}        # error
```

## Operators

Logical operators are words:

```lena
and or not
```

Bitwise operators:

```lena
& | ^ ~ << >>
```

Assignment operators:

```lena
= += -= *= /= %= &= |= ^= <<= >>=
```

`!` is not logical not. It is reserved for fallible types/operations.

## Increment

```lena
i++
i--
```

`i++` returns the old value and then increments `i`.
