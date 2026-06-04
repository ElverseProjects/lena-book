
# Lena-API

Для поддержки совместимости между двумя языками, существует модуль `Lena.jl`:

```julia
using Lena
```

Для того чтобы функция могла быть вызвана в Lena, необходимо, чтобы её аргументы и возвращаемое значение являлись Lena-совместимыми, и использовался макрос `@lena`. Список поддерживаемых типов, реализуемых в `Lena.jl`:

```julia
# Lena.jl
# module Lena
const I8  = Int8
const I16 = Int16
const I32 = Int32
const I64 = Int64
const U8  = UInt8
const U16 = UInt16
const U32 = UInt32
const U64 = UInt64
const F32 = Float32
const F64 = Float64
const LBool = UInt8
# ...
```

Пример создания функции:

```julia
using Lena

# Создание Лена-совместимой функции
@lena function MyLenaFunction(arg1::Lena.I8, arg2::Lena.F64)::Lena.F64
    return (arg1 + arg2) / 3.14
end

```

Пример подключения модуля:

```julia

from @Julia import "ChemEquations" as Chem

main():nil {

    let source:str = "BaCl2(aq) + Na2SO4(aq) -> BaSO4(s) + NaCl(aq)"

    let equation = Chem::parse(source)
    let balanced = Chem::balance(equation)

    cli::out("@{Chem::format(balanced)}")

    nil
}

```

