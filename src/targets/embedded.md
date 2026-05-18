# Multiplatform и embedded

Lena должна поддерживать разные профили исполнения.

```text
Lena Host
    filesystem, heap, dynamic arrays, CLI, Julia/Rust/C providers.

Lena Embedded
    no OS, AVR/Cortex-M, MMIO, C ABI, optional/no heap.

Lena Robotics
    сенсоры, моторы, control loops, связь host <-> firmware.
```

## Cortex-M / AVR

Для embedded нельзя предполагать наличие:

```text
- filesystem
- Julia runtime
- GraalVM
- heap
- threads
- stdout/stderr в обычном смысле
```

Допустимы:

```text
- @C provider
- @LLVM backend
- @Lena self namespace
- MMIO
- static arrays
- fixed memory
- optional allocator
```

## Pointers and MMIO

Raw pointers нужны для embedded и FFI, но должны быть системным слоем.

Возможный будущий синтаксис:

```lena
*p
*const T
*volatile T
```

MMIO может выглядеть так:

```lena
GPIOA_ODR = @MMIO(*volatile u32, 0x48000014)
```

или через typed register abstraction.

## Target profiles

Будущий target profile может описывать:

```text
arch = arm / avr / x86_64
cpu = cortex-m4 / atmega328p / ...
abi = eabi / avr / system-v
os = none / linux / windows / macos
heap = true/false
filesystem = true/false
providers = [LLVM, C, Julia, Rust]
```

Если код использует недоступный provider:

```lena
from @Julia import "Flux" as Flux
```

на Cortex-M, компилятор должен дать ошибку до codegen.
