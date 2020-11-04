# JavaScript Regex Tutorial

Regular expressions are patterns used to match character combinations in strings. In JavaScript, regular expressions are also objects. These patterns are used with the exec() and test() methods of RegExp, and with the match(), matchAll(), replace(), replaceAll(), search(), and split() methods of String. This chapter describes JavaScript regular expressions.

## Summary

You construct a regular expression in one of two ways:

Using a regular expression literal, which consists of a pattern enclosed between slashes, as follows:

```sh
let re = /ab+c/;
```


Regular expression literals provide compilation of the regular expression when the script is loaded. If the regular expression remains constant, using this can improve performance.

Or calling the constructor function of the RegExp object, as follows:

```sh
let re = new RegExp('ab+c');
```

Using the constructor function provides runtime compilation of the regular expression. Use the constructor function when you know the regular expression pattern will be changing, or you don't know the pattern and are getting it from another source, such as user input.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

### Anchors
The caret ^ and dollar $ characters have special meaning in a regexp. They are called “anchors”.

The caret ^ matches at the beginning of the text, and the dollar $ at the end.

```sh
let str1 = "Mary had a little lamb";
alert( /^Mary/.test(str1) ); // true
```

```sh
let str1 = "it's fleece was white as snow";
alert( /snow$/.test(str1) ); // true
```
### Quantifiers
Quantifiers indicate numbers of characters or expressions to match.
| Character | Meaning |

| x* | Matches the precedi| ------ | ------ |ng item "x" 0 or more times. For example, /bo*/ matches "boooo" in "A ghost booooed" and "b" in "A bird warbled", but nothing in "A goat grunted". |
| x+ | Matches the preceding item "x" 1 or more times. Equivalent to {1,}. For example, /a+/ matches the "a" in "candy" and all the "a"'s in "caaaaaaandy".|
| x? | Matches the preceding item "x" 0 or 1 times. For example, /e?le?/ matches the "el" in "angel" and the "le" in "angle." If used immediately after any of the quantifiers *, +, ?, or {}, makes the quantifier non-greedy (matching the minimum number of times), as opposed to the default, which is greedy (matching the maximum number of times). |
| x{n} | Where "n" is a positive integer, matches exactly "n" occurrences of the preceding item "x". For example, /a{2}/ doesn't match the "a" in "candy", but it matches all of the "a"'s in "caandy", and the first two "a"'s in "caaandy". |
| x{n,} | x{n,}	Where "n" is a positive integer, matches at least "n" occurrences of the preceding item "x". For example, /a{2,}/ doesn't match the "a" in "candy", but matches all of the a's in "caandy" and in "caaaaaaandy". |
| x{n,m} | Where "n" is 0 or a positive integer, "m" is a positive integer, and m > n, matches at least "n" and at most "m" occurrences of the preceding item "x". For example, /a{1,3}/ matches nothing in "cndy", the "a" in "candy", the two "a"'s in "caandy", and the first three "a"'s in "caaaaaaandy". Notice that when matching "caaaaaaandy", the match is "aaa", even though the original string had more "a"s in it. |
| x*?, x+?, x?, x{n}?, x{n,}?, x{n,m}? | By default quantifiers like * and + are "greedy", meaning that they try to match as much of the string as possible. The ? character after the quantifier makes the quantifier "non-greedy": meaning that it will stop as soon as it finds a match. For example, given a string like ```"some <foo> <bar> new </bar> </foo> thing": /<.*>/ will match "<foo> <bar> new </bar> </foo>"/<.*?>/ will match "<foo>"``` |

### OR Operator
Alternation is the term in regular expression that is actually a simple “OR”.
In a regular expression it is denoted with a vertical line character |.
For instance, we need to find programming languages: HTML, PHP, Java or JavaScript.
The corresponding regexp: html|php|java(script)?.

```sh
let regexp = /html|php|css|java(script)?/gi;

let str = "First HTML appeared, then CSS, then JavaScript";

alert( str.match(regexp) ); // 'HTML', 'CSS', 'JavaScript'
```

### Character Classes
| Characters | Meaning |
| ------ | ------ |
| . | Has one of the following meanings: Matches any single character except line terminators: \n, \r, \u2028 or \u2029. For example, /.y/ matches "my" and "ay", but not "yes", in "yes make my day". Inside a character set, the dot loses its special meaning and matches a literal dot. Note that the m multiline flag doesn't change the dot behavior. So to match a pattern across multiple lines, the character set [^] can be used — it will match any character including newlines.ES2018 added the s "dotAll" flag, which allows the dot to also match line terminators. |

### Flags

### Grouping and Capturing

### Bracket Expressions

### Greedy and Lazy Match

### Boundaries

### Back-references

### Look-ahead and Look-behind

## Author

Ted Pedersen
