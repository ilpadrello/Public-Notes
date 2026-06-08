Basic syntax: 

```bash
if [condition] then
....
else
....
fi
```

- Since linux is case sensivite `if` and `IF` are not the same !
- you can use `elif` as else if

## The important part is not in the syntax but in the comparison.

There are multiple way of compare if statements, let's have a look: 

### String compare
`[1 = 1]` is not a number comparison, is a string comparison. And that is why `[01 = 1]` is false. Bash do not know what you are comparing, and by default is text !

### Number comparison
If you want to compare numbers you must use number comparisons like this: 

```bash
[01 -eq 1]
```

- `-eq` → numeric equal
- `-ne` → not equal
- `-gt` → greater than
- `-lt` → less than
- `-ge` → greater or equal
- `-le` → less or equal

Since `<` and `>` are used for output and input manipulation they can not be used to making comparisons

### `[[ ... ]]` → Bash extended test
This is actually a better way to compare stuff:

```bash
[[ 1 = 1 ]]  
[[ 1 == 1 ]]
```

Inside `[[ ]]`:

- `=` and `==` both do string comparison
- `==` allows pattern matching (`*`, etc.)
- `<` and `>` work for strings
- safer with variables (no word splitting issues)

## BEST PRATICE
### For numbers:
```bash
if (( 1 == 1 )); then
```
This is arithmetic evaluation — cleaner for numeric logic.

```bash
if (( a > b )); then
```
No `-gt`, no quotes needed.
### For strings
Use: 

```bash
a="hello"  
b="hello"
if [[ "$a" == "$b" ]]; then
```
Safer and more modern.

Inside `[[ ]]`, `==` allows pattern matching:
```bash
a="hello.txt"  
  
[[ "$a" == *.txt ]] && echo "Text file"
```

That works because `*.txt` is treated as a pattern.

But if you quote the pattern:

```bash
[[ "$a" == "*.txt" ]] # now it's literal string comparison
```

