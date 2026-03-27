
# 🔧 Mini Compiler

<div align="center">

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Language](https://img.shields.io/badge/language-C-orange.svg)
![Build](https://img.shields.io/badge/build-passing-brightgreen.svg)
![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)

**A pedagogical mini-compiler demonstrating fundamental compiler design principles**

[Features](#-features) • [Installation](#-installation) • [Quick Start](#-quick-start) • [Documentation](#-documentation) • [Examples](#-examples)

</div>

---

## 📋 Table of Contents

- [Overview](#-overview)
- [Features](#-features)
- [Architecture](#-architecture)
- [Project Structure](#-project-structure)
- [Installation](#-installation)
- [Quick Start](#-quick-start)
- [Detailed Usage](#-detailed-usage)
- [Grammar Specification](#-grammar-specification)
- [Examples](#-examples)
- [How It Works](#-how-it-works)
- [Concepts Demonstrated](#-concepts-demonstrated)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🎯 Overview

**Mini Compiler** is an educational compiler implementation built in C that demonstrates the core back-end phases of compiler design. It uses **Flex** for lexical analysis and **Bison/YACC** for parsing, covering two fundamental compilation stages:

1. **Abstract Syntax Tree (AST)** Construction and Traversal
2. **Intermediate Code Generation (ICG)** using Quadruple Representation

This project is ideal for computer science students and enthusiasts looking to understand how compilers work under the hood.

---

## ✨ Features

### 🌳 AST Module

- ✅ Parses a C-like language with:
  - Variable assignments
  - Arithmetic expressions (`+`, `-`, `*`, `/`)
  - Conditional statements (`if`, `if-else`)
- ✅ Constructs a **ternary expression tree** (left/middle/right child nodes)
- ✅ Outputs **preorder traversal** of the AST
- ✅ Comprehensive error reporting with **line numbers**
- ✅ Validates syntax against defined grammar rules

### 🔄 ICG Module

- ✅ Extended grammar support:
  - `if` and `if-else` conditionals
  - `while` loops
  - `do-while` loops
- ✅ Generates **quadruple (quad) representation**:
  - Format: `(operator, arg1, arg2, result)`
- ✅ **Automatic resource management**:
  - Temporary variable generation (`t1`, `t2`, `t3`, ...)
  - Jump label generation (`L1`, `L2`, `L3`, ...)
- ✅ Supports relational operators: `<`, `>`, `<=`, `>=`, `==`, `!=`
- ✅ Implements **short-circuit control flow** evaluation
- ✅ Produces human-readable quad tables

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      SOURCE CODE                             │
│                   (C-like language)                          │
└───────────────────────┬─────────────────────────────────────┘
                        │
                        ▼
            ┌───────────────────────┐
            │   LEXICAL ANALYZER    │
            │       (Flex)          │
            │  • Tokenization       │
            │  • Pattern Matching   │
            └──────────┬────────────┘
                       │ Token Stream
                       ▼
            ┌───────────────────────┐
            │    SYNTAX ANALYZER    │
            │     (Bison/YACC)      │
            │  • Grammar Validation │
            │  • Parse Tree Build   │
            └──────────┬────────────┘
                       │
         ┌─────────────┴─────────────┐
         ▼                           ▼
┌─────────────────┐         ┌─────────────────┐
│   AST MODULE    │         │   ICG MODULE    │
├─────────────────┤         ├─────────────────┤
│ • Tree Building │         │ • Quad Gen      │
│ • Node Creation │         │ • Temp Vars     │
│ • Traversal     │         │ • Label Gen     │
│ • Preorder Out  │         │ • Control Flow  │
└─────────────────┘         └─────────────────┘
         │                           │
         ▼                           ▼
    AST Output              Quadruple Table
```

---

## 📁 Project Structure

```
Mini-Compiler/
│
├── AST/                              # Abstract Syntax Tree Module
│   ├── lexer.l                       # Flex lexer specification
│   ├── parser.y                      # Bison/YACC grammar + AST builder
│   ├── abstract_syntax_tree.c        # AST node creation & traversal logic
│   ├── abstract_syntax_tree.h        # AST structure & function declarations
│   ├── run.sh                        # Build automation script
│   ├── test_input1.c                 # Test: if statement
│   ├── test_input2.c                 # Test: if-else statement
│   ├── test_input3.c                 # Test: nested if + sequences
│   ├── commands.png                  # Screenshot: build commands
│   ├── output 1.png                  # Screenshot: test output 1
│   ├── output 2.png                  # Screenshot: test output 2
│   └── output 3.png                  # Screenshot: test output 3
│
├── ICG/                              # Intermediate Code Generation Module
│   ├── lexer.l                       # Flex lexer specification
│   ├── parser.y                      # Bison/YACC grammar + quad generator
│   ├── quad_generation.c             # Quad table emission & management
│   ├── quad_generation.h             # ICG function & variable declarations
│   ├── run.sh                        # Build automation script
│   ├── test_input1.c                 # Test: if statement
│   ├── test_input2.c                 # Test: if-else statement
│   ├── test_input3.c                 # Test: while loop
│   ├── output1.png                   # Screenshot: test output 1
│   ├── output2.png                   # Screenshot: test output 2
│   └── output3.png                   # Screenshot: test output 3
│
└── README.md                         # Project documentation
```

---

## 🛠️ Installation

### Prerequisites

Ensure you have the following tools installed:

| Tool | Purpose | Installation Check |
|------|---------|-------------------|
| **Flex** | Lexical analyzer generator | `flex --version` |
| **Bison/YACC** | Parser generator | `bison --version` |
| **GCC** | GNU C Compiler | `gcc --version` |

### Installation Commands

#### Ubuntu / Debian
```bash
sudo apt-get update
sudo apt-get install flex bison gcc
```

#### Fedora / RHEL / CentOS
```bash
sudo dnf install flex bison gcc
```

#### macOS (using Homebrew)
```bash
brew install flex bison gcc
```

#### Verify Installation
```bash
flex --version
bison --version
gcc --version
```

---

## 🚀 Quick Start

### Option 1: AST Module

```bash
# Navigate to AST directory
cd AST

# Make the build script executable
chmod +x run.sh

# Compile the project
./run.sh

# Run with test input
./a.out < test_input1.c
```

### Option 2: ICG Module

```bash
# Navigate to ICG directory
cd ICG

# Make the build script executable
chmod +x run.sh

# Compile the project
./run.sh

# Run with test input
./a.out < test_input1.c
```

---

## 📖 Detailed Usage

### Building from Source

The `run.sh` script automates the compilation process:

```bash
#!/bin/bash

# Step 1: Generate lexical analyzer
lex lexer.l              # Creates lex.yy.c from lexer specification

# Step 2: Generate parser
yacc -d parser.y         # Creates y.tab.c and y.tab.h from grammar

# Step 3: Compile everything
gcc -g y.tab.c lex.yy.c  # Compiles and links all components

# Step 4: Cleanup intermediate files
rm -f lex.yy.c y.tab.c y.tab.h
```

### Manual Compilation

If you prefer manual control:

```bash
# For AST module
cd AST
lex lexer.l
yacc -d parser.y
gcc -g y.tab.c lex.yy.c abstract_syntax_tree.c -o ast_compiler
./ast_compiler < test_input1.c

# For ICG module
cd ICG
lex lexer.l
yacc -d parser.y
gcc -g y.tab.c lex.yy.c quad_generation.c -o icg_compiler
./icg_compiler < test_input1.c
```

### Running Custom Input

Create your own test file:

```bash
# Create a new test file
cat > my_test.c << 'EOF'
if(x > 5) {
    y = x * 2;
    z = y + 3;
}
EOF

# Run the compiler
./a.out < my_test.c
```

---

## 📝 Grammar Specification

### Statement Grammar

```
START → SEQ

SEQ   → S SEQ
      | S

S     → if ( C ) { SEQ }
      | if ( C ) { SEQ } else { SEQ }
      | while ( C ) { S }              ← ICG only
      | do { S } while ( C ) ;         ← ICG only
      | ASSGN

ASSGN → id = E ;
```

### Expression Grammar

```
E → E + T
  | E - T
  | T

T → T * F
  | T / F
  | F

F → ( E )
  | id
  | number
```

### Condition Grammar

```
C → F relop F

relop → < | > | <= | >= | == | !=
```

### Lexical Tokens

| Token | Pattern | Example |
|-------|---------|---------|
| `T_ID` | `[a-zA-Z][a-zA-Z0-9_]*` | `x`, `var1`, `temp_value` |
| `T_NUM` | `[0-9]+` | `42`, `100`, `0` |
| `IF` | `if` | `if` |
| `ELSE` | `else` | `else` |
| `WHILE` | `while` | `while` *(ICG only)* |
| `DO` | `do` | `do` *(ICG only)* |
| Relational | `<`, `>`, `<=`, `>=`, `==`, `!=` | `<`, `>=` |
| Arithmetic | `+`, `-`, `*`, `/` | `+`, `*` |

---

## 💡 Examples

### Example 1: Simple If Statement

**Input (`test_input1.c`)**
```c
if(a > b) {
    a = a + 1;
    b = b - 1;
}
```

**AST Output**
```
Preorder:
if,>,a,b,seq,=,a,+,a,1,=,b,-,b,1
Valid syntax
```

**ICG Quad Table Output**
```
ICG 
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
| Label      |            |            | L2         |
-----------------------------------------------------
Valid syntax
```

---

### Example 2: If-Else Statement

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

**AST Output**
```
Preorder:
if-else,>,a,b,seq,=,a,+,a,1,=,b,-,b,1,seq,=,a,-,a,1,=,b,-,b,1
Valid syntax
```

**ICG Quad Table Output**
```
ICG 
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

### Example 3: While Loop (ICG Only)

**Input (`test_input3.c`)**
```c
while(x < 10) {
    x = x + 1;
    y = y * 2;
}
```

**ICG Quad Table Output**
```
ICG 
-----------------------------------------------------
| op         | arg1       | arg2       | result     |
-----------------------------------------------------
| Label      |            |            | L1         |
| <          | x          | 10         | t1         |
| if         | t1         |            | L2         |
| goto       |            |            | L3         |
| Label      |            |            | L2         |
| +          | x          | 1          | t2         |
| =          | t2         |            | x          |
| *          | y          | 2          | t3         |
| =          | t3         |            | y          |
| goto       |            |            | L1         |
| Label      |            |            | L3         |
-----------------------------------------------------
Valid syntax
```

---

## 🧠 How It Works

### Compilation Pipeline

```
┌─────────────────────────────────────────────────────────┐
│  1. SOURCE CODE                                         │
│     • User writes C-like program                        │
│     • Contains if/else, while, assignments              │
└────────────────────┬────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│  2. LEXICAL ANALYSIS (Flex - lexer.l)                   │
│     • Scans input character by character                │
│     • Recognizes tokens using regex patterns            │
│     • Outputs: Token stream                             │
│       Example: "if(a > b)" → [IF, (, ID(a), >, ID(b), )]│
└────────────────────┬────────────────────────────────────┘
                     │ Token Stream
                     ▼
┌─────────────────────────────────────────────────────────┐
│  3. SYNTAX ANALYSIS (Bison - parser.y)                  │
│     • Validates token stream against grammar            │
│     • Builds parse tree bottom-up (LALR parsing)        │
│     • Triggers semantic actions on reduction            │
│     • Reports syntax errors with line numbers           │
└────────────────────┬────────────────────────────────────┘
                     │
          ┌──────────┴──────────┐
          │                     │
          ▼                     ▼
┌─────────────────────┐   ┌─────────────────────┐
│  4a. AST MODULE     │   │  4b. ICG MODULE     │
├─────────────────────┤   ├─────────────────────┤
│ • Create tree nodes │   │ • Generate quads    │
│ • Link parent/child │   │ • Manage temps      │
│ • Traverse preorder │   │ • Create labels     │
│ • Print structure   │   │ • Emit quad table   │
└─────────┬───────────┘   └──────────┬──────────┘
          │                          │
          ▼                          ▼
   AST Traversal            Quadruple IR Code
```

### Detailed Flow

1. **Tokenization (Lexer)**
   - Input: Raw source code string
   - Process: Pattern matching using regular expressions
   - Output: Stream of categorized tokens

2. **Parsing (Parser)**
   - Input: Token stream from lexer
   - Process: Bottom-up LALR(1) shift-reduce parsing
   - Output: Parse tree + semantic actions triggered

3. **AST Construction**
   - Creates `expression_node` structures
   - Links nodes in ternary tree (left, middle, right)
   - Performs preorder traversal for output

4. **ICG Quad Generation**
   - Walks grammar reduction actions
   - Emits quadruples for each operation
   - Generates temporaries for intermediate results
   - Creates labels for control flow jumps

---

## 📚 Concepts Demonstrated

### Core Compiler Theory

- ✅ **Lexical Analysis**
  - Regular expressions for token recognition
  - Finite automata implementation via Flex
  - Token categorization and symbol table integration

- ✅ **Syntax Analysis**
  - Context-free grammar design
  - LALR(1) bottom-up parsing (Bison/YACC)
  - Shift-reduce conflict resolution
  - Error recovery and reporting

- ✅ **Abstract Syntax Trees**
  - Tree data structure in C
  - Node allocation and memory management
  - Tree traversal algorithms (preorder)
  - Recursive tree construction

- ✅ **Intermediate Code Generation**
  - Three-address code (quadruple form)
  - Temporary variable allocation
  - Label generation for control flow
  - Symbol table management

- ✅ **Control Flow Translation**
  - If-then-else translation
  - Loop translation (while, do-while)
  - Short-circuit boolean evaluation
  - Backpatching for forward references

### Software Engineering

- 🔧 **Modular Design**: Separation of lexer, parser, AST, and ICG
- 🔧 **Build Automation**: Shell scripts for compilation pipeline
- 🔧 **Testing**: Multiple test cases with expected outputs
- 🔧 **Error Handling**: Line number tracking with `yylineno`

---

## 🐛 Troubleshooting

### Common Issues

#### 1. `lex: command not found`

**Solution:**
```bash
sudo apt-get install flex
```

#### 2. `yacc: command not found`

**Solution:**
```bash
sudo apt-get install bison
```

#### 3. `syntax error at line X`

**Cause:** Input doesn't match the grammar

**Solution:**
- Check for missing semicolons
- Ensure braces `{}` are balanced
- Verify relational operators are correct
- Check that identifiers start with a letter

#### 4. `undefined reference to 'yywrap'`

**Cause:** Missing yywrap function

**Solution:** Already included in parser.y:
```c
int yywrap() {
    return 1;
}
```

#### 5. Compilation warnings about implicit declarations

**Solution:**
```bash
gcc -g y.tab.c lex.yy.c -o compiler -lfl
```

#### 6. Permission denied when running `./run.sh`

**Solution:**
```bash
chmod +x run.sh
```

---

## 🔬 Advanced Usage

### Debugging

#### Enable Debug Output in Bison
```bash
yacc -d -t parser.y  # -t enables debug mode
export YYDEBUG=1
./a.out < test_input.c
```

#### Enable Debug Output in Flex
```bash
flex -d lexer.l  # -d enables debug mode
```

#### Use GDB for Debugging
```bash
gcc -g y.tab.c lex.yy.c  # -g includes debug symbols
gdb ./a.out
(gdb) run < test_input.c
(gdb) break yyparse
(gdb) step
```

### Extending the Compiler

#### Add New Operators

1. Update `lexer.l` to recognize new tokens
2. Add token definitions in `parser.y`
3. Extend grammar rules
4. Implement semantic actions

Example: Adding modulo operator

```c
// In lexer.l
"%"         { return MOD; }

// In parser.y
T : T '%' F {
        $$ = new_temp();
        quad_code_gen($$, $1, "%", $3);
    }
```

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

### Reporting Bugs

1. Check if the issue already exists
2. Create a new issue with:
   - Descriptive title
   - Steps to reproduce
   - Expected vs actual behavior
   - Input code that causes the issue

### Suggesting Enhancements

- New language features
- Optimization techniques
- Better error messages
- Additional test cases

### Pull Requests

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests for new features
5. Submit a pull request


---

## 🎓 Educational Resources

### Recommended Reading

- **Compilers: Principles, Techniques, and Tools** (Dragon Book) - Aho, Lam, Sethi, Ullman
- **Modern Compiler Implementation in C** - Andrew W. Appel
- **Engineering a Compiler** - Cooper and Torczon

### Online Resources

- [Flex Manual](https://westes.github.io/flex/manual/)
- [Bison Manual](https://www.gnu.org/software/bison/manual/)
- [Compiler Design Tutorial - Tutorialspoint](https://www.tutorialspoint.com/compiler_design/)

---

## 🙏 Acknowledgments

- Built using **GNU Flex** and **GNU Bison**
- Inspired by classic compiler design textbooks
- Educational project for learning compiler construction

---

## 📧 Contact

For questions, suggestions, or collaboration:

- Open an issue on GitHub
- Contribute via pull requests

---

<div align="center">

**⭐ Star this repository if you found it helpful! ⭐**

Made with ❤️ for learning compiler design

</div>
