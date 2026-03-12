
# 🔧 Mini Compiler

A mini compiler built in C using **Flex** (lexical analysis) and **Bison/YACC** (parsing), covering two core compiler back-end phases:

- **AST** — Abstract Syntax Tree construction and preorder traversal
- **ICG** — Intermediate Code Generation using quadruple (quad) representation

---

## 📁 Project Structure

```
Mini-Compiler/
├── AST/                        # Abstract Syntax Tree module
│   ├── lexer.l                 # Flex lexer specification
│   ├── parser.y                # Bison/YACC grammar + AST builder
│   ├── abstract_syntax_tree.c  # AST node creation & traversal
│   ├── abstract_syntax_tree.h  # AST struct & function declarations
│   ├── run.sh                  # Build script
│   ├── test_input1.c           # if statement test
│   ├── test_input2.c           # if-else statement test
│   └── test_input3.c           # nested if + sequence test
│
└── ICG/                        # Intermediate Code Generation module
    ├── lexer.l                 # Flex lexer specification
    ├── parser.y                # Bison/YACC grammar + quad generator
    ├── quad_generation.c       # Quad table emission, temp & label generation
    ├── quad_generation.h       # ICG extern declarations
    ├── run.sh                  # Build script
    ├── test_input1.c           # if statement test
    ├── test_input2.c           # if-else statement test
    └── test_input3.c           # while loop test
```

---

## ✨ Features

### AST Module
- Parses a C-like language with assignments, arithmetic expressions, and `if` / `if-else` control flow
- Builds a ternary (left / middle / right) expression tree
- Outputs a **preorder traversal** of the AST to stdout
- Reports syntax errors with line numbers

### ICG Module
- Extends the grammar to support `if`, `if-else`, `while`, and `do-while` constructs
- Emits a **quadruple table** — `(op, arg1, arg2, result)` — for every operation
- Automatically generates temporary variables (`t1`, `t2`, …) and jump labels (`L1`, `L2`, …)
- Handles relational operators (`<`, `>`, `<=`, `>=`, `==`, `!=`) and short-circuit control flow

---

## 🛠️ Prerequisites

| Tool | Purpose |
|------|---------|
| `flex` | Lexical analyser generator |
| `bison` / `yacc` | Parser generator |
| `gcc` | C compiler |

Install on Ubuntu/Debian:

```bash
sudo apt-get install flex bison gcc
```

---

## 🚀 Getting Started

### Build & Run — AST

```bash
cd AST
chmod +x run.sh
./run.sh                        # compiles the project
./a.out < test_input1.c         # run with a test file
```

### Build & Run — ICG

```bash
cd ICG
chmod +x run.sh
./run.sh                        # compiles the project
./a.out < test_input1.c         # run with a test file
```

### What `run.sh` does

```bash
lex lexer.l          # generates lex.yy.c
yacc -d parser.y     # generates y.tab.c and y.tab.h
gcc -g y.tab.c lex.yy.c
# intermediate files are cleaned up automatically
```

---

## 📝 Supported Grammar

### Statements
```
S  → if ( C ) { SEQ }
   | if ( C ) { SEQ } else { SEQ }
   | while ( C ) { S }          ← ICG only
   | do { S } while ( C ) ;     ← ICG only
   | id = E ;
```

### Expressions
```
E  → E + T | E - T | T
T  → T * F | T / F | F
F  → ( E ) | id | number
C  → F relop F
```

### Relational operators
`<`  `>`  `<=`  `>=`  `==`  `!=`

---

## 💡 Example

**Input (`test_input2.c`)**
```c
if(a > b) {
    a = a + 1;
    b = b - 1;
} else {
    a = a - 1;
    b = b - 1;
}
```

**AST output (preorder)**
```
if-else,>,a,b,seq,=,a,+,a,1,=,b,-,b,1,seq,=,a,-,a,1,=,b,-,b,1
Valid syntax
```

**ICG quad table output**
```
-----------------------------------------------------
| op         | arg1       | arg2       | result     |
-----------------------------------------------------
| >          | a          | b          | t1         |
| if         | t1         |            | L1         |
| goto       |            |            | L2         |
| Label      |            |            | L1         |
| +          | a          | 1          | t2         |
| =          | t2         |            | a          |
| -          | b          | 1          | t3         |
| =          | t3         |            | b          |
| goto       |            |            | L3         |
| Label      |            |            | L2         |
| -          | a          | 1          | t4         |
| =          | t4         |            | a          |
| -          | b          | 1          | t5         |
| =          | t5         |            | b          |
| Label      |            |            | L3         |
-----------------------------------------------------
Valid syntax
```

---

## 🧠 How It Works

```
Source Code
     │
     ▼
┌──────────┐     tokens      ┌──────────┐
│  Lexer   │ ─────────────▶  │  Parser  │
│ (Flex)   │                 │ (Bison)  │
└──────────┘                 └────┬─────┘
                                  │
                    ┌─────────────┴──────────────┐
                    ▼                            ▼
             ┌────────────┐             ┌──────────────┐
             │  AST Build │             │  Quad  ICG   │
             │ & Traverse │             │  Generation  │
             └────────────┘             └──────────────┘
```

1. **Lexer** tokenises identifiers, numbers, operators, and keywords.
2. **Parser** validates the token stream against the grammar.
3. **AST module** constructs a tree of `expression_node` structs and prints a preorder traversal.
4. **ICG module** walks the grammar actions and emits a flat quad table with auto-generated temporaries and labels.

---

## 📚 Concepts Demonstrated

- Lexical analysis with regular expressions (Flex)
- Bottom-up LALR(1) parsing (Bison/YACC)
- Abstract Syntax Tree construction in C
- Intermediate code generation — quadruple form
- Temporary variable and jump-label management
- Error reporting with line numbers (`yylineno`)

---

