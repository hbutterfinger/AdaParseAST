# AdaParseAST Custom Language Parser

AdaParseAST is a recursive descent parser and semantic analyzer for a simplified Ada-inspired programming language written in Python. The project tokenizes source code, constructs an Abstract Syntax Tree (AST), and performs semantic validation including scope resolution and static type checking.

## Features

This project implements a parser for a custom imperative language with familiar control-flow and expression syntax. It is designed to:

- Recursive descent parser implementation
- Integrated lexer/tokenizer
- AST generation for all supported language constructs
- Semantic analysis with scoped symbol tables
- Static type checking for expressions and assignments
- Declaration-before-use validation
- Redeclaration prevention within the same scope
- Nested scope handling for conditional statements and loops
- Support for arithmetic, logical, and comparison expressions
- Error handling using semantic validation rules

## Supported Language Constructs

### Statements
- Variable Declaration
- Assignment
- `Put(...)` output statement
- `if ... then ... else ... end if`
- `while ... loop ... end loop`
- `for ... in ... .. ... loop ... end loop`

### Expressions
- Integer literals
- Boolean literals: `True`, `False`
- Identifiers
- Parenthesized expressions
- Arithmetic operators: `+`, `-`, `*`, `/`, `mod`
- Comparison operators: `=`, `/=`, `<`, `>`, `<=`, `>=`
- Logical operators: `and`, `or`

## Semantic Analysis

### Declaration Before Use

Checks for declaration before use using the `var` keyword. 
```
var x : Integer := 10;
var flag : Boolean := True;
```

### No Redeclaration in the Same Scope

Invalid:
```
var x : Integer;
var x : Integer;
```

### Static Type Checking

Assignments and expressions must use compatible types.

Invalid:
```
var x : Integer;
x := True;
```

### Boolean Conditions

Conditions for if and while statements must evaluate to Boolean.

Invalid:
```
if 5 then
    Put(5);
end if;
```

## Grammar Summary

```ebnf
program      ::= block-stmt
block-stmt   ::= stmt*
stmt         ::= assign-stmt
               | put-stmt
               | if-stmt
               | loop-stmt
               | decl-stmt

decl-stmt    ::= ‘var’ id ‘:’ type (‘:=’ expr)? ‘;’
type         ::= ‘Integer’ | ‘Boolean’
assign-stmt  ::= id ":=" expr ";"
put-stmt     ::= "Put" "(" expr ")" ";"
if-stmt      ::= "if" expr "then" block-stmt ("else" block-stmt)? "end" "if" ";"
loop-stmt    ::= while-loop | for-loop
while-loop   ::= "while" expr "loop" block-stmt "end" "loop" ";"
for-loop     ::= "for" id "in" expr ".." expr "loop" block-stmt "end" "loop" ";"

expr         ::= conjunction ("or" conjunction)*
conjunction   ::= comparison ("and" comparison)*
comparison    ::= term (( "=" | "/=" | "<" | ">" | "<=" | ">=" ) term)?
term          ::= factor (( "+" | "-" ) factor)*
factor        ::= primary (( "*" | "/" | "mod" ) primary)*
primary       ::= integer | bool | id | "(" expr ")"
```

## Output

For valid input programs, the parser prints the formatted AST as text. The exact node layout and formatting follow the project’s AST node definitions, so the output can be used for testing and verification.
For invalid inpuit programs, if the program violates type or scoping rules, the exact string "Invalid" is returned. 

## Compatibility

- Compatible with Python 3.10 or lower
- No external packages are required

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/hbutterfinger/AdaParseAST.git
cd AdaParseAST
```

### 2. Run Verification Tests

```
python Verify.py
```
## Example Program

### Input

```
var x : Integer := 10;
var check : Boolean := False;

if x > 0 then
    check := True;
end if;

while x > 0 loop
    Put(x);
    x := x - 1;
end loop;
```

### Output

The program prints the AST representation for the full block, including the assignment nodes, the conditional node, and the nested expressions (invalid grammar outputs "Invalid"). 
```
Block([
    Decl(Identifier(x), Type(Integer), Integer(10)),
    Decl(Identifier(check), Type(Boolean), Boolean(False)),
    If(...),
    WhileLoop(...)
])
```


## Acknowledgments

This project is developed as part of a compiler/parser design project focused on recursive descent parsing and semantic analysis in Python.


## Contributing

 If you extend the language, consider updating:

- the grammar summary,
- example programs,
- AST output documentation,
- any parser tests or fixtures.


## License

This project is licensed under the [MIT License](LICENSE). 

Copyright (c) 2026 hbutterfinger
