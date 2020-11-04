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
|Characters|Meaning|
|--- |--- |
|.|Has one of the following meanings:

		
				Matches any single character except line terminators: \n, \r, \u2028 or \u2029. For example, /.y/ matches "my" and "ay", but not "yes", in "yes make my day".
				Inside a character set, the dot loses its special meaning and matches a literal dot.
			

			Note that the m multiline flag doesn't change the dot behavior. So to match a pattern across multiple lines, the character set [^] can be used — it will match any character including newlines.

			ES2018 added the s "dotAll" flag, which allows the dot to also match line terminators.|
|\d|Matches any digit (Arabic numeral). Equivalent to [0-9]. For example, /\d/ or /[0-9]/ matches "2" in "B2 is the suite number".|
|\D|Matches any character that is not a digit (Arabic numeral). Equivalent to [^0-9]. For example, /\D/ or /[^0-9]/ matches "B" in "B2 is the suite number".|
|\w|Matches any alphanumeric character from the basic Latin alphabet, including the underscore. Equivalent to [A-Za-z0-9_]. For example, /\w/ matches "a" in "apple", "5" in "$5.28", "3" in "3D" and "m" in "Émanuel".|
|\W|Matches any character that is not a word character from the basic Latin alphabet. Equivalent to [^A-Za-z0-9_]. For example, /\W/ or /[^A-Za-z0-9_]/ matches "%" in "50%" and "É" in "Émanuel".|
|\s|Matches a single white space character, including space, tab, form feed, line feed, and other Unicode spaces. Equivalent to [ \f\n\r\t\v\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]. For example, /\s\w*/ matches " bar" in "foo bar".|
|\S|Matches a single character other than white space. Equivalent to [^ \f\n\r\t\v\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]. For example, /\S\w*/ matches "foo" in "foo bar".|
|\t|Matches a horizontal tab.|
|\r|Matches a carriage return.|
|\n|Matches a linefeed.|
|\v|Matches a vertical tab.|
|\f|Matches a form-feed.|
|[\b]|Matches a backspace. If you're looking for the word-boundary character (\b), see Boundaries.|
|\0|Matches a NUL character. Do not follow this with another digit.|
|\cX|Matches a control character using caret notation, where "X" is a letter from A–Z (corresponding to codepoints U+0001–U+001F). For example, /\cM/ matches "\r" in "\r\n".|
|\xhh|Matches the character with the code hh (two hexadecimal digits).|
|\uhhhh|Matches a UTF-16 code-unit with the value hhhh (four hexadecimal digits).|
|\u{hhhh} or \u{hhhhh}|(Only when the u flag is set.) Matches the character with the Unicode value U+hhhh or U+hhhhh (hexadecimal digits).|
|\|Indicates that the following character should be treated specially, or "escaped". It behaves one of two ways.

		
				For characters that are usually treated literally, indicates that the next character is special and not to be interpreted literally. For example, /b/ matches the character "b". By placing a backslash in front of "b", that is by using /\b/, the character becomes special to mean match a word boundary.
				For characters that are usually treated specially, indicates that the next character is not special and should be interpreted literally. For example, "*" is a special character that means 0 or more occurrences of the preceding character should be matched; for example, /a*/ means match 0 or more "a"s. To match * literally, precede it with a backslash; for example, /a\*/ matches "a*".
			

			
			To match this character literally, escape it with itself. In other words to search for \ use /\\/.|


### Flags

### Grouping and Capturing

### Bracket Expressions

### Greedy and Lazy Match

### Boundaries

### Back-references

### Look-ahead and Look-behind

## Author

Ted Pedersen
