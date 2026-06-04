# Введение

Lena — язык программирования со встроенной поддержкой мультиязычности, основанный на LLVM. Позволяет вызывать функции из библиотек других языков программирования которые входят в LLVM типы.

Главная идея Lena — дать низкоуровневую предсказуемость, гибкость и выразительность для реализации кода от задач embedded устройств, до высокоуровневых приложений.

Философия Lena предполагает максимизацию compiled-time обработки высокоуровневых составляющих языка.

Основная информация:

- строгая статическая типизация;
- явное понижение к LLVM;
- типы как compile-time сущности;
- явная модель владения через `yield`;
- строгие providers для внешних миров: `@LLVM`, `@C`, `@Rust`, `@Julia`, `@Lena`;
- мультиплатформенность: от приложений до no-OS embedded-профилей;


Минимальная программа:

```rust
main():nil {
    nil # or return nil
}
```

Привет мир:

```rust
main():exit_status {
    cli::out("Hello World!\n")
    return ::SUCCESS
}
```

Пример мультиязычности:

```rust
from @Julia import "Flux" as Flux
from @Rust import "Regex" as Regex

@Julia {
    using Lena

    @lena function func1(x::Lena.I64, y::Lena.I64)::Lena.I64
        return x + (y * x) ÷ 2
    end
}

@Rust {
    use Lena;
    use regex::Regex;

    // in Lena is contains_digits(text:str):bool
    pub fn contains_digits(text: &str) -> bool { 
        Regex::new(r"\d+").unwrap().is_match(text)
    }
}

main():exit_status {

    let jl_f_result:i64 = @Julia.f(100, 2000)

    let jl_sigmoid:f64 = Flux.sigmoid(100) # aka @Julia.Flux.sigmoid(100)

    cli::out("Julia result = {@result}\n")

    let some_str:str = "Lena, Lina, 2008\n";
    let rust_result = @Rust.contains_digits(some_str)

    return ::SUCCESS
}
```

Providers, что используют JIT, и внешний рантайм, не поддерживаются на embedded платформах (CortexM, AVR и др.). Более подробно это описывается в разделе ... .

