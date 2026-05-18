# Статус черновика

Это не финальная спецификация языка. Некоторые разделы являются решениями v0.1, некоторые — дизайн-заметками на будущее.

## В v0.1 считаем зафиксированным

- ASCII identifiers.
- `NEWLINE` является токеном, потому что в Lena нет обязательной точки с запятой.
- Комментарии `#` и `#= ... =#`.
- `let`, `const`, функции, `struct`, `enum`, `module`.
- Параметры функций:
  - `x:T` — изменяемая ссылка/borrow по умолчанию;
  - `const x:T` — readonly borrow;
  - `yield x:T` — передача владения.
- Динамические массивы `T[*]` как встроенный type constructor.
- Методы динамических массивов: `.len()`, `.cap()`, `.push()`, `.resize()`, `.free()`.
- Цикл `for item = source by step` как итерация по источнику с шагом.
- `@Ident { ... }` как raw foreign block.

## На будущее

- Полная система `type {}` descriptors.
- Подробные backend capability headers.
- Providers как shared libraries.
- Embedded target profiles для AVR/Cortex-M.
- GraalVM bridge.
- Расширенная система ошибок и `nil!`/`T!`.
- Matrix-типы отдельно от многомерных массивов.
