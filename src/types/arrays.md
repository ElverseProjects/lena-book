# Массивы

В Lena есть два базовых массива:

```text
T[]    static array with inferred length
T[N]   static array with explicit length
T[*]   dynamic owned array
```

## Static arrays

```lena
let a:i32[] = {10, 20, 30}
let b:i32[3] = {10, 20, 30}
```

Static array имеет фиксированную длину.

## Dynamic arrays

```lena
let a:i32[*] = {10, 20, 30}
```

`T[*]` — встроенный generic type constructor динамического массива.

У каждого `T[*]` есть type-level constructors:

```lena
T[*]::alloc(capacity:u64):T[*]
T[*]::from(values:T[]):T[*]
```

И value-level methods:

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
let a:i32[*] = i32[*]::alloc(10)

a.push(100) else err {
    cli::err("push failed: {@err}\n")
    return ::FAILURE
}

a.resize(20) else err {
    cli::err("resize failed: {@err}\n")
    return ::FAILURE
}
```

`a.free()` consumes the array. После этого `a` недоступен, пока не будет присвоено новое значение.

## Literal conversion

```lena
let a:i32[*] = {10, 20, 30}
```

можно понимать как сахар для:

```lena
let a:i32[*] = i32[*]::from({10, 20, 30})
```

Если expected type `i32[]`, literal остаётся static array.

```lena
let a:i32[]  = {10, 20, 30}
let b:i32[*] = {10, 20, 30}
```

## Stacking array constructors

Array constructors stack from left to right.

```lena
T[4][*]
```

значит:

```text
dynamic array of static arrays of 4 T
```

То есть:

```lena
let rows:i32[4][*]
```

`rows` — dynamic array. Его элементы имеют тип `i32[4]`.

```lena
rows.resize(10)
rows.push({1, 2, 3, 4})
```

А:

```lena
T[*][6]
```

значит:

```text
static array of 6 dynamic arrays of T
```

```lena
let buckets:i32[*][6]
```

`buckets` — static array длины 6. `buckets[0]` имеет тип `i32[*]`.

```lena
buckets.resize(10)      # error: outer container is static
buckets[0].resize(10)   # ok
```

## Matrix is not nested array

`T[N][M]` — это nested arrays, не математическая матрица.

Матрица должна быть отдельным типом:

```lena
let m:matrix(f64, 3, 3) = {}
```

Matrix type может иметь свою семантику: row-major/column-major layout, multiplication, transpose, slicing.
