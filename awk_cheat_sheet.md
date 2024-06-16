
# The 'awk' command cheat sheet

## Basic syntax

```Bash
awk ' /pattern/ [BEGIN {Program}] { Program } [END { Program }]' file
```

## Common Usage

1. Print entire line

```Bash
awk '{ print }' file
```

2. Print specific fields

```Bash
awk '{ print $1, $2 }' file
```
$1 is the first field, $2 is the second field.

3. Field separator

use the -F flag
```Bash
awk -F: '{ print $1, $2}' file
```

or set the FS built in variable with your prefered separator character

```Bash
awk -v FS=: '{ print $1, $2}' file
```
$1 and $2 must now be separated by : (a column) to be printed by awk

## Patterns

1. Print lines matching a pattern

```Bash
awk '/pattern/ { print }' file
```

2. Print lines with more than 3 fields

```Bash
awk 'NF > 3' file
```
NF stands for number of fields

3. Print lines where first fied is greater than 100

```Bash
awk '$1 > 100 { print }' file
```

## Actions

1. Print specific fields and text:

```Bash
awk '{print "Field 1:", $1, "Field 2:", $2}' file
```

2. sum values in a column:

```Bash
awk '{ sum += $1 } END { print sum}' file
```

3. Print the max value in a column

```Bash
awk ' NR == 1 || $1 > max { max = $1} END {print max}' file
```

## Built-in variables

* **`NR`**: Number of records (line number)
* **`NF`**: Number of fields in the current record.
* **`FS`**: Field Separator (default is whitespace)
* **`OFS`**: Output field separator(default is space)
* **`RS`**: Record sepacator (default is a newline)
* **`ORS`**: Output record separator (default is a newline)

## Control flow

1. If-else statements

```Bash
awk '{ if ($1 > 100) print "High" els print "Low"}' file
```

2. For loop:

```Bash
awk '{ for (i =1; i <= NF; i++) print i}' file
```

## Functions

1. Lenght of the strinf or field

```Bash
awk '{ print length($1) }' file
```

2. Substrings:

```Bash
awk '{ print substr(1, 3, $1) }' file 	# This prints the first 3 characters 
					# in the first field
```

3. String replacement (substitute):
```Bash
awk '{ gsub(/text/, "new text", $1); print }'
# This replaces any occurence of the regex /text/ with "ne text" in the first field.
```

## Advanced Usage

1. Combine multiple awk commands

```Bash
awk '{ print $1 }' file | awk ' { sum += $1 } END {print sum}'
# First awk result is piped to the second awk to calculate the sum of the first field in each row.

2. Use BEGIN and END blocks
```Bash
awk 'BEGIN { print "Start" } { print $1 } END { print "End" }' file
```
Tips: 
- Always single quote the script when typing it explicitly in the command line to avoid shell issues
- Use -v option to pass a shell variable to awk.
