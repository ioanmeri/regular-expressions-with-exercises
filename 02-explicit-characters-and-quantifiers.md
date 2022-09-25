# Explicit Characters and Quantifiers

## Regex with letters only

Can search an exact string

```
/hey/gm
```

```
hey   / /matches
they  // matches
Hey   // not match
h e y // not match
```

---

## Quantifiers

Determine how many times a character can appear in a row

| Symbol | Description             |
| ------ | ----------------------- |
| ?      | zero(0) or one(1) times |
| +      | one(1) or more times    |
| \*     | zero(0) or more times   |

```
/he+y/gm
```

| Letter | Description                                             |
| ------ | ------------------------------------------------------- |
| h      | The letter h                                            |
| e+     | Immediately followed by **one or more of the letter e** |
| y      | Immediately followed by the letter **y**                |

```
hey         // match
they        // match
Hey         // not
h e y       // not
heeeeeeeeey // match
heey        // match
hy          // not
```

**Example**

Match 'Kitten!' or 'Kitterns!' but not 'Kittenssss!'

```
/Kittens?!/
```

**Example**

'Kittens' with any number of exclamation points (including zero)

```
/Kittens!*/
```

```
Kittens             // match
Kittens!!!!!        // match
Kittens!            // match
Kittens!!!!!!!!!!!! // match
```

---

## Escaping Special Characters

Some characters have other meaning ("special characters")

- already met quantifiers: + ? \*
- . is a special character too
  - means "any character except a newline"
- Match special characters **literally** with "backslash escape": `\'
- Characters that need to be escaped:
  - \+ ? \* . { } [ ] ( ) ^ $
- The fine print: in some situations characters don't need to be escaped

**Example**

Meh kittens: 'Kittens' followed by one or more periods (.)

```
/Kittens\.+/
```

Kittens, The string Kittens

`\.+`: Followed immediately by **one or more literal . character**

---

## Curly Brace { } Quantifiers

Maybe 'Kittens..............' is too many dots

What if we want between 1 and 3 dots?

- {1,3} = between one and three times (inclusive)
- {3} = exactly three times
- {3,} = three or more times

```
/Kittens\.{1,3}/
```

`\.{1,3}`: Followed immediately by **one to three literal . characters**

```
Kittens............... // match
Kittens!!!!
Kittens!
Kittens!!!!!!!!!!!!!
Kittens.              // match
Kitetens..            // match
```

**Example**

'kittens' followed by exactly three periods (.)

```
/Kittens\.{3}/
```

`\.{3}`: Followed immediately by **exactly three literal . characters**

```
Kittens............... // match
Kittens!!!!
Kittens!
Kittens!!!!!!!!!!!!!
Kittens...            // match
Kitetens..
```

**Example**

'Kittens' followed by exactly three periods (.)

```
/Kittens\.{3,}/
```

`\.{3}`: Followed immediately by **three or more literal . characters**

```
Kittens...............  // match
Kittens!!!!
Kittens!
Kittens!!!!!!!!!!!!!
Kittens.........        // match
Kittens...              // match
Kittens.
Kittens
```

---
