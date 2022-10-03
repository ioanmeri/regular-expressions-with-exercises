# Multi Character Strings Quantifiers and Options

## Multi-character String Tokens

Up until now, all tokens have been one character

- h, \s etc

Can add quantifiers: \* + ?

Can select between multiple options: []

This section: multi-character tokens

- how do we add quantifiers?
- how do we select between multiple options?

**Example**

Any of three animals

```
/kittens/foals/ducklings/
```

kittens: the string kittens

`|` : "or" character for multi-character options

foals: the string foals

`|`: "or: character for multi-character options

ducklings: the string ducklings

---

## Groups with Multi-Character Strings

1. Group syntax: `( )`

2. Multiple multi-character options for a regex that contains other parts

- Example: match either I want a kitten or I want a puppy

3. Quantifiers for multi-character strings

- Example: multiple kitten

More uses for groups! Will discuss in future sections

```
/I love (kittens|foals|ducklings)/
```

I love: the string: "I love "

`( )`: group containing multiple multi-character options

kittens`|`foals`|`ducklings: three options, separated by `|`

```
I would like to adopt some kittens // not
The petting zoo had ducklings and ponies // not
foals are young horses // not
I love kittens // match
can you believe how much I love foals // match
She and I love ducklings. // match
```

**Example**

One or more of the string "kittens"

```
/(kittens)+/
```

`( )+` : One or more of the contents of this group
kittens: The string kittens

```
/(kittens)/+
```

```
I love kittenskittenskittens kittens // 2 match
```

---

## Different punctuation

- No container: `/kittens+/`

  - quantifier applies only to last letter
  - the string kitten followed by one or more s characters

- Square brackets: `/[kittens]+/`

  - quantifier applies to the collection
  - one or more of any of the characters k i t e n s
  - double t is meaningless

- Parentheses: `/(kittens)+/`
  - quantifier applies to the group
  - one or more of any of the string kittens

---

## Example

Match one or more non-overlapping WOW or W0W

```
/(W[0O]W)+/
```

`( )+` : One or more of the contents of this group

W: The character W

`[O0]`: Either the character 0 or 0

W: The letter W

```
WOW W0WW0WWOWW0W // match
```

---

## 24 hour clock

Digital clock that shows 24 time

- Valid: 5:43, 15:08, 23:12
- Invalid: 28:33, 41:12, 12: 89

Rules

- first number, first digit is nothing, 1 or 2
  - but if it's 2, the second digit can only be 0 - 3
- second number is two digits, between 00 and 59

```
/(1?\d|2[0-3]):[0-5]\d/
```

`( )`: Group for multi-character options

`1?\d`: First multi-character option: one or two digit number from 0 to 19

`|`: "or" character for multi-character options

`2[0-3]`: Second multi-character option: 20 - 23

`:` : A colon character

`[0-5]\d: A two-digit number between 00 - 59 (inclusive)

---

## Exercises

```
// Exercise 29: Match strings that contain either `puppy` or
// `puppies` (no quotes, case sensitive)
const puppyOrPuppiesRegex = /pupp(y|ies)/;
```

```
// Exercise 30: Match a string whose only contents represent
// a playing card. This would be the card number, which is
// - a choice of 2, 3, 4, 5, 6, 7, 8, 9, 10, J, Q, K, A
// - a space character
// - the word `of` (without the quotes)
// - a space character
// - the suit (hearts, spades, diamonds, or clubs)
const playingCardRegex = /%([2-9JQKA]|10) of (hear|spade|diamond|club)s%/;
```

```
// Exercise 31: Test whether a string is a valid hex web color:
// The string must start with a `#` (no quotes)
// then contain 3 or 6 (but not 4 or 5) hex digits
//
// alphabetical hex digits can be lower or uppercase
// The hex string should comprise the entire string.
const hexStringRegex = /^#([\dA-F]{3}){1,2}$/i;
```

```
// Exercise 32: In a log file, parse out all lines that contain
// `ERROR` or `FATAL` (no quotes). No need to capture which one
// (ERROR or FATAL) you found in the line.
// Example log contents:
//
// 2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
// 2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
// 2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
// 2012-02-03 18:35:34 SampleClass3 [WARN] missing id 423340895
// 2012-02-03 18:35:34 SampleClass5 [TRACE] verbose detail for id 2082654978
// 2012-02-03 18:35:34 SampleClass0 [ERROR] incorrect id  1886438513
// 2012-02-03 18:35:34 SampleClass9 [TRACE] verbose detail for id 438634209
// 2012-02-03 18:35:34 SampleClass8 [DEBUG] detail for id 2074121310
const errorFatalRegex = /^.*(?:ERROR|FATAL).*$/gm;
```

---
