# Substitution / Replace

Replace matched part of string with

- fixed replacement
- replacement that references or transforms match or group
  - Fine print: syntax is platform dependent, here JS!

**Example**

Replace cute animal with the string "cutie"

Regular expression

```
/kitten|puppy|piglet|foal|fawn|duckling|click/
```

-> Click on replacement tool of `regex101`

---

## Example

Replace cute animal with the string "cute little <animal>"

Regular expression with group

- `/(kitten|puppy|piglet|foal|fawn|duckling|chick)/`

Reference group in replacement

- cute little $1

-> Click on replacement tool of `regex101` and type: cute little $1

---

## Remove Regular Expression Matches from String

Remove asterisks and underscores from a string

Regular expression:

```
/[*_]/
```

Replace with empty string

- ''

---

## Exercises

```
// Write your regular expressions within the // delimiters.
//     Example: /a+/
// Add flags after the last / in the regular expression.
//     Example: /a+/gm
// Group contents *within the regex* can be accessed with the syntax \groupnumber
//     for example, to reference the first group within the regex, use $1
// Group contents *within the replacement string* can be accessed with the syntax $groupnumber
//     for example, to reference the first group within the replacement, use $1
```

```
// Exercise 36: Replace all email addresses (using the
// simplified pattern description from exercises in
// character categories section) with the text
// `<<redacted>>` (no quotes)
//
// For reference, here's the description of the exercise for
// simplified email address:
// One or more word characters or . before the @, and at
// least one period after the @
//
// NOTE: In this exercise, the string may contain text other
// than the email address(es).
const emailRegex = /\b[\w\.]+@[\w\.]+\.[\w]+\b/g;
const emailReplacement = '<<redacted>>';
```

```
// Exercise 37: Strip html tags out of text
// For example, <b>bold statement</b> should become
// "bold statement" (no quotes).
// Note: strings may be multi-line
const htmlTagRegex = /<.+?>/gm;;
const htmlTagReplacement = '';
```

```
// Exercise 38: Substitute any multiple whitespace
// characters (including newlines) with a single
// space character
//
// IMPORTANT NOTE: The engine that Udemy uses to compile its JavaScript code does not support
// the 's' flag, so this exercise throws an error "SyntaxError: Invalid flag supplied to RegExp
// constructor." Please skip this exercise in the Udemy code exercise platform, and instead check
// your answer in the course repository.
// https://github.com/bonnie/udemy-regex-syntax-examples/tree/master/exercises/javascript/9_substitution
//
// --------> const multipleWhitespaceRegex = /\s{2,}/sg;;
// --------> const multipleWhitespaceReplacement = ' ';
```

```
// Exercise 39: Strip off any whitespace characters
// from the beginning or end of a string, but let the
// whitespace in the middle stand.
// You may assume single-line strings
// Note: This one is tricky! How do you keep the "middle"
// part from gobbling up the outer space, and yet
// still respect spaces in the middle?
const outerWhitespaceRegex = /^\s*(.*\S)\s*$/;;
const outerWhitespaceReplacement = '$1';
```

```
// Exercise 41: Building on the last exercise: Put last name
// first even if there are more than two words in the name
// (you may assume last name is only the last word)
// For example,
//   Carlos Alberto Torres → Torres, Carlos Alberto
//   Harriet Beecher Stowe → Stowe, Harriet Beecher
const multiNameSwitchRegex = /^([\w ]+) ([\w\-\']+)$/mg;;
const multiNameSwitchReplacement = '$2, $1';;
```
