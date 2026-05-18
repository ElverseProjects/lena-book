# Динамические массивы

`T[*]` — встроенный dynamic array type constructor.

## Constructors

```lena
T[*]::alloc(capacity:u64):T[*]
T[*]::from(values:T[]):T[*]
```

Пример:

```lena
let a:i32[*] = i32[*]::alloc(10)
let b:i32[*] = {10, 20, 30}
```

Второй пример эквивалентен:

```lena
let b:i32[*] = i32[*]::from({10, 20, 30})
```

## Methods

```lena
a.len():u64
a.cap():u64
a.resize(new_len:u64):nil!
a.push(value:T):nil!
a.pop():T!
a.free():nil
```

Пример:

```lena
let a:i32[*] = i32[*]::alloc(4)

a.push(10) else err {
    cli::err("push failed: {@err}\n")
    return ::FAILURE
}

a.resize(10) else err {
    cli::err("resize failed: {@err}\n")
    return ::FAILURE
}
```

`free` consumes array:

```lena
a.free()
```

После `a.free()` переменная `a` больше не доступна, пока ей не присвоят новое значение.

## Implementation note

На уровне lowering dynamic array может быть структурой вида:

```text
ptr: *T
len: u64
cap: u64
```

Но пользователь работает с `T[*]`, а не с полями реализации.
