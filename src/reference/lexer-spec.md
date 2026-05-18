# Lexer spec

Эта глава — краткое ТЗ для токенизатора Lena v0.1.

## Requirements

1. ASCII identifiers only:

```text
[A-Za-z_][A-Za-z0-9_]*
```

2. Keywords are case-sensitive.

3. `NEWLINE` must be emitted as token.

4. Comments:

```text
# line comment
#= block comment =#
```

Block comments are not nested in v0.1.

5. Integer literals:

```text
123
1_000
0b1010_1100
0xFF_A0
```

6. Decimal literals:

```text
digits '.' digits exponent?
```

Allowed:

```lena
10.2
1.0e-5
```

Disallowed in v0.1:

```lena
.5
10.
1e10
```

7. Strings are single tokens. Interpolation is parsed later.

8. No char literals in v0.1.

9. Operators:

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

10. Logical words:

```text
and or not
```

No `&&`, `||`, or logical `!`.

## Foreign block tokenization

If lexer/parser sees:

```lena
@Ident {
    raw source
}
```

it should produce/store a foreign block with:

```text
language = Ident
raw_source = text between braces
```

The raw source is not lexed as Lena.

If no `{` follows `@Ident`, tokenize normally:

```lena
@Julia.f
@LLVM.i32
```

as:

```text
AT IDENT DOT IDENT
```
