# Lookarounds

Lookaheads and Lookbehinds

## Lookarounds

- Specify conditions for regular expressions without capturing

  - What you **do** want to see immediately before or after part of regex, positive lookahead or positive lookbehind
  - What you **don't** want to see immediately before or after part of regex, negative lookahead or negative lookbehind

- Different from non-capturing group in that it is **not** part of match

  - in that way, lookarounds are like ^ $ \b \B tokens

- This is another way to specify a kind of group
  - Guess what's going to be involved in the syntax...?

| Description         | syntax  |
| ------------------- | ------- |
| Positive Lookahead  | `(?=)`  |
| Negative Lookahead  | `(?!)`  |
| Positive Lookbehind | `(?<=)` |
| Negative Lookbehind | `(?<!)` |

---

## Positive Lookbehind

We want USD to come before the match that we are interested in

```
/(?<=USD )\d+(?:\.\d\d)?$
```

```
USD 34.75         // match 34.75
GBP 1.20
BRL 15.91
JPY 68.93
USD 22.03...8789
USD 50  //match
BRL 120.33
INR 879.21
```

Find amounts preceded by USD:

```
/(?<=USD )\d+(?:\.\d\d)?$/mg
```

`(?<= )`: Lookbehind: match this before, but don't include in match

USD: The string USD and a space

`\d+`: One or more digits

`(?: )?`: Non-capturing, optional group for cents

`(\.\d\d)`: Decimal point and cents

`$`: Anchor the regular expression at the end of the line

---

## Negative Lookbehind

```
It was a quarter pas six when we left Baker Street, and it still wanted ten minutes to the hour when we found ourselves in Serpentine Avenue. It was already dusk, and the lamps were just being lighted as we paced up and down in front of Briony lodge, waiting for the coming of its occupant. The house was just such as I had pictured it from Sherlock Holmes' succinct description, but the locality appeared to be less private that I expected. On the contrary, for a small street in a quiet neighborhood, it was remarkably animated.

Except From: Arthur Conan Doyle. "The Adventures of Sherlock Homes". Apple Books.
```

Capture all capitalized words:

```
/\b[A-Z]\w*\b/
```

Capture all capitalized words except those that start a sentence:

```
/(?<![\.?!\s+])[A-Z]\w*\b
```

Capture all capitalized words except those that start a sentence, and remove the beginning **It**:

```
/(?<!^)(?<![\.?!"?\s+"?][A-Z]\w*\b
```

Find all capitalized words that **don't** start a sentence

```
/(?<!^)(?<![\.\?!]"?\s+"?)[A-Z]\w*\b/gm
```

`(?<! )`: Negative lookbehind

`^` m flag: Do not match the beginning of the line immediately before pattern

`(?<! )`: Negative lookbehind

`[\.?!]"?\s"?`: . ? ! and then a whitespace character with optional "s before and after

`[A-Z]`: Capital letter from A through Z inclusive

`\w*`: Zero or more word characters

`\b`: Word boundary

g flag: Return all non-overlapping matches

---
