# Whitespace Characters and String Boundaries

## Multi-tasking Backslash (\\)

- backslash can either mean "literally the next character"
  - for characters with special meanings, like `.` (`\.`)
- OR "combined with the next character, this is a special token"
  - used with characters that DON'T have special meaning, like `t` (`\t`)
  - for whitespace characters that are hard to include otherwise
  - for character classes (next section)
- Backslashes make special characters un-special
  - and un-special characters special

---

## Whitespace Characters

| Symbol | Description     |
| ------ | --------------- |
| \t     | tab character   |
| \n     | newline         |
| \r     | carriage return |
| \f     | line feed       |
| \v     | vertical tab    |

- Esoteric answer:
  - mechanical printer
  - carriage return brings you to beginning of the line
  - a "line feed" gets you to the next line
    - but in the same place your were
  - Line feed + carriage return takes you to the beginning of next line
- Practical answer:
  - New lines from Windows have two characters: `\r\n`
  - New lines for most other systems (MacOS, Linux) are simply `\n`

```
/\tb/
```

```
 b  // not (space b)
  b // match (tab b)
```

## What about a Space Character ?

- No backslash needed!
- Search for strings that:
  - start with one or more space characters
  - followed by any number of characters that aren't newlines
    - `/ +.*/`

```
/ +.*/
```

```
petter piper picked a pack of pickled peppers // match all from piper and after
```

- That matches every space and the following word
- What if we wanted spaces only at the **start** of the string?

---

## String Boundaries

| Symbol | Description                   |
| ------ | ----------------------------- |
| ^      | anchor at the start of string |
| $      | anchor at the end of string   |

- Why "anchor"?
  - These tokens don't consume characters
  - They don't match a character
    - just indicate where the regex is within the string
- Double-duty caret(^)
  - Also means "negation" for character collections (for example, [^246])

---

**Example**

`/^ +.*/`

- `^` Anchor at the start of the string
- `_+` One or more space characters
- `.*` Zero or more of any character but newline (`\n`)

```
_         // match
_______   // match
__________start typing this is a string starting with a space character // matches all
only space in the middle // no match
```

### Is the `.*` necessary ?

- We'd still match strings starting with spaces without it
- Are we **interested** in the rest of the string?
  - only interested in spaces
    - say, to replace
    - no need for `.*`
  - say, lines that start with space indicate certain type of data
  - want to include the `.*` in the match

### Is th + necessary ?

- Who cares if there are **multiple** spaces?
  - if you simply want to **find** strings starting with multiple spaces
    - this will work: `/^ /`
- Often you want to **replace** one or more space characters
  - with a single space (remove multiple spaces)
  - even an empty string (remove all spaces at the beginning of the string)
- In this case, need to use + to capture all the spaces
- Talk about replacement in the substitution lecture

---

## End of String

- Very similar: use `$` to anchor regex at the end of the string.
- Remember our sentence finder?
  - `/[A-Z][^\.!\?]+[\.!\?]/`
- Here the sentence can be anywhere in the string
- Let's anchor it so that the entire string is only one sentence
  - at defined by above, not a complete definition

```
/[A-Z][^\.?!]+[\.?!]/
```

```
This is a sentence. // match
This is another sentence! Is this a sentence? // match both 2
```

**Example**

```
/^[A-Z][^\.?!]+[\.?!]$/
```

- `^` : Anchor to the start of the string
- `[A-Z]`: Exactly one capital letter from A through Z inclusive
- `[^\.?!]+` : One or more of any character but these: . ? !
- `[\.?!]`: Exactly one of any of these
  - the literal `.` character, the ? character, or the `!` character
- `$`: Anchor to the end of the string

```
This is a sentence. // match
This is another sentence! Is this a sentence? // not match
```

with the 2 anchors, it's looking at the string as a whole

---

## Exercises

```
// Exercise 9: Match all strings with one or more space
// characters at the end, preparing to replace them
// with an empty string. Contents of the string before
// the spaces is not important
const trailingSpaceRegex = / +$/;
```

```
// Exercise 10: Match strings that contain at least two tabs in a row
// anywhere in the string. Contents of the string are unimportant.
const twoConsecutiveTabsRegex = /\t\t/;
```

```
// Exercise 11: Match strings that contain at least two tabs, not
// necessarily in a row, anywhere in the string.
// Contents of the string are unimportant.
const twoTabsRegex = /\t.*\t/;
```

```
// Exercise 12: Match strings that start with at least
// three digits from 0 to 5 (inclusive).
// Contents of the string are unimportant.
const startWithThreeDigitsRegex = /^[0-5]{3,}/;
```

```
// Match entire strings that are six characters or longer and
// don't contain the letter `E` (capital or lowercase)
const stringsWithoutERegex = /^[^Ee]{6,}$+/;
```
