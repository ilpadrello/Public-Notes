---
title: "BASH"
date: "2020-03-22"
tags: 
  - "bash"
  - "linux"
  - "scripting"
  - "sh"
  - "shell"
---

[https://www.learnshell.org/en/Hello,\_World!](https://www.learnshell.org/en/Hello,_World!)

## Basic arithmetic Operations

Simple arithmetics on variables can be done using the arithmetic expression: $((expression))

```
A=3
B=$((100 * $A + 5)) # 305
#you just can't do like this:

C=$a+3
#Because if you try to echo it the result will be 3+3 as text
```

The basic operators are:

**a + b** addition (a plus b)

**a - b** substraction (a minus b)

**a \* b** multiplication (a times b)

**a / b** division (integer) (a divided by b)

**a % b** modulo (the integer remainder of a divided by b)

**a** **\***\* **b** exponentiation (a to the power of b)
