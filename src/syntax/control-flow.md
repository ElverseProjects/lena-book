# Управляющие конструкции и циклы

## if / else

```lena
if (a == b) {
    cli::out("equal\n")
} else if (a > b) {
    cli::out("greater\n")
} else {
    cli::out("less\n")
}
```

Condition должен быть `bool`.

## while

```lena
let i = 0

while (i on 0..1000) {
    cli::out("i = {@i++}\n")
}
```

`on` проверяет попадание в range bounds.

## Ranges and sequences

```lena
0..100      # inclusive range object
0...100     # exclusive-end range object
.0..100     # materialized/generated sequence 0..100
.0...100    # materialized/generated sequence 0..99
```

`0..100` сам по себе не обязан быть массивом. Это range object. В разных контекстах он может использоваться как bounds или как iterable source.

## for

`for` в Lena — это не C-style loop. Это итерация по источнику с шагом.

```lena
for item = source by step {
    ...
}
```

`source` может быть:

```text
- range object: 0...1000
- generated sequence: .0..100
- static array
- dynamic array
- custom iterable в будущем
```

Пример:

```lena
for i = 0...1000 by 1 {
    cli::out("i = {@i}\n")
}
```

Это означает: пройти по виртуальной последовательности `0, 1, ..., 999` с шагом по элементам `1`.

Если шаг `2`:

```lena
for i = 0...1000 by 2 {
    cli::out("i = {@i}\n")
}
```

то будут взяты элементы `0, 2, 4, ...`.

Для массива:

```lena
let array:i32[] = {10, 30, 32, 235, 65, 83}

for i = array by 2 {
    cli::out("i = {@i}\n")
}
```

Выводит элементы с индексами `0, 2, 4`:

```text
10
32
65
```

Компилятор может раскрывать range iteration в эффективный counter-loop без материализации массива.

## Generator statement

```lena
.1..1000 -> i {
    cli::out("i = {@i}\n")
}
```

Это сахар для итерации по source с шагом `1`.
