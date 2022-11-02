# Regex Tutorial

Welcome to my Regex guide where I am going to do my best to explain each and every individual aspect of Regular Expressions, also known as Regex. Regex is a powerful search tool programmers use to match specific sequences of text inside a string. Regex is its own programming language meaning it may be used alongside other languages like JavaScript, Python, or PHP, and is not exclusive to any one language. Understanding Regex is important because it gives us control over whatever is inside our strings. We are able to search for specific sequences and label groups of strings like an area code of a phone number.

## Summary

The regular expression I will be using as an example is:

* Matching a URL: `/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

This example will cover Anchors, Bracket Expressions, Quantifiers, Grouping Constructs, Character Classes, and Character Escapes. The other categories such as The OR Operator and Flags will be discussed outside of this example.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [Character Escapes](#character-escapes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Lookaround Assertions](#lookaround-assertions)

## Regex Components

### Anchors

**Anchors** are the characters `^` and `$` and are used to signify text that comes before or after these characters. 

- `^` signifies a string that begins with the characters that follow it
- `$` signifies the opposite. A string that ends with the characters that precede it

So in the example: 

* Matching a URL: `/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

The `^` character signifies the beginning of the string. The regex will match a string that begins with `https://` or `http://` as the `s` is optional OR the group that follows because this `http(s)://` group is optional.

The `$` character signifies the end of the string. The regex will match a string that ends with the group `([\/\w \.-]*)*` OR a `/` if it is included.

I will explore each of these groups in depth later in this tutorial.

### Quantifiers

**Quantifiers** specify how many instances of a character, group, or character class must be present in the input for a match to be found. Quantifiers include:

- `*` --------> Matches zero or more times
- `+` --------> Matches one or more times
- `?` --------> Matches zero or one time (also thought of as optional)
- `{ n }` ---> Matches exactly n times
- `{ n,}` ---> Matches at least n times
- `{ n, m }`> Matches from n to m times

The variables `n` and `m` are integer constants.

So in the example:

* Matching a URL: `/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

Group 1: `(https?:\/\/)?` has two **quantifiers**. The first `?` applies to the `s` character. It tells the expression that this `s` character is optional. If there is an `s` here, include it in the search. If not, the expression continues.

The `?` at the end tells the expression that this entire group is optional. The expression will include `http(s)://` if it exists.

Group 3: `([a-z\.]{2,6})` includes the **quantifier** `{2,6}` and tells the expression to match the preceeding sequence only if it occurs 2 - 6 times. 

Group 4: `([\/\w \.-]*)*\/?` includes three **quantifiers**. The first and second `*` is telling the expression to include the preceeding sequence if it exists, or if it repeats, include every repitiion. Next the `?` tells the expression to include the preceeding group if it exists.

### Grouping Constructs

**Grouping Constructs** are used to break up a string into subexpressions to check if different subexpressions fulfill different requirements. This is useful when we have a complicated string that can be broken up into smaller pieces. To use **grouping constructs**, include a subexpression contained inside parenthesis `()` .

**Grouping Constructs** are also used to **store information** and therefore can be named and recalled. By default groups are named `Group` followed by an incrementing integer. 

**Grouping Constructs** have two primary categories: **capturing** and **non-capturing**. The main difference between these two categories is **capturing** constructs store information that can be recalled later, whereas **non-caputring** constructs do not store information. 

Lastly, **grouping constructs** may use **lookaround assertions** may be used to find a string before or after a given sequence. Lookaround assertions have several types including: positive lookahead, negative lookahead, positive lookbehind and negative lookbehind. These are used to match a string that comes after a given sequence without including the given sequence.

This tutorial will not cover advanced grouping constructs. For more information about how grouping constructs store information, or capturing vs non-capturing constructs, or lookaround assertions, visit the link below.

[Grouping Constructs in Regular Expressions](https://learn.microsoft.com/en-us/dotnet/standard/base-types/grouping-constructs-in-regular-expressions)

So in the example:

* Matching a URL: `/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

- Group 1: `(https?:\/\/)`    

- Group 2: `([\da-z\.-]+)`    

- Group 3: `([a-z\.]{2,6})`   

- Group 4: `([\/\w \.-]*)`    

None of these groups are explicitly named, meaning if we want to recall them we need to use the `\#` operator where `#` is the number of group.

### Bracket Expressions

**Bracket Expressions** represent a range of characters and have an implicit OR operator bewteen each range. So for example:

- `[ab]` means a range of either `a` or `b`
- `[a-z]` means a range of all the characters between a-z (lowercase)
- `[0-9]` means a range of all the characters bewtween 0-9
- `[a-zA-Z0-9_-]` means a range of characters between a-z, or A-Z, or 0-9, or `_` or `-`

**Bracket Expressions** will only match one character unless we use a **quantifier** to denote that we want to capture more than one character inside the range. 

It is important to note that `^` **negates** the folowing range of characters. For example, `[^a-z]` will match every character that is **NOT** a lowercase letter between a-z.

So in the example:

* Matching a URL: `/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

- Group 2: `([\da-z\.-]+)` includes a range of characters of `\d`, or a-z, or `.` or `-`. Because the `+` quantifier is included in the group, this expression will match one or more of these characters inside the range.

- Group 3: `([a-z\.]{2,6})` is similar to the previous example, but with less specifications. It includes a range of characters between a-z, or `.` with a quantity between 2 - 6 repetitions.

- Group 4: `([\/\w \.-]*)` includes all characters inside the range of either `\/`, or `\w`, or `space`, or `.`, or `-`. the `*` quantifier includes this range in the group zero or more times, making it optional, but if it repeats include every repetition of a character inside this range.

### Character Classes

A **Character Class** represent a kind of character such as, for example, digits or letters. There are positive and negative character classes as well as advanced character classes that are not often used. Some common character classes include:

- `.` Matches any character except the newline character `\n`

- `\d` Matches any Arabic numeral digit. This class is equivalent to the bracket expression `[0-9]`

- `\w` Matches any alphanumeric character from the basic Latin alphabet, including the underscore `_`. This class is equivalent to the bracket expression `[A-Za-z0-9_]`.

- `\s` Matches a single whitespace character, including space, tab, form feed, line feed, and other Unicode spaces

It is important to note that `\d`, `\w`, and `\s` each have **negative** versions of themselves which represent anything that is **NOT** included in the positive version. For example, `\d` matches anything that is **NOT** a Arabic numeral digit or `[^0-9]` in bracket expression.

So in the example:

* Matching a URL: `/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

- Group 2: `([\da-z\.-]+)` includes the character class `\d` inside a bracket expression

- Group 4: `([\/\w \.-]*)` includes the character class `\w` inside a bracket expression

### Character Escapes

**Character Escapes** are usedful we need to match a character having special meaning in regex such as `{` or `+`. Simply use a backslash `\` to escape the special significance of a character (or to apply it).

So in the example:

* Matching a URL: `/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

- Group 1: `(https?:\/\/)` uses two `\/` character escapes so as to include the forwardslash in the expression.

- Group 2: `([\da-z\.-]+)` uses a `\.` character escape to include the `.` in the search.

- Group 3: `([a-z\.]{2,6})` uses the same character escape as Group 2.

- Group 4: `([\/\w \.-]*)` uses `\/` and `\.` 

- Between the groups we have some character escapes as well; `\.` in the middle and `\/` and the end.

### The OR Operator

**The OR Operator** refers to the `|` character which self-explanatory. An example of this in use: `four|4` matches strings `"four"` or `"4"`. Bracket expressions have this operator implicitly stated as apart of their functionality as stated in the **Bracket Expressions** section above. 

So in the example:

* Matching a URL: `/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

- There are no OR Operators

### Flags

**Flags** affect the search and are placed at the very end of the regular expression. There are six different flags: 

- **`i`** With this flag the search is case-insensitive: no difference between A and a

- **`g`** With this flag the search looks for all matches, without it – only the first match is returned

- **`m`** Multi-line search: a multi-line input string should be treated as multiple lines

- **`s`** Enables “dotall” mode, that allows a dot `.` to match newline character `\n`

- **`u`** Enables full Unicode support. The flag enables correct processing of surrogate pairs

- **`y`** “Sticky” mode: searching at the exact position in the text

So in the example:

* Matching a URL: `/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

There is no flag, but if there were it would be placed at the very end of the expression after the last `/`.

## Author

I hope this tutorial was helpful for you!

Nathaniel Mays (Junior Web Developer)

[My Github Profile](https://github.com/NateDogg97)
