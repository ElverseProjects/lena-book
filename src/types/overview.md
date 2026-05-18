# Обзор системы типов
## Стандартные типы

Типы-конструкторы:

```text
type {}   - дескриптор типа.
enum {}   - перечисление.
struct {} - структура.
module {} - модуль.
```

Примитивы образованные из дескриптора type:
```text
nil - отсутствие значения.
bool - булевое значение.
i8 i16 i32 i64 - целочисленный знаковый тип.
u8 u16 u32 u64 - целочисленный беззнаковый тип.
f32 f64 - числа с плавающей точкой.
char - unicode 32-битный символ.
T[] - статический массив образованный из любого типа-конструктора.
T[N] - статический массив заданной длины образованный из любого типа-конструктора.
T[*] - динамический массив образованный из любого типа-конструктора.
T[][] ... - аналогично для многомерных массивов.
```

Предварительные репрезентации типов:
```text
int - все целочисленные значения.
float - все значения с плавающей точкой.
```
## Примеры использования типов-конструкторов

Создание типа:
```rust
i32 = type {
    repr = @LLVM.int(bits = 32, signed = true)
}
```

Отличием `type {}` от `struct {}` является невозможность создавать несколько полей содержащих значения. Может быть только один накопитель. 

Создание перечисления:
```rust
langs = enum {
    LENA, JULIA, C, CPP, RUST,
}

main():nil {
    my_lang:langs = ::LENA
    return nil
}

```

Создание структуры со значениями по умолчанию:
```rust
dog = struct {
    name:str = "Barbos",
    age:u8   = 3,
    breed:enum{DOG, DOG2} = ::DOG, # Анонимный тип enum{}
}

main():nil {
    my_dog:dog = {
        .name = "Barbos2",
        .age = 2,
        .breed = DOG2
    }

    if (my_dog.age >= 1)
        cli::out("This is not puppy anymore")

    return nil
}

```

Создание модуля:
```rust
## robot.le
robot = module {

    eye_dist = type {
        repr = @LLVM.int(bits = 8, signed = false)
        const ENEMY_DIST:self = 120
    }

    direction = enum {
        FORWARD, BACKWARD, RIGHT, LEFT
    }

    sensors = struct {
        eye1:eye_dist,
        eye2:eye_dist
    }

    extern go(dir:direction):nil
    extern forward(speed1:i32, speed2:i32):nil
    enemy_is_here(sns:sensors):bool {
        return (sns.eye1 <= eye_dist::ENEMY_DIST or sns.eye2 <= eye_dist::ENEMY_DIST)
    }
}

## robot_1.le

impl robot {
    go(dir:direction):nil {
        ## do something
    }
    forward(speed1:i32, speed2:i32):nil {
        ## do something
    }
}

```

## Функции

Определение функции производиться без использования дополнительных ключевых слов, а на уровне грамматики.

```rust
my_function():nil {
    cli::out("This is my function!\n");
}

main():nil {
    ## Вызов функции
    for i = .1..10 by 1 {
        cli::out("Calling {@i}: ");
        my_function()
    }
}
```

```rust
my_function():nil {
    cli::out("This is my function!\n");
}
```
