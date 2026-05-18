# Система сборки

Lena должна иметь одну основную команду:

```bash
lena build
lena run
lena check
lena fmt
lena doc
lena test
```

Не нужно заставлять пользователя писать CMake.

## Project config

Практичный старт — `lena.toml`.

```toml
[package]
name = "robot-control"
version = "0.1.0"
edition = "2026"

[build]
entry = "src/main.le"
output = "robot-control"

[profile.dev]
opt_level = 0
debug = true
checks = true

[profile.release]
opt_level = 3
debug = false
checks = false

[target.host]
profile = "host"
providers = ["LLVM", "C", "Rust", "Julia"]

[target.cortex_m4]
triple = "thumbv7em-none-eabi"
cpu = "cortex-m4"
profile = "embedded"
providers = ["LLVM", "C"]
heap = false
filesystem = false
```

## Commands

```bash
lena new app robot-control
lena check
lena build
lena build --release
lena build --target cortex_m4
lena run
lena test
lena doc
lena provider list
lena backend show LLVM
lena target show cortex_m4
```

## Future

Позже можно добавить `build.le` для сложной программируемой сборки на самой Lena.
