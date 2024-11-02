# Carot's IDA Pattern Plugin

This shi works for IDA 9 and was made by [AI](https://claude.ai)

## Basic Syntax
Pattern searches are composed of chained operations:
- `:` - Chain operations (pipe output of previous operation to next)
- `;` - Separate multiple pattern searches
- `[...]` - Comments or modifiers (e.g., [silent])

## Operations

### String Search
```
ln string(value)
```
Finds a string in the binary.
- **Input**: String value in quotes
- **Returns**: Line address where string is located
- **Example**: `string("_LOADED")`

### Cross References
```
ln xref(n)
```
Gets the nth cross-reference to an address.
- **Input**: Index number (0-based)
- **Returns**: Line address of the xref
- **Example**: `string("test"):xref(0) [get first xref]`

### Function Container
```
ln sub()
```
Gets the function containing the current line.
- **Input**: None
- **Returns**: Function start address
- **Example**: `string("test"):xref(0):sub() [get function containing xref]`

### Line Offset
```
ln line(n)
```
Moves n lines relative to current position.
- **Input**: Number (positive = forward, negative = backward)
- **Returns**: Line address at offset
- **Example**: `string("test"):xref(0):line(-2) [two lines up]`

### Decompile
```
ln decompile()
```
Gets decompiled pseudocode for current function.
- **Input**: None
- **Returns**: Line address in decompiled view
- **Example**: `string("test"):xref(0):decompile()`

### Function Argument
```
arg arg(n)
```
Gets the nth argument of current instruction/line.
- **Input**: Argument index (0-based)
- **Returns**: Argument value or variable
- **Example**: `string("test"):xref(0):arg(1) [second argument]`

### Print
```
void print(ln or arg)
```
Prints the result of a pattern search.
- **Input**: Any pattern search operation
- **Returns**: None (prints to output window)
- **Example**: `print(string("test"):xref(0):decompile():arg(1))`

## Modifiers
- `[silent]` - Suppresses result window display
  ```
  [silent]print(string("test"):xref(0))
  ```

## Examples
```python
# Find luaL_register function in Roblox
string("_LOADED"):xref(0):sub() [find function containing second xref]

# Print function argument without showing window
[silent]print(string("test"):xref(0):arg(1))

# Multiple searches
string("func1"):xref(0); string("func2"):xref(0)

# Get decompiled function with argument
string("test"):xref(0):decompile():arg(1)

# Chain multiple xrefs
string("something"):xref(0):sub():xref(0):sub()
```
