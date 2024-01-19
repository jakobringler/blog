---
title: Strings in VEX
draft: true
tags:
  - houdini
  - vex
---
This list was created and kindly shared by [Mattias Malmer]() on a discord I'm on. Thanks for letting me host it here:

```
abspath() -- Returns the full path of a file.
chr() -- Converts a Unicode codepoint to a UTF8 string.
concat() -- Concatenates all the specified strings into a single string.
decode() -- Decodes a variable name that was previously encoded.
decodeattrib() -- Decodes a geometry attribute name that was previously encoded.
decodeparm() -- Decodes a node parameter name that was previously encoded.
decodeutf8() -- Decodes a UTF8 string into a series of codepoints.
encode() -- Encodes any string into a valid variable name.
encodeattrib() -- Encodes any string into a valid geometry attribute name.
encodeparm() -- Encodes any string into a valid node parameter name.
encodeutf8() -- Encodes a UTF8 string from a series of codepoints.
endswith() -- Indicates if the string ends with the specified string.
find() -- Finds an item in an array or string.
isalpha() -- Returns 1 if all characters in the string are alphabetic.
isdigit() -- Returns 1 if all characters in the string are numeric.
itoa() -- Converts an integer to a string.
join() -- Concatenates all strings of an array, inserting a common spacer.
lstrip() -- Strips leading whitespace from a string.
makevalidvarname() -- Forces a string to conform to the rules for variable names.
match() -- Returns 1 if the subject matches the pattern specified, or 0 if it doesn’t.
opdigits() -- Returns the integer value of the last sequence of digits of a string.
ord() -- Converts a UTF8 string into a codepoint.
pluralize() -- Converts an English noun to its plural.
re_find() -- Matches a regular expression in a string.
re_findall() -- Finds all instances of the given regular expression in the string.
re_match() -- Returns 1 if the entire input string matches the expression.
re_replace() -- Replaces instances of regex_find with regex_replace.
re_split() -- Splits the given string based on regex match.
relativepath() -- Computes the relative path for two full paths.
relpath() -- Returns the relative path to a file.
replace() -- Replaces occurrences of a substring.
replace_match() -- Replaces the matched string pattern with another pattern.
rstrip() -- Strips trailing whitespace from a string.
split() -- Splits a string into tokens.
splitpath() -- Splits a file path into the directory and name parts.
sprintf() -- Formats a string like printf but returns the result as a string instead of printing it.
startswith() -- Returns 1 if the string starts with the specified string.
strip() -- Strips leading and trailing whitespace from a string.
strlen() -- Returns the length of the string.
titlecase() -- Returns a string that is the titlecase version of the input string.
tolower() -- Converts all characters in a string to lower case.
toupper() -- Converts all characters in a string to upper case.
```

---

sources / further reading:
- [Houdini DOCs -SideFX](https://www.sidefx.com/docs/houdini/vex/strings.html)

