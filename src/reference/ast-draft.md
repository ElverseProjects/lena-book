# AST draft

Минимальные категории AST для Lena v0.1.

## Program

```text
Program
  declarations: Decl[]
```

## Declarations

```text
FunctionDecl
BindingDecl
ModuleDecl
ForeignBlock
ImportDecl
```

## Statements

```text
BlockStmt
LocalBindingStmt
AssignStmt
ReturnStmt
IfStmt
WhileStmt
ForStmt
GeneratorStmt
SecureStmt
ExprStmt
```

## Expressions

```text
IntegerLiteral
DecimalLiteral
BoolLiteral
StringLiteral
NilLiteral
PathExpr
CallExpr
MethodCallExpr
FieldExpr
IndexExpr
CastExpr
UnaryExpr
BinaryExpr
RangeExpr
SequenceExpr
ArrayLiteral
StructLiteral
StructConstructorExpr
EnumConstructorExpr
ModuleExpr
TypeDescriptorExpr
YieldExpr
PostfixIncExpr
PostfixDecExpr
```

## Types

```text
PathType
StaticArrayType
DynamicArrayType
PointerType
FunctionType
```

## Important notes

- `T[*]` is a dynamic array type constructor.
- `T[N]` and `T[]` are static array type constructors.
- `str` is a type, not a module.
- `@Ident { ... }` stores raw foreign source.
- `for item = source by step` is iterable-source loop, not C-style loop.
