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
| ------ | ------ |
| x* | Matches the preceding item "x" 0 or more times. For example, /bo*/ matches "boooo" in "A ghost booooed" and "b" in "A bird warbled", but nothing in "A goat grunted". |
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
Character classes distinguish kinds of characters such as, for example, distinguishing between letters and digits.

| Characters | Meaning |
| ------ | ------ |
| ```.``` | Has one of the following meanings: Matches any single character except line terminators: \n, \r, \u2028 or \u2029. For example, /.y/ matches "my" and "ay", but not "yes", in "yes make my day". Inside a character set, the dot loses its special meaning and matches a literal dot. Note that the m multiline flag doesn't change the dot behavior. So to match a pattern across multiple lines, the character set [^] can be used — it will match any character including newlines. ES2018 added the s "dotAll" flag, which allows the dot to also match line terminators. |
| ```\d``` | Matches any digit (Arabic numeral). Equivalent to [0-9]. For example, /\d/ or /[0-9]/ matches "2" in "B2 is the suite number". |
| ```\D``` |  Matches any character that is not a digit (Arabic numeral). Equivalent to [^0-9]. For example, /\D/ or /[^0-9]/ matches "B" in "B2 is the suite number". | 
| ```\W``` | Matches any character that is not a word character from the basic Latin alphabet. Equivalent to [^A-Za-z0-9_]. For example, /\W/ or /[^A-Za-z0-9_]/ matches "%" in "50%" and "É" in "Émanuel". |

See full Charater List here [Mozilla Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Character_Classes).

### Flags
A regular expression (also “regexp”, or just “reg”) consists of a pattern and optional flags.

There are two syntaxes that can be used to create a regular expression object.
The “long” syntax:

```sh 
regexp = new RegExp("pattern", "flags");
```

And the “short” one, using slashes "/":

```sh 
regexp = /pattern/; // no flags
regexp = /pattern/gmi; // with flags g,m and i (to be covered soon)
```

### Grouping and Capturing
A part of a pattern can be enclosed in parentheses (...). This is called a “capturing group”.

That has two effects:

It allows to get a part of the match as a separate item in the result array.
If we put a quantifier after the parentheses, it applies to the parentheses as a whole.

```sh
alert( 'Gogogo now!'.match(/(go)+/ig) ); // "Gogogo"
```

Parentheses group characters together, so (go)+ means go, gogo, gogogo and so on.

### Bracket Expressions
Brackets indicate a set of characters to match. Any individual character between the brackets will match.

```sh
'elephant'.match(/[abcd]/) // -> matches 'a'
```

You can use the ^ metacharacter to negate what is between the brackets.

```dh
'donkey'.match(/[^abcd]/) // -> matches 'o'
```

### Greedy and Lazy Match
The standard quantifiers in regular expressions are greedy, meaning they match as much as they can, only giving back as necessary to match the remainder of the regex. By using a lazy quantifier, the expression tries the minimal match first.

Lazy Search — will try to match the smallest possible string. Append ```?``` to make the search lazy.

### Boundaries
The ```\b``` is an anchor like the caret ```^``` and the dollar sign ```$```. It matches a position that is called a “word boundary”. The word boundary match is zero-length.

### Back-references
Backreferences match the same text as previously matched by a capturing group. Suppose you want to match a pair of opening and closing HTML tags, and the text in between. By putting the opening tag into a backreference, we can reuse the name of the tag for the closing tag. Here’s how: ```<([A-Z][A-Z0-9]*)\b[^>]*>.*?</\1>```. This regex contains only one pair of parentheses, which capture the string matched by ```[A-Z][A-Z0-9]*```. This is the opening HTML tag. (Since HTML tags are case insensitive, this regex requires case insensitive matching.) The backreference ```\1``` (backslash one) references the first capturing group. ```\1``` matches the exact same text that was matched by the first capturing group. The ```/``` before it is a literal character. It is simply the forward slash in the closing HTML tag that we are trying to match.

### Look-ahead and Look-behind
Sometimes we need to find only those matches for a pattern that are followed or preceeded by another pattern.

##### Lookahead
The syntax is: X(?=Y), it means "look for X, but match only if followed by Y". There may be any pattern instead of X and Y.
For an integer number followed by €, the regexp will be \d+(?=€):
```sh
let str = "1 turkey costs 30€";
alert( str.match(/\d+(?=€)/) ); // 30, the number 1 is ignored, as it's not followed by €
```

##### Lookbehind
Lookahead allows to add a condition for “what follows”.

Lookbehind is similar, but it looks behind. That is, it allows to match a pattern only if there’s something before it.

The syntax is:

Positive lookbehind: ```(?<=Y)X```, matches ```X```, but only if there’s ```Y``` before it.
Negative lookbehind: ```(?<!Y)X```, matches ```X```, but only if there’s no ```Y``` before it.
For example, let’s change the price to US dollars. The dollar sign is usually before the number, so to look for $30 we’ll use ```(?<=\$)\d+``` – an amount preceded by ```$```:

```sh
let str = "1 turkey costs $30";
// the dollar sign is escaped \$
alert( str.match(/(?<=\$)\d+/) ); // 30 (skipped the sole number)
```

## Author
Ted Pedersen [Ted Pedersen](https://github.com/tedpedersen)
