# Textscript manual

Textscript is a light script based on python, for generating input/script files more flexibly and efficiently. Its two commands are:

- ts sub: Substitute variables and execute block commands;
- ts view: View variables;

## Install

Requires python3. Just a single file.

## Grammar

There are 'simple variable' and 'blocks' in textscript. They all begin with a flag str (default is '$'). String begin with '\\$' will not be regarded as variable, but backslash '\\' will be removed.

#### Simple Variable
Only a single string. Examples:

    $a $abc $_a123

Simple variable will be directly substituted by new values.

#### Block
A block of code. Must be included in large brackets '{}'. Examples:

    ${a + b}  ${print(a)}

The code block will be executed by python, by default is _eval()_, but can also be specified to _exec()_.

Variables in block must be specified a value (or default value). In _exec_ mode, variables are treated as locals, which means their value can be dynamically changed.

## Usage

Basic command for _ts sub_ is

    ts sub -r name1:val1,name2:val2 input

Where name1, name2 are variable name, and val1, val2 are values to replace the position of variables. Input must be a file.

The full options for _ts sub_:

- --exec:     use 'exec' instead of 'eval' in blocks
- --flag:     specify flag str (only one char), default is '$'
- -h/--help:  display help string
- -o:         output filename (default is stdout), which can also be textscript.
- -r:         replace string, followed by name:value... dict
- --rmmacro:  remove macro in file after substitution
- --strict:   all variables must be replaced
- --strmode:  treat value as string (default is auto convert to number)
- -v/--version: display version

Basic command for _ts view_ is

    ts view input

The full options for _ts view_:
- --flag:     specify flag str
- -h/--help:  display this help
- -v/--version: display version

## Macros

Textscript will detect lines start with '#ts' (no space, no indent) at the head of file. Default options and values as well as variable descriptions will be readed. Currently there are two options: _ts set_ and _ts note_.

#### ts set: set default option
Example:

    #ts set -r name1:arg

The argument list for _ts set_ macro is same as _ts sub_.

#### ts note: set variable description
Example:

    #ts note a Important variable

Which will be automatically readed by _ts view_.

__Notice:__ Macros do not have to be in the top of file. However, non-macro lines among and before macro lines will be ignored by both _ts view_ and _ts sub_.


## Examples

File:

    #ts set -r a:2 -o a=$a-b=$b.output --rmmacro
    #ts note b The second variable
    \$This file specify a as $a, specify ${'b as %d' % b}.

Command (sub):

    ts sub -r b:3 example.txt

Output (a=2-b=3.output)

    $This file specify a as 2, specify b as 3.

Command (view):

    ts view example.txt

Output:

    Output: a=$a-b=$b.output
    Name    Default Description
    a       2
    b       <unset> The second variable

Use with shell script:

    for b in 1 2 3 4 5; do ts-sub -r b:$b example.txt; done
