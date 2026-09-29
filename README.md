# Smart Calculator
A modular command-line calculator in Python with safe expression evaluation, persistent history, reporting, logging and unit tests.

## Overview
The project develops a simple menu calculator into an application with good software-engineering practice: layered modules, custom error handling, input validation, logging and automated tests.

The calculator uses three modules:
Module 1: Arithmetic operations: + -  / ^ % sqrt

Module 2: Expression evaluator: type 2 + 3 (4 - 1); parsed safely with ast (no eval)

Module 3: History & reports: saved to JSON, summary stats (count / average / min / max), CSV export, clear history
Various protections: division-by-zero, invalid input, overflow, and unsafe-code
Logging to logs/calculator.log
Bounded history (last 100 entries) for resource efficiency.
The project is implemented in Python 3.9+ (standard library only: ast, math, json, csv, logging, unittest), and uses Git for version control.

## Non-Functional Requirements Table
|Category |Implementation|
|---|---|
|Security |No eval; only numbers and arithmetic operators are allowed |
|Reliability |Corrupt history file is handled; app never crashes on bad input |
|Usability |Numbered menus, clear error messages, tidy number formatting |
|Maintainability |Small single-purpose modules, docstrings, config constants |
|Resource efficiency |History capped at 100 entries; exponent and expression length limits |
|Logging |Every operation and handled error is logged |

## Technologies Used
- Python 3.9+ (standard library only: ast, math, json, csv, logging, unittest)
- Git
  
## Project Structure
```
smart_calculator/
├── main.py         # entry point
├── calculator/
│  ├── __init__.py
│  ├── config.py      # constants
│  ├── exceptions.py    # custom exceptions
│  ├── logger.py      # logging setup
│  ├── validators.py    # input validation
│  ├── operations.py    # Module 1: arithmetic
│  ├── expression.py    # Module 2: safe evaluator
│  ├── history.py      # Module 3: history & reports
│  └── cli.py        # menu / workflow
├── tests/          # unit tests
├── docs/design.md      # architecture & UML diagrams
├── data/ logs/
├── statement.md
└── README.md
```

## Installation & Running
```bash
git clone
cd smart_calculator
python main.py
```
No packages need to be installed.
## Testing
```bash
python -m unittest discover -s tests -t . -v
```
## Sample Session
```
=== Smart Calculator ===
1. Basic Operation 2. Expression Evaluator 3. History & Reports 4. Exit
Enter choice (1-4): 2
Enter expression: 2 + 3 (4 - 1)
Result: 2 + 3 (4 - 1) = 11
```

## System Architecture
```mermaid
flowchart TB
    U[User] --> CLI[cli.py - Presentation layer]
    CLI --> V[validators.py]
    CLI --> OPS[operations.py - Module 1]
    CLI --> EXP[expression.py - Module 2]
    CLI --> HIS[history.py - Module 3]
    EXP --> OPS
    EXP --> V
    HIS --> FS[(history.json / CSV)]
    CLI --> LOG[logger.py]
    HIS --> LOG
    LOG --> LF[(calculator.log)]
    OPS --> EXC[exceptions.py]
    EXP --> EXC
    CFG[config.py] -.-> OPS & HIS & LOG
```

## Workflow Diagram
```mermaid
flowchart TD
    A([Start]) --> B[Show main menu]
    B --> C{Choice}
    C -->|1| D[Select operation, enter numbers]
    C -->|2| E[Enter expression]
    C -->|3| F[View / Summary / Export / Clear]
    C -->|4| Z([Exit])
    D --> G{Valid?}
    E --> G
    G -->|No| H[Show error and log it] --> B
    G -->|Yes| I[Compute result]
    I --> J[Display and save to history] --> B
    F --> B
```
## Use Case Diagram
```mermaid
flowchart LR
    User((User))
    subgraph Smart Calculator
    UC1[Perform basic operation]
    UC2[Evaluate expression]
    UC3[View history]
    UC4[View summary]
    UC5[Export CSV]
    UC6[Clear history]
    end
    User --> UC1 & UC2 & UC3 & UC4 & UC5 & UC6
```

## Class / Component Diagram
```mermaid
classDiagram
    class HistoryManager {
        +path
        +entries
        +add(expression, result)
        +get_all()
        +clear()
        +save()
        +load()
        +export_csv(path)
        +summary()
    }
    class CalculatorError
    CalculatorError <|-- InvalidInputError
    CalculatorError <|-- DivisionByZeroError
    CalculatorError <|-- UnknownOperationError
    CalculatorError <|-- ExpressionError
    class Operations {
        +add() +subtract() +multiply()
        +divide() +power() +modulus() +square_root()
    }
    class ExpressionEvaluator {
        +evaluate(expr)
    }
    ExpressionEvaluator --> Operations
    HistoryManager ..> CalculatorError
```

## Design Decisions & Rationale
- **`ast` instead of `eval`:** prevents code injection.
- **Custom exception hierarchy:** one `except CalculatorError` in the CLI handles every expected failure.
- **Operation registry (dict):** adding a new operation needs one function and one registry line.
- **JSON storage:** human-readable, no database setup needed for a small project.
- **Layered design:** UI, logic, and storage are separate, so each can be tested alone.

