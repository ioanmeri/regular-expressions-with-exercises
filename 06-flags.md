# Flags

First really platform-dependent feature

- for example: PHP and Go use a flag for greedy / lazy (next lecture)
- whereas JavaScript and Python use syntax within the regular expression

This section will cover flags that are nearly universal

- notable exception: grep (Linux)
- see grep example files

---

## Flags in this Course

| Symbol | Meaning          | Description                                                                                                   |
| ------ | ---------------- | ------------------------------------------------------------------------------------------------------------- |
| g      | global           | Match as many times as you can (without flag = only once)                                                     |
| m      | multi-line       | match ^ and $ at the beginning and end of each line, not the end of string                                    |
| i      | case-insensitive | Match upper or lower case versions of letter                                                                  |
| s      | single line      | Where . is concerned, treat the string as a single line (. matches newline). Matches can span multiple lines. |

---

## Syntax

When using `//` for regex, flags go after the second `/`

**Example**

```
/^\s+/gm
```

means

- match one or more whitespace characters at the beginning of each line
- match as many times as you can
- Match ^ at the beginning of each _line_ not just the beginning of the string

---

## Example no flags

String starts with "Hello" and ends with "bye!"

Flags: none

```
/^Hello.*bye!$/
```

`^Hello` : Sting starts with the string Hello

`.*` : Followed by zero or more non-newline characters

`bye!$` : String ends with the string bye!

no g flag : Return only first match

```
Hello. Goodbye!
Hello, how are you doing today? Hope you're having a good one. Goodbye!
Hello! It's been a great talking to you. Bye!
Hello, so good to see you. Seeya later!
Goodbye!
```

Zero matches because it many new line characters in the middle (`.*`)

---

## Example m flag

```
/^Hello.*bye!$/m
```

`^Hello` : **Line** starts with the string Hello

`.*` : Followed by zero or more non-newline characters

`bye!$` : **Line** ends with the string bye!

```
Hello. Goodbye! // match
Hello, how are you doing today? Hope you're having a good one. Goodbye!
Hello! It's been a great talking to you. Bye!
Hello, so good to see you. Seeya later!
Goodbye!
```

---

## Example mg flags

```
/^Hello.*bye!$/mg
```

`^Hello` : **Line** starts with the string Hello

`.*` : Followed by zero or more non-newline characters

`bye!$` : **Line** ends with the string bye!

g flag: Return all non-overlapping matches.

```
Hello. Goodbye! // match
Hello, how are you doing today? Hope you're having a good one. Goodbye! // match
Hello! It's been a great talking to you. Bye!
Hello, so good to see you. Seeya later!
Goodbye!
```

---

## Example mgi flags

```
/^Hello.*bye!$/mgi
```

`^Hello` : **Line** starts with the string Hello (case insensitive)

`.*` : Followed by zero or more non-newline characters

`bye!$` : **Line** ends with the string bye! (case insensitive)

g flag: Return all non-overlapping matches.

```
Hello. Goodbye! // match
Hello, how are you doing today? Hope you're having a good one. Goodbye! // match
Hello! It's been a great talking to you. Bye! // match
Hello, so good to see you. Seeya later!
Goodbye!
```

---

## Example mgis

```
/^Hello.*bye!$/mgis
```

`^Hello` : **Line** starts with the string Hello (case insensitive)

`.*` : Followed by zero or more characters (**including \\n**)

`bye!$` : **Line** ends with the string bye! (case insensitive)

g flag: Return all non-overlapping matches.

```
Hello. Goodbye! // match
Hello, how are you doing today? Hope you're having a good one. Goodbye! // match
Hello! It's been a great talking to you. Bye! // match
Hello, so good to see you. Seeya later!
Goodbye!
```

ONE match

---

## Exercises

```
// Exercise 19: Match he or hey once, with any capitalization.
// The he or hey may be anywhere in the string; only match
// the he / hey part.
const heHeyRegex = /hey?/i;
```

```
// Exercise 20: Capture all the words that start with "se"
// (without quotes) in a string (case insensitive)
const seStartRegex = /se[^ .*]+/gi;
const seStartRegex2 = /\bse\w+\b/gi;
```

```
// Exercise 21: Given this block of text (the last four
// lines of Robert Frost’s Stopping by the Woods on a
// Snowy Evening), find all the lines that end with eep.
// (including the period). Capture the entire line.

// The woods are lovely, dark, and deep,
// But I have promises to keep,
// And miles to go before I sleep,
// And miles to go before I sleep.
const eepRegex = /^.*eep\.$/gm;
```

```
// Exercise 21: Using the same poem lines as above, find only the
// first phrase on a single line that starts with "to" and
// ends with "eep" (without quotes).
const toEepRegex = /to.*eep/i;
```

```
// Exercise 23: Using the same poem lines as above, capture only
// the first word that starts with an a (it could be a capital
// or lower case a)
const firstARegex = /a[\w]+/i;
const firstARegex2 = /\ba\w+/i;
```

```
// Using the same poem lines as above, find the first
// phrase that starts and ends with "and" (no quotes, case
// doesn’t matter). The phrase may span multiple lines.

// Want to know how to find the *shortest* first phrase? That's
// next lecture: greedy vs lazy!
const andBookendsRegex = /and.*and/is;
```
