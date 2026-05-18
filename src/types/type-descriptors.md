# Дескриптор типов type {}

Пример объявления типа `i32` из стандартной библиотеки:

```rust
i32 = type {
    repr:@LLVM.repr = @LLVM.int(bits = 32, signed = true)

    const MIN:self = -2147483648
    const MAX:self = 2147483647
    const BITS:self = 32
    const DEFAULT:self = 0

    extern parse(const s:str):self!
    extern format(v:self):str
}
```

Можно заметить, что сигнатура `type {}` включает в себя семантику `enum {}` и `struct {}`.
Требованием объявления `type {}` является поле `repr:@LLVM.repr` либо `repr:type`. 
Остальные поля могут являться:

- Константами
- Функциями

Для стандартных типов, и тех типов, что будут использоваться в функциях стандартной библиотеки, необходимо иметь поля:

```rust
name = type {
    repr:@LLVM.repr = @LLVM... # backend репрезентация типа
    ## или
    repr:type = ... # образование от существующего типа

    const MIN:self = ...
    const MAX:self = ...
    const DEFAULT:self = ...

    extern parse(const s:str):self!
    extern format(v:self):str
}
```

## LLVM backend репрезентация типа




## Storage constructors

Компилятор должен знать ограниченный набор storage constructors:

```text
builtin::int(bits, signed)
builtin::float(bits, llvm, standard)
builtin::bool()
builtin::struct { ... }
builtin::enum(backing)
builtin::ptr(to, mutable, volatile)
builtin::array(element, length)
builtin::dynamic_array(element)
builtin::function(params, result)
builtin::opaque(...)
```

Пользователь может создавать много типов, но каждый тип должен сводиться к понятному storage.

## Foreign mappings

`foreign {}` описывает связь типа с другими языками/providers.

```lena
foreign {
    llvm = @LLVM.double
    c = "double"
    rust = "f64"
    julia = "Float64"
}
```

Если mapping отсутствует, тип нельзя передавать через соответствующий provider без явного adapter-а.

## `@Lena`

`@Lena` — compiler/self provider.

Примеры будущих полей:

```lena
@Lena.version
@Lena.target
@Lena.profile
@Lena.has_provider(@Julia)
@Lena.has_backend(@LLVM)
```

## Capability discovery

Backend/provider namespaces вроде `@LLVM` не должны быть магией.

Перед semantic analysis компилятор должен зарегистрировать доступные поля providers:

```text
@LLVM.i32
@LLVM.i64
@LLVM.float
@LLVM.double
@LLVM.fp128
...
```

Если пользователь пишет несуществующее поле:

```lena
@LLVM.float96
```

анализатор должен дать ошибку и список доступных полей.
