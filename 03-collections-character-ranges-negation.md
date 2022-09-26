# Collections, Character Ranges and Negation

## Collections

**Hexadecimal**

- Base 16
  - every digit can be a number from 0 through 15
  - just like base 10 digits are from 0 through 9
  - and base 2 (binary) digits are from 0 through 1
- For hexadecimal (hex for short):
  - 0 - 9 can use numbers
  - after that, A = 10, B = 11 ... F = 15
- Example hex numbers
  - 2F, C70, ABCDE4

**Collection of Characters**

- Use square brackets []
  - means "one of any of these characters"
  - one character after the other with no separation
  - can add quantifier (+ \* ?) after to indicate how many from collection
- For hexadecimal:
  - `/[0123456789ABCDEF]+/`

`/[0123456789ABCDEF]+/gm`

```
C70       // match
234B7C    // match
ABCE12    // match
NH4       // only the 4
```

---

## Character Ranges

- Inside a collection (square brackets [ ])
- Use a hyphen (-) to create a range
- Instead of `/[0123456789ABCDEF]+/`
- Can use `/[0-9A-F]+/`
- Later we'll replace 0-9 with a token meaning "any digit"
  - In the character classes section

**Example**

`/[0-9A-F]+/`

| Symbol | Description                                           |
| ------ | ----------------------------------------------------- |
| [ ]+   | One or more of the characters represented between [ ] |
| 0 -9   | Any digit from 0 through 9 (inclusive)                |
| A-F    | Any capital letter from A through F (inclusive)       |

```
C70       // match
234B7C    // match
ABCDE12   // match
NH4789    // only the numerical part not the alphabetical
```

---

## Negation

- Negating an entire collection
- Use the caret (`^`) inside character collections (`[ ]`)
- Means "anything BUT the characters in this collection"

`[^a-z4]`

- Anything but lowercase letters a through z or the number 4

**Example: Match a sentence**

starts with a capital letter / no "sentence punctuation" (. ? !) in the middle / sentence punctuation at the end

```
/[A-Z][^\.?!]+[\.?!]/
```

| Symbol  | Description                                                                                     |
| ------- | ----------------------------------------------------------------------------------------------- |
| [A-Z]   | Exactly one capital letter form A through Z inclusive                                           |
| [^ ]+   | One or more of any character NOT in this collection                                             |
| \\.?!   | The literal `.` character, the `?` character, or the `!` character                              |
| [\\.?!] | Exactly one of any of these: the literal `.` character, the `?` character, or the `!` character |

```
Do you want to get coffee with me ? // match
I love kittens! // match
This is the best course ever. Thy are you laughing? // both sentences match
```

---
