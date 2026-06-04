# Providers и `@`-пространства

Все foreign/backend/compiler модули имеют префикс `@`.

```rust
@LLVM
@C
@Rust
@Julia
@Lena
```

## Foreign функции и переменные

Для вызова сторонней функции существует несколько методов:

1. Вставка foreign кода:

```rust

@Julia {
    using Lena

    @lena function quadratic2(a::Float64, b::Float64, c::Float64)
        sqr_term = sqrt(b^2-4a*c)
        r1 = quadratic(a, sqr_term, b)
        r2 = quadratic(a, -sqr_term, b)
        r1, r2
    end

    Base.@ccallable function increment(count::Cint)::Cint
        return count + 1
    end
}

```

## Foreign block

```lena
@Julia {
    function f(x::Int64, y::Int64)::Int64
        return x + y * x / 2
    end
}
```

Содержимое foreign block не лексится как Lena. Оно сохраняется как raw source плюс имя языка/provider-а.

## Providers are optional

`@Julia` существует только если подключён Julia provider.

На embedded-профиле:

```text
@Julia unavailable
@Graal unavailable
@C available
@LLVM available
@Lena available
```

На host-профиле можно включить:

```text
@C
@Rust
@Julia
@Graal
```

## Capability discovery

Compiler/provider должен уметь сказать, какие поля он поддерживает.

Пример команды будущего CLI:

```bash
lena provider list
lena backend show LLVM
lena target show cortex-m4
```

Пример ошибки:

```text
error: provider @LLVM has no field `float96`
help: available fields: i1, i8, i16, i32, i64, i128, half, float, double, fp128
```
