# Модули

Модуль — compile-time namespace value.

```lena
cli = module {
    out(format:str, args:...):nil
    err(format:str, args:...):nil
}
```

Доступ:

```lena
cli::out("Hello\n")
```

## Items inside module

Внутри модуля могут быть:

```text
- функции
- prototype declarations
- const/comptime bindings
- nested modules
- type definitions
- state variables
```

## Mutable module state

Обычный `let` внутри module не используется. Для mutable module state используется `state`:

```lena
random = module {
    state seed:u64 = 123

    next():u64 {
        seed = seed * 1103515245 + 12345
        return seed
    }
}
```

`state` должен быть явным, чтобы module-global mutability не появлялась случайно.

## Headers

Функция без тела внутри module — header/prototype:

```lena
out(format:str, args:...):nil
```

Реализация может находиться в runtime/provider/shared library.
