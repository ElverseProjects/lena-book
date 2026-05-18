# Строки

`str` — тип, а не модуль.

Базовые операции строки являются associated functions типа `str`:

```lena
str::len(const s:str):u64
str::eq(const a:str, const b:str):bool
str::clone(const s:str):str
str::format(const s:str):str
```

Через UFCS:

```lena
s.len()
a.eq(b)
```

Отдельный модуль `str = module { ... }` не используется, потому что `str` уже имя типа.

## Open question

Модель ownership строк пока не финализирована.

Возможные варианты:

```text
str     — immutable string view
string  — owned heap string
```

или один `str` с ownership/lifecycle. Это надо решить до полноценной реализации `cli::in(...):str` и `push` структур, содержащих строки.
