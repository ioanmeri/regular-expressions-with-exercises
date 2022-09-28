# Character Classes

## Whitespace Character Classes

We've been working with a character that represents a character class

- `.` means any character except a newline
- it's a pretty expansive class

Last lecture, we created a regex for "spaces at the start of the string"

- `/^ +.*/`

What if we wanted any whitespace, not just the space character?

- you could make a collection: `[\r\n\t\v\f]`
- or use a **character class**: `\s`

**Example**

Find whitespace at the beginning of a string

```
/^\s+/
```

`^` : Anchor to the beginning of the string
`\s+`: One or more whitespace characters (`[\r\n\t\v\f]`)

```
  whitespace // matches the whitespace
          more // all tabs and whitespace matches
```

### What about the inverse ?

What if you wanted strings that **didn't** start with whitespace?

- `\S` = character that is not whitespace
  - `[^\r\n\t\v\f]`
  - common pattern: capital letter means the inverse

**Example**

Find non-whitespace at he beginning of a string

```
/^\S+/
```

`^`: Anchor to the beginning of the string

`\S+`: One or more **non-whitespace** characters (`[^\r\n\t\v\f]`)

```
  whitespace    // not
          more  // not
no whitespace here, no sirree! // matches no until whitespace
```

---

## More Character Classes

| Symbol | Description        | Collection                                                  |
| ------ | ------------------ | ----------------------------------------------------------- |
| \s     | whitespace         | `[\r\n\t\v\f]`                                              |
| \S     | not whitespace     | `[^\r\n\t\v\f]`                                             |
| \d     | digit              | `[0-9]`                                                     |
| \D     | not digit          | `[^0-9]`                                                    |
| \w     | word character     | `[0-9A-Za-z_]`                                              |
| \W     | not word character | `[^0-9A-Za-z_]`                                             |
| \b     | word boundary      | before the `\b` is `\w` and after is `\W` or vice versa     |
| \B     | not word boundary  | before and after the `\B` are either both `\w` or both `\W` |

---

## Word Boundary

Word boundary (`\b` or `\B`) do **not** consume characters

- like `^` and `$`

**Example**

```
/\bstem\b/
```

`\b` : Anchor at a word boundary

stem: The string stem

`\b` Anchor at a word boundary

```
That rose has a lovely stem! // matches "stem"
Look at that solar system!  // no match, before stem there is y: word character, no word boundaries on both sides of "stem"
```

```
/\Bstem\b/
```

```
That rose has a lovely stem! // no match
Look at that solar system!  // matches "stem"
```

it has a word character in front

```
/\Bstem\B/
```

```
That rose has a lovely stem! // no match
Look at that solar system!  // no match
Operating systems are fascinating // matches "stem"
```

---

## Word Boundary

What's the difference between `/\bstem\b/` and `/\Wstem\W/`?

- the `\b` version does not include the "boundary" characters in the match
- the `\b` version works if there's no actual character at the boundary
  - beginning or end of string

**Example**

**stem** surrounded by non-word characters

```
/\Wstem\W/
```

`\W` : A non-word character

stem : The string stem

`\W` : A non-word character

This a consuming character

```
stem // not, need 6 characters to match
stem stem! // match
stem stem! stem! // matches last 2 stem!
stem stem! stem // only middle matches
```

With word boundaries:

```
/\bstem\b/
```

```
stem stem! stem // all stem match
```

---

## Another Character Class Example

Find a string that

- starts with a word character
- ends with anything that's not a digit

```
/^\w.*\D$/
```

`^\w` : String starts with a single word character (`[0-9A-Za-z_]`)

`.*` : Followed by zero or more non-newline characters

`\D$` : Ending with a non-digit character (`[^0-9]`)

```
stem stem! stem   // match
_i                // match
pennsylvania 6-50000 // not
-0_i // not
 0_i // not
!0_i // not
```

---

## Exercises

```
// Exercise 14: Match two or more o’s, but only if they’re in the
// middle of a word.
// Do not include any characters other than the o’s in the match
const middleOoRegex = /\Bo{2,}\B/;
```

```
// Exercise 15: Match list item strings that start with one or more digits
// followed by a ) .Example string to match:
// 1) make breakfast. 2) eat breakfast. 3) go to work.
// Match the entire contents of each list item string (not just the
// digit(s) and parenthesis).
const listItemRegex = /\d\).*$/;
```

```
// Exercise 16: Match any whitespace at the end of a string
// Do not include characters other than the whitespace in the match
// Do not match strings that don’t have whitespace at the end
const trailingWhitespaceRegex = /\s+$/;
```

```
// Exercise 17: Find any phrase that matches ____ the ____
// That is, one word before and after "the" (without quotes).
// Don't match any non-word characters before or after the matched
// string.
const blankTheBlankRegex = /\w+ the \w+/;
```

```
// Exercise 18: Make a simplified email address matcher with these rules:
// - One or more word or period (.) characters before the @
// - At least one  period (.) after the @
// - The string should contain only the email address and no
//   surrounding characters
//
// Interested in unsimplified? See http://emailregex.com/
const emailRegex = /^[\w\.]+@[\w\.]+.[\w]+$/;
```
