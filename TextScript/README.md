# Textscript

Substitute predefined variables and functions in text file.

## Usage

The basic command is

    ts name1=val1 name2=val2 input

Where name1,name2 is the variable name existed in input file, and val is its string value. See examples below for more information.

## Input File

TS substitutes variables based on replacing block. There are two kinds of blocks: variable block and function block. They all begin with a flag str (default is '$').

String begin with '\\$' will not be regarded as variable, but backslash '\\' will be removed.

#### Variable Block
Only a single string. Examples:

    $a $abc $_a123

Simple variable will be directly substituted by new values. If the variable value is not given, it will remain unchanged.

#### Function Block
A block of code. Must be included in large brackets '{}'. Examples:

    ${a + b}  ${print(a)}

The code block will be executed by python by `eval()` (default) or `exec()` (if *--exec* is specified). The stdout of `exec()` will be redirected as substitution.

When substituting by `exec()`, variables are treated as locals, which means their value can be dynamically changed.

TS will raise exception if variable value in function block is not given.

## Options

- --exec:         use `exec()` instead of `eval()` in blocks
- -f, --flag:     specify flag str (only one char), default is `$`
- -h, --help:     display this help string
- -n, --note=var:note:     add description to variable
- -o, --output=outputname:   output filename (default is stdout)
- -s, --strict:   all variables must be replaced
- --str:          treat value as string (default is auto convert to number)
- -v, --view:     view variables without substitution

Textscript will also detect lines start with `#ts` (no space, no indent) at the head of file, and add them into option list. These lines will be removed in output.

## Examples

Substitution example:

example.txt

    #ts a=2
    #ts -n b:The second variable
    \$This file specify a as $a, specify ${'b as %d' % b}.

Command1:

    ts -o 'a=$a-b=$b.output' b=3 example.txt

a=2-b=3.output:

    $This file specify a as 2, specify b as 3.

---
Command2:

    ts -v example.txt

stdout:

    Output: a=$a-b=$b.output
    Name    Default Description
    a       2
    b       <unset> The second variable

---
Multiple substituion:

    for b in 1 2 3 4 5; do ts b:$b example.txt; done
