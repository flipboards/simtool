# SheetTool Manual

A series of scripts for operation on sheet files (like '.csv' or space separated data, which is common in calculation results)

Python module [pandas](https://pandas.pydata.org/) is required for colstack and colstat.

## colstack
---

Stack same column on several different files together. Example:

If we have two files:

File1:

    a   b   c
    1   2   3
    4   5   6

File2:

    a   b   c
    1   2.2 3.2
    4   5.2 6.2

Command:

    colstack -u 'a':'b,c' File1 File2

Output:

    a   File1-b   File2-b   File1-c File2-c
    1   2   2.2 3   3.2
    4   5   5.2 6   6.2


## colstack2
---

Stack the second column from several different two-column files, and keeps the first column at the same time. The title is renamed automatically.

Example:

File1.txt

    x   y
    1   a
    2   b

File2.txt

    x   y
    1   c
    2   d

Command:

    colstack2 File1 File2

Output:

    x   y-File1 y-File2
    1   a   c
    2   b   d

Options:
- -s: Specify input separator.


## colstat
---

Perform statistics based on same column on several different files. Currently average, sum and standard deviation is supported.
Example (using File1 and File2 above):
Command:

    colstat -u 'a':'b,c' --stat=ave File1 File2

Output:

    a   b   c
    1   2.1 3.1
    4   5.1 6.1

Options:

- --format:       float format, default is '%g'
- -h,--help:      display this help message.
- --notitle:      do not use title (and in output).
- -s:             specify split string of input datasheet. Default is tab.
- --sep:          specify output split string. Default is tab.
- --stat:         which statistic to perform. Can be 'ave'/'sum'/'dev'(standard deviation). Default is 'ave'
- -u:             which column to use. Similar with gnuplot grammar, column can be '$1' to specify index, or strings to specify title.


## tp
---
Transpose matrix file separated by tab.

Usage:

    tp [file]

## csv
---
View csv file.

Command:

    csv [file]
