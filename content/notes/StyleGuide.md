---
title: "Style Guide"
date: 2023-12-27
lastmod: 2023-12-27
tags:
- software
enableToc: true
---
Here is my style guide for writing code and working on software projects (updated as needed). It differs by language. 

## Python
### General
- Generally follows the [Black](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#line-length) style guide.
- Line length is 88 characters.
- Tab size is 4 spaces.
- Though not standard or popular, keep parenthese for condidtions in `if` statements and `while` loops (space before parentheses).
```py
if (condition):
    # do something

while (condition):
    # do something
```
- Keep parentheses around tuples
- Empty lines should be used to separate logical sections of code. (e.g. between two functions, end of an if/while block, block of code with specific purpose, etc.)
- Use `is` and `is not` instead of `==` and `!=` when comparing to `None`.


### Imports
- Imports should be on separate lines.
- Imports should be grouped in the following order:
    - Standard library imports
    - Third-party imports
    - Local packages imports
- Imports should be sorted alphabetically within each group.
- `from x import y` statements should come before `import z` statements.
- If multiple imports are from the same package, they should be on the same line.
- Imports should try to be absolute with regards to the system path.

### Docstrings
- Docstrings should be in triple quotes.
- Docstrings for functions/methods are in the following format:
```py
"""
Description of purpose of function/method.

Parameters
----------
param1 : type
    Description of param1
...

Returns
-------
type
    Description of return value
...

Raises
------
Exception
    Description of exception
...
"""
```
- Docstrings for classes are in the following format:
```py
"""
Description of purpose of class.

Attributes
----------
attr1 : type
    Description of attr1
...

Methods
-------
method1(param1, param2, ...)
    Description of method1
...
"""
```

### Comments
- Comments should be used to explain code that is not self-explanatory.

### Naming
- Use `snake_case` for variable names and function names.
- Use `PascalCase` for class names.
- Use `UPPER_SNAKE_CASE` for constants.
- Add `_` at the beginning for variables and methods that should be considered private.

### Strings and Numbers
- Use double quotes for strings.
- When numbers are used in code, when they require letters, lowercase used for syntactic elements, uppercase used for letters that are digits. (e.g. `0x1A` instead of `0x1a`, `1e10` instead of `1E10`)
- Use `f-strings` for string formatting.

### Call Chains
- When using call chains, each call should be on a separate line.
```py
result = (
    some_object.long_function_name(param1, param2)
    .long_function_name(param3, param4)
    .long_function_name(param5, param6)
)
```

### Arithmetic
- Use spaces around all operators _unless_ the arithmetic is being used to index an iterable/dictionary (don't ask me why I prefer this, I just do).
```py
result = 1 + 2
array = [1, 2, 3, 4]
result = array[1+2]
```

### Type Annotations
- Type annotations should be used for all functions and methods when possible.
    - Use for parameter types and return types.

### Files and Folders
- File names should be all lowercase/snake case.
- Folder names should all be lowercase and should not require snake case.
- Separate tests folder from source code folder.


## Java
### General
#### Source Code File Structure
- Source code files should be structured as follows:
    - License/Copyright information
    - Package declaration
    - Imports
    - Public/Top Level Class declaration

#### Formatting
- Line length is 140 characters.
- Tab size is 4 spaces.
- Opening braces should be on the same line as the declaration with space before the brace.
- Closing braces should be on their own line.
- Line break after closing brace unless it is followed by an `else` or `catch` statement.
- If empty block between braces, braces should be on the same line.

#### Switch Statements
- Case statements should be indented 4 spaces from the switch statement.
- Statements within each case should be indented 4 spaces from the case statement.
- Always include a `default` case.

### Imports
- Imports should be on separate lines.
- Imports should be grouped in the following order:
    - Standard library imports
    - Third-party imports
    - Local packages imports
- Imports should be sorted alphabetically within each group.


### Docstrings
- Use Javadoc for docstrings.

### Comments
- Comments should be used to explain code that is not self-explanatory.
- Single line comments use `//`.
- Multi-line comments use `/* */`.

### Naming
- Use `camelCase` for variable names and function names.
- Use `PascalCase` for class names.
- Use `UPPER_SNAKE_CASE` for constants.

### Call Chains
- When using call chains, each call should be on a separate line.
```java
result = someObject.longFunctionName(param1, param2)
    .longFunctionName(param3, param4)
    .longFunctionName(param5, param6);
```
- When line-wrapping a chain of method calls, the indentation on the subsequent lines should be at least 4 spaces.

### Files and Folders
- File names should be all camel case.
- Folder names should all be lowercase and should not require snake case.
- Separate tests folder from source code folder.


