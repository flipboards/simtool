#SheetTool Manual

A series of scripts for operation on sheet files (like '.csv' or space separated data, which is common in calculation results)

# colstack

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

# colstat

Perform statistics based on same column on several different files. Currently average, sum and standard deviation is supported.
Example (using File1 and File2 above):
Command:

    colstat -u 'a':'b,c' --stat=ave File1 File2

Output:

    a   b   c
    1   2.1 3.1
    4   5.1 6.1
