# Функции и параметры

Функция:

```lena
name(arg:T):R {
    return value
}
```

Пример:

```lena
main():exit_status {
    return ::SUCCESS
}
```

## Режимы параметров

В Lena режим передачи указывается перед именем параметра.

```lena
x:T          # mutable borrow по умолчанию
const x:T    # readonly borrow
yield x:T    # ownership transfer
```

Примеры:

```lena
max(const ar:i32[*]):i32 {
    let mx_v:i32 = i32::MIN

    for val = ar by 1 {
        if (val > mx_v) {
            mx_v = val
        }
    }

    return mx_v
}
```

`const ar:i32[*]` означает, что функция только читает массив.

```lena
resize_to_10(ar:i32[*]):nil! {
    ar.resize(10)
}
```

`ar:i32[*]` означает изменяемый borrow. Функция может менять массив, но не владеет им.

```lena
destroy(yield ar:i32[*]):nil {
    ar.free()
}
```

`yield ar:i32[*]` означает, что функция забирает владение.

Вызовы должны соответствовать сигнатуре:

```lena
let a:i32[*] = {10, 20, 30}

let mx = max(a)        # ok
resize_to_10(a)        # ok
destroy(yield a)       # ok

destroy(a)             # error: expected yield
max(yield a)           # error: max does not take ownership
```

## Prototype declarations

Функция без тела — declaration/header:

```lena
out(format:str, args:...):nil
```

Реализация может быть в runtime, provider-е или другом compilation unit.
