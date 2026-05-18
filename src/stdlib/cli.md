# CLI

`cli` — стандартный модуль для консольного ввода/вывода в host-профиле.

Черновой header:

```lena
cli = module {
    stream = struct {
        handle:i64
    }

    stdout:stream
    stderr:stream
    stdin:stream

    out(format:str, args:...):nil
    err(format:str, args:...):nil

    in(T:type, prompt:str):T!
    scan(format:str):nil!

    flush(s:stream):nil!
}
```

Пользовательский синтаксис:

```lena
let x:i32 = cli::in("Number: "):i32
let name:str = cli::in("Name: "):str
```

`cli::in` требует expected type, потому что от типа зависит parsing.

Нельзя:

```lena
let x = cli::in("Number: ") # error
```

Format strings:

```lena
cli::out("value = {@x}\n")
```

Для `{@x}` тип `x` должен иметь formatting operation.
