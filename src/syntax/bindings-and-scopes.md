# Объявления и области видимости

## Top-level

На верхнем уровне разрешены:

```text
- function declarations
- const/comptime bindings
- modules
- imports
- foreign blocks
```

`let` на верхнем уровне запрещён:

```lena
let x = 10 # error
```

Top-level binding без `const` считается compile-time/constant binding:

```lena
credit = struct {
    amount:f64,
}

const SYSTEM_LOL:u64 = 1024
```

## Local bindings

Внутри функции:

```lena
let x = 10
let y:i32 = 20
const LIMIT:u64 = 1000
```

`let` — изменяемая локальная переменная.
`const` — неизменяемое значение.

```lena
let x = 10
x = 20 # ok

const y = 10
y = 20 # error
```

Локальное присваивание без объявления требует, чтобы имя уже существовало:

```lena
main():exit_status {
    x = 10 # error if x is not declared
    return ::SUCCESS
}
```

## Type inference

```lena
let a = 100
let b = 10.2
```

`100` сначала имеет внутренний abstract kind `integer`, а `10.2` — `decimal`. Конкретный тип выбирается семантическим анализатором по использованию.

Если контекста нет:

```text
integer -> i32
decimal -> f64
```

`integer` и `decimal` не являются пользовательскими storage types:

```lena
let a:integer # error
```
