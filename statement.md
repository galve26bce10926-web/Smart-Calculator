## Problem
Basic calculators can perform only one operation (addition, substraction, etc.) at a time or use unsafe methods such as eval() to parse expressions from strings. Most do not retain a calculation history, provide useful error messages or any additional reporting. A safe, reliable calculator that validates input and provides context to errors is needed for students and general use.

## Scope
In scope: The development of a command-line calculator with step-by-step operations, a secure expression evaluator, persistent history, summary reports and CSV statistics export, logging and input validation.

Out of scope: Graphical or web interface, scientific operations (trigonometric, logarithmic functions), multi-user accounts and cloud storage.

## Target Users
- Students learning Python and object-oriented programming
- End-users desiring a simple, dependable command-line calculator
- Instructors desiring a small, easily-understood demonstration project

## High-Level Features
1. Arithmetic Operations Module: add, subtract, multiply, divide, exponent, modulus, square root
2. Expression Evaluator Module: evaluate expressions such as '2 + 3 (4 - 1)' with the ast module
3. History & Reports Module: persistent JSON history, summary statistics, CSV report
4. Input validation, custom exceptions, file logging
5. Unit test suite (18+ tests)

