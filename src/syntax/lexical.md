# Лексическая структура

Эта глава описывает токены Lena v0.1.

## Идентификаторы

В v0.1 идентификаторы только ASCII:

```ebnf
identifier ::= [A-Za-z_][A-Za-z0-9_]*
```

Примеры:

```lena
main
exit_status
client_last_name
GPIOA_ODR
```

Ключевые слова чувствительны к регистру:

```lena
let  # keyword
Let  # identifier
```

## Ключевые слова

```text
let const yield return
if else while for by in on secure
struct enum module type state
from import as
and or not
true false nil
```

`storage`, `foreign`, `rules` и похожие имена не являются ключевыми словами. Это обычные identifiers, смысл которых задаётся семантикой внутри `type {}`.

## Newline

`NEWLINE` является токеном. Lena не требует `;`, поэтому перенос строки участвует в разделении statements.

Парсер может игнорировать `NEWLINE` внутри `(`, `{`, `[`.

## Комментарии

Однострочный комментарий:

```lena
# comment
```

Блочный комментарий:

```lena
#=
  block comment
=#
```

В v0.1 блочные комментарии не вложенные.

## Числа

Integer literals:

```lena
100
1_000_000
0b1100_0011
0xFF_A0
```

Подчёркивания разрешены внутри числа и игнорируются.

Отрицательное число — это unary `-` плюс положительный literal:

```lena
-100
```

лексится как:

```text
MINUS INT(100)
```

Decimal literals в v0.1 строго имеют форму `digits "." digits`:

```lena
10.2
1.0e-5
```

Не разрешены в v0.1:

```lena
.5
10.
1e10
```

Это сделано, чтобы не конфликтовать с sequence marker `.0..100`.

## Строки

Строки лексируются одним токеном `STRING_LITERAL`.

```lena
"Hello World!\n"
"Piano: {@p.name}\n"
"You put: {0}"
```

Интерполяция внутри строки не разбирается lexer-ом. Её разбирает отдельный format-string pass.

Минимальные escape-последовательности:

```text
\n \r \t \\ \" \0 \xNN
```

Raw newline внутри обычной строки запрещён.

## Char literals

В v0.1 отдельного `char` literal нет.

Для байтов используется `u8`, для текста — `str`. Позже можно добавить `b'a'` и Unicode `char` отдельно.

## Операторы и разделители

```text
+ - * / %
= == !=
< <= > >=
& | ^ ~
<< >> <<= >>=
+= -= *= /= %= &= |= ^=
:
::
.
..
...
,
(
)
{
}
[
]
->
@
!
```

Логические операторы только словесные:

```lena
and or not
```

`&&`, `||` и `!` как logical not не используются. `!` зарезервирован для fallible types/operations вроде `nil!` и `T!`.

## `@` и foreign blocks

`@` означает foreign/backend/compiler namespace.

```lena
@Julia
@Rust
@C
@LLVM
@Lena
```

Если lexer видит:

```lena
@Julia {
    ... raw source ...
}
```

то это foreign block. Его содержимое не лексится как Lena. Lexer или parser должны сохранить:

```text
language = "Julia"
raw_source = "..."
```

Если после `@Ident` нет `{`, это обычный foreign path:

```lena
@Julia.f(100, 2000)
@LLVM.i32
@Lena.version
```
