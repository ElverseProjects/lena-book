# Struct и enum

## Struct

```lena
credit = struct {
    account_number:i32,
    client_last_name:str,
    client_first_name:str,
    amount:f64,
    percent:i32,
}
```

Создание значения:

```lena
let c:credit = {
    .account_number = 10,
    .client_last_name = "Ivanov",
    .client_first_name = "Ivan",
    .amount = 5000.0,
    .percent = 12,
}
```

Пустой literal:

```lena
let c:credit = {}
```

означает default initialization и требует expected type.

## Nested types

```lena
piano = struct {
    name:str = "NO_NAME",

    color = enum {
        BLACK,
        BROWN,
        WHITE
    }
}
```

Такой nested enum может использоваться в struct literal через expected type:

```lena
let p:piano = {
    .name = "YAMAHA",
    .color = BLACK
}
```

## Enum

```lena
exit_status = enum {
    SUCCESS = 0,
    FAILURE = 1
}
```

Доступ:

```lena
exit_status::SUCCESS
```

Shortcut:

```lena
return ::SUCCESS
```

Shortcut допустим только если ожидаемый тип известен и variant однозначен.
