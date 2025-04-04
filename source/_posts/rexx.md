---
title: REXX CheatSheet
date: 2025-04-03 12:00:00
icon: terminal
background: bg-blue-600
tags:
  - REXX
  - TSO/E
categories:
  - Mainframe
intro: A comprehensive reference guide for REXX programming language under TSO/E environment.
---

## Execution Methods {.cols-2}

### Explicit Execution

```text
EXEC dataset(member) 'parameters' operands
```

Runs a REXX program from a **sequential dataset** or **PDS member**[^1].

### Implicit Execution

```text
membername parameters
```

Requires the script to be in **SYSEXEC** or **SYSPROC**. The system checks **SYSEXEC** first, then **SYSPROC**, and
finally the command line[^1].

### Extended Implicit Execution

```text
%membername parameters
```

Forces execution from **SYSEXEC/SYSPROC**, bypassing other execution methods[^1].

## Basic Syntax

### Comments

```
/* REXX execs should start with a comment */

say "Hello!" /* Inline comment */

/* Multi-line comments
   can span multiple lines */

/* Nested comments /* are possible */ too */
```

**Note**: Some environments **require** the first line to be a comment containing the word "REXX". For maximum
compatibility, always start programs with a comment that includes "REXX"[^1].

### Instructions and Continuation {.cols-2}

**Separating Instructions**

```
a = 'Cat';b = 'Dog';c = 17  /* Semicolons separate instructions */
```

**Line Continuation**

```
sentence = "The quick brown fox jumps over", /* Comma continues */
           "the lazy dog."
```

**Formatting with Indentation**

```
/* Indentation improves readability but is ignored by REXX */
If a = b Then
   Say "a and b are equal."
Else
   Say "a and b are not equal."
```

## Variables and Data Types {.cols-2}

### Variables

```
/* Variables in REXX are dynamically typed and not case-sensitive */
name = "John"  /* Same as NAME or Name */
age = 30       /* Numeric value */

/* Variable names can be up to 250 characters */
very_long_descriptive_variable_name = "Valid"

/* Special characters allowed in TSO/E: @#$!?_.¢ */
user@company = "email format"
first.last = "compound variable"

/* Cannot begin with numbers or periods */
/* INVALID: 1stPlace  .config */
```

### Strings

```
/* Strings can use single or double quotes (equivalent to REXX) */
var1 = 'This is a literal string.'
Say "Hello, World!"

/* Quotes in strings require doubling the quote character */
Say "Don't do that."     /* Using double quotes */
Say 'Don''t do that.'    /* Using single quotes */

/* String functions */
Address TSO 'LISTCAT'    /* Command to environment */
where = Pos('B',input)   /* Function call */
```

## Control Structures {.cols-2}

### Logical Operators

| Operator | Meaning                        |
| :------- | :----------------------------- | ---- |
| `&`      | "And"                          |
| `        | `                              | "Or" |
| `&&`     | "Exclusive Or (only one True)" |
| `\`      | "Not"                          |

**Examples**

```
var1 = 1; var2 = 0
Say var1 & var2         --> Returns 0 (False)
Say var1 | var2         --> Returns 1 (True)
Say var1 && var2        --> Returns 1 (True)
```

### If-Else

**Syntax**

```
If comparison Then instruction1
Else instruction2
```

The **Else** is optional  
Only one instruction is allowed following **Then** or **Else**

**Example**

```
If age >= 18 Then
   Say "Adult"
Else
   Say "Minor"
```

### Do-End Group

To code more than one instruction following a **Then** or **Else**, bracket them between the **Do** and the **End**
instructions :

```
If var1 = var2 Then
    Do
        instruction1
        instruction2
    etc...
    End

```

### Select-When-Otherwise

```
Select
   When month = 'JAN' Then
        Do
            instructions...
        End
   When month = 'FEB' Then
        If leap_year Then
            days = 29
        Else
            days = 28
   Otherwise
        days = 30
End
```

### Loops

**Controlled repetitive DO loop**  
Syntax : Do cntlvar = init To final By incr For maxloops

- **ctlvar** : The loop control variable name
- **init** : The initial value assigned to the control variable
- **final** : The final value the control variable will accept
- **incr** : The amount by which the control variable is incremented
- **maxloops** : The absolute maximum number of times the loop will execute

```
Do i = 1 To 100 By 2
    /* these instructions will execute while
    the value of the variable i changes from
    1 to 3 to 5 ... to 100 */
End
```

**DO WHILE condition**  
**Do While** loops first checks the condition before executing the instructions.

```
Do While lines_available
   line = readline()
   Process(line)
End
```

**DO UNTIL condition**  
**Do Until** loops always perform the instructions at least once.

```
Do Until answer = 'YES'
   Say "Please enter YES to continue:"
   Pull answer
End
```

**DO loops for a specific number of times**

```
Do 5
   instructions
End
```

**Infinite loop with conditional EXIT**  
The **Leave** keyword is used to exit out from a loop

```
Do Forever
   If finished Then Leave
   Process_more()
End
```

**Leave and Iterate**

- The **Leave** instruction is used to terminate the currently active loop
- The **Iterate** instruction is used to bypass the remaining loop instrcution and pass the control to the End
  instruction

**Nested Loops**

- Loops can be nested
- The loop control variable can be used as the name of the loop on the End, Leave, and Iterate instrcution

```
Do outer = 1
    Do inner = 1
        ...
        If ... Then Iterate inner
        If ... Then Iterate outer
        If ... Then Leave outer
        If ... Then Leave inner
    End inner
End outer
```

## Common Functions {.bold-first}

| Function                          | Description                          |
| :-------------------------------- | :----------------------------------- |
| `LENGTH(string)`                  | Returns the length of the string     |
| `POS(needle, haystack, [start])`  | Finds position of needle in haystack |
| `SUBSTR(string, start, [length])` | Extracts a substring                 |
| `TRANSLATE(string, [out], [in])`  | Character translation                |
| `WORD(string, n)`                 | Returns the nth word                 |
| `WORDS(string)`                   | Counts words in a string             |
| `DATE([option])`                  | Returns date in various formats      |
| `TIME([option])`                  | Returns time in various formats      |
| `RANDOM(min, max)`                | Returns a random number              |

{.show-header}

## Operations

### Arithmetic {.bold-first}

| Function                      | Description  | Example               |
| :---------------------------- | :----------- | :-------------------- |
| `Parentheses`                 | ( )          | (5 + 3) \* 2 gives 16 |
| `Prefix operator`             | +, -         | -5 or +x              |
| `Exponentiation`              | \*\*         | 2 \*\* 3 gives 8      |
| `Multiplication and division` | \*, /, %, // | 7 \* 3 gives 21       |
|                               |              | 10 / 2 gives 5        |
|                               |              | 7 % 2 gives 3         |
|                               |              | 7 // 2 gives 1        |
| `Exponentiation`              | \*\*         | 2 \*\* 3 gives 8      |
| `Addition and substraction`   | +/-          | 7 + 3 gives 10        |

### Say

**Syntax**

```
Say expression
```

**Examples**

```
Say "Hello, world!"

Say 25 * (9 / 3)

Say "The answer is :" num1 + num2

Say

Say ''
```

### Concatenation

#### Concatenation by abuttal (direct)

```
first = 'John'
last = 'Smith'
name = first||last
```

Result: 'JohnSmith'

#### Concatenation with blank operator (space)

```
greeting = 'Hello' 'there'

```

Result: 'Hello there'

#### Concatenation with variable values

```
a = 'ABC'
b = 'DEF'
result1 = a b
result2 = a||b
```

Result 1: 'ABC DEF'  
Result 2: 'ABCDEF'

#### Concatenation with literals

```
message = 'Error:' 'File not found'
```

Result: 'Error: File not found'

### Trace

**Syntax**

```
Trace option
```

where option is the trace option to put in effect

**Useful options (intro)**

- N : Normal (default)
- O : Off
- R : Results
- I : Intermediates

### REXXTRY

EXEC REXXTRY allows you to interactively execute REXX instructions. Each instruction string is executed when you press
ENTER. To end, type EXIT.

```
/* interactive REXX instruction execution*/
Do Forever /* the EXIT instruction will terminate loop */
    Say "REXXTRY:" /* prompt for the next instruction */
    Parse External @line@
    Interpret @line@
End /* Do forever */
```

## Comparing

### Comparison Operators

| Symbols | Operation                                |
| :------ | :--------------------------------------- |
| `=`     | equal                                    |
| `\=`    | not equal                                |
| `<>`    | not equal (less than or greater than)    |
| `><`    | not equal (greater than or less than)    |
| `<=`    | less than or equal to                    |
| `\>`    | less than or equal to (not greater than) |
| `>=`    | greater than or equal to                 |
| `\<`    | grater than or equal to (not less than)  |
| `<`     | less than                                |
| `>`     | greater than                             |

### String Comparison

- Always case sensitive

- Two types of string comparison :
  - **Normal** : Ignore leading/trailing blanks
  - **Strict** : Each character must match exactly in order for the two terms to be equal

**Examples**

```
answer = "   YES   "
Say answer = "YES"     --> Returns 1 (True)
Say answer = "yes"     --> Returns 0 (False)
```

### Strict Comparison

- Doubling an operator character forces a strict comparison
- Alawys character comparison, never numeric
- Just match the bytes, one by one
- Leading and trailing blanks are included

**Examples**

```
answer = "   YES   "
Say answer == "YES"     --> Returns 0 (False)
Say 5 == 5.0            --> Returns 0 (False)
```

### Numeric Comparison

- Automatically performed if both terms can be recognized as numbers, and the comparison type is normal
- Comparison is performed within the **Numeric Fuzz** value (default is 0)

**Examples**

```
num1 = 31; num2 = 30
Say num1 = num2         --> Returns 0 (False)
Say num1 \= num2        --> Returns 1 (True)
Say num1 > num2         --> Returns 1 (True)
Say num1 < num2         --> Returns 0 (False)
```

## Parsing

### From Keyboard (Parse Pull)

**Syntax**

```
Parse (Upper) Pull variable_template
```

**Examples**

```
/*Rexx sample*/
Say "Please enter your name:"

Parse Pull name

Say "Hello" name". Enter two numbers:"

Parse Upper Pull numa numb

Say "You entered these numbers:" numa "and" numb
```

### From command line (Parse Arg)

**Syntax**

```
Parse (Upper) Arg variable_template
```

**Examples**

```
/*Rexx sample*/

Parse Arg inparms

Say inparms

Parse Upper Arg parm1 parm2 parm3 rest

Say parm1

Say parm2
```

### Parsing Placeholder

To ignore some of the input data, you could code a "dummy" variable :

```
Parse Pull var1 var2 dummy var3

Parse Arg var1 var2 dummy var3
```

Rather than waste a variable name, code the "parsing placeholder" in the variable template, a period in place of a
variable name:

```
Parse Pull var1 var2 . var3

Parse Arg var1 var2 . var3
```

Data that would be assigned to a variable in that position is ignored.

## I/O Operations {.cols-2}

### Input

```

/_ Read input with PULL (uppercase conversion) _/ PULL variable

/_ Read input with PARSE PULL (preserves case) _/ PARSE PULL variable

/_ Read from a specific stream _/ stream = 'MYFILE' 'EXECIO 1 DISKR' stream '(STEM line.)'

```

### Output

```

/_ Display output _/ SAY "Hello, World!"

/_ Write to a specific stream _/ stream = 'MYFILE' line.1 = "This is a line of text" 'EXECIO 1 DISKW' stream '(STEM
line.)'

```

## TSO/E Specific {.cols-2}

### Environment Commands

```

/_ Execute TSO commands _/ ADDRESS TSO "LISTCAT"

/_ Execute ISPF commands _/ ADDRESS ISPEXEC "DISPLAY PANEL(MYPANEL)"

/_ Execute MVS commands _/ ADDRESS MVS "DISPLAY A,L"

```

### Accessing Variables

```

/_ Access REXX variables from TSO/E _/ value = SYMBOL("MYVAR")

/_ Access TSO/E variables from REXX _/ ADDRESS TSO "EXEC 'MY.REXX(MYPROG)' 'PARM1 PARM2'" PARSE ARG parm1 parm2

```

```

```
