# Общая грамматика

Грамматика записана в человекочитаемом EBNF-like виде. Это черновик для parser-а, а не финальная machine grammar.

## Source file

```ebnf
source_file ::= top_level_item* EOF

top_level_item ::= function_decl
                 | binding_decl
                 | module_decl
                 | import_decl
                 | foreign_block
                 | NEWLINE
```

На верхнем уровне `let` запрещён.

## Binding

```ebnf
binding_decl ::= "const" binding_list
               | binding_list

binding_list ::= binding ("," binding)*

binding ::= identifier type_annotation? initializer?

type_annotation ::= ":" type
initializer     ::= "=" expression
```

На верхнем уровне binding без `const` считается compile-time/constant binding.

## Function

```ebnf
function_decl ::= identifier parameter_list ":" type function_body?

parameter_list ::= "(" parameter_items? ")"
parameter_items ::= parameter ("," parameter)*

parameter ::= parameter_modifier? identifier ":" type
parameter_modifier ::= "const" | "yield"

function_body ::= block
```

Функция без тела — prototype/header declaration.

## Module

```ebnf
module_decl ::= identifier "=" module_expr
module_expr ::= "module" "{" module_item* "}"

module_item ::= function_decl
              | binding_decl
              | module_decl
              | state_decl
              | NEWLINE

state_decl ::= "state" binding_list
```

## Struct

```ebnf
struct_expr ::= "struct" "{" struct_item* "}"

struct_item ::= struct_field
              | nested_type_binding
              | function_decl
              | NEWLINE

struct_field ::= identifier ":" type default_initializer? ","?
default_initializer ::= "=" expression

nested_type_binding ::= identifier "=" type_constructor_expr ","?
```

## Enum

```ebnf
enum_expr ::= "enum" enum_backing_type? "{" enum_variant_list? "}"
enum_backing_type ::= ":" type

enum_variant_list ::= enum_variant ("," enum_variant)* ","?
enum_variant ::= identifier enum_value?
enum_value ::= "=" expression
```

## Type descriptors

`type {}` пока описывается минимально. Подробности в главе про type descriptors.

```ebnf
type_expr ::= "type" "{" type_item* "}"

type_item ::= descriptor_assignment
            | descriptor_section
            | function_decl
            | binding_decl
            | NEWLINE

descriptor_assignment ::= identifier "=" expression

descriptor_section ::= identifier "{" descriptor_entry* "}"

descriptor_entry ::= identifier "=" expression NEWLINE?
```

Пример:

```lena
i32 = type {
    storage = builtin::int(32, signed = true)

    foreign {
        llvm = @LLVM.i32
        c = "int32_t"
    }
}
```

## Statements

```ebnf
statement ::= block
            | local_binding_stmt
            | assignment_stmt
            | return_stmt
            | if_stmt
            | while_stmt
            | for_stmt
            | generator_stmt
            | secure_stmt
            | expression_stmt
            | NEWLINE

block ::= "{" statement* "}"

local_binding_stmt ::= ("let" | "const") binding_list
```

## Expressions

```ebnf
expression ::= logical_or_expr
```

Подробные приоритеты описаны в главе про выражения.
