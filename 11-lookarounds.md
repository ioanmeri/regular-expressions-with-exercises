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

## Note about Fixed-Width Lookarounds

- Some platforms only support fixed-width lookarounds
  - Python is one of these
- Variable number of characters in a lookaround can be a performance risk
- Any indeterminate quantifiers (\*,+,or even ?) aren't allowed
- Workaround:
  - add the lookaround as a normal part of the regex (without the syntax)
  - capture non-lookaround part in a group / groups
  - access the desired group(s) as the match
- Way to mimic (negative) lookbehind in some situations
  - http://blog.stevenlevithan.com/archives/mimic-lookbehind-javascript

### While we're on the topic...

- Some browsers don't support lookbehind **at all** for performance reasons
  - positive or negative
  - fixed or variable width
  - Internet Explorer, Safari, Firefox on Android

---

## Positive Lookahead

Extract the dates that which "something happened" event occurred

```
/\w{3} \d\d(?= 00:00:00 something happened)/
```

```
sep 24 00:00:00 something happened // match sep 24
sep 24 13:29:33 something else happened
sep 24 28:42:07 yet another thing happened
sep 25 00:00:00 something happened  // match sep 25
sep 25 09:08:56 you gotta see this
sep 26 06:37:40 alert! alert!
sep 26 10:22:49 this needs your attention
sep 27 00:00:00 something happened // match sep 27
sep 27 18:29:12 maybe you should look into this
```

Match exactly the something happened messages

```
/\w{3} \d\d(?= 00:00:00 something happened$)/
```

The lookahead needs to have that string at the end of the line

```
sep 24 00:00:00 something happened // match sep 24
sep 24 13:29:33 something else happened
sep 24 28:42:07 yet another thing happened
sep 25 00:00:00 something happened  // match sep 25
sep 25 09:08:56 you gotta see this
sep 26 06:37:40 alert! alert!
sep 26 10:22:49 this needs your attention
sep 27 00:00:00 something happened // match sep 27
sep 27 00:00:00 something happened that you HAVE to see!
sep 27 18:29:12 maybe you should look into this
```

### Example

Find dates when specific error occurred at midnight

```
/^\w\w\w \d\d (?=00:00:00 something happened$)/gm
```

`^\w\w\w`: flag: m | Three word characters at the beginning of the line

`\d\d`: a space, two digit characters, and another space

`(?= )`: Positive lookahead

`00:00:00 something happened$`: flag: m | Date string and message at the end of the line

g flag: Return all non-overlapping matches

### Why does the $ need to be inside the lookahead?

- Remember, the lookahead doesn't consume characters

  - similar to `\b` or `^`

- So if we put the `$` **outside** the lookahead, it means
  - The lookahead string must come immediately after the match
  - The end of the string must immediately come after the match
    - as far as the match is concerned, those characters don't exist
  - Impossible for both to be true!

---

## Negative Lookahead

Find all words that are not at the end of the line

```
/\w+\b(?!\W?$)/gm
```

```
There was a young fellow of Crete
Who was so exceedingly neat.
When he got out of bed,
He stood on his head
To make sure of not soiling his feet.
```

`\w+` One or more word characters

`\b` Word boundary

`(?! )` Negative lookahead: do not match this immediately after

`\W?` optional non-word character

`$` flag m: end of line

---

## Exercises

```

// Exercise 42: Given a file with command-line capture, find
// all the commands. The prompt looks like this:
//
// directoryname git:(branchname) $
//
// Where directoryname and branchname are the working directory
// and git branch, respectively.
//
// The regular expression should capture only commands,
// not output (which would be on lines that did not start with
// a prompt), and not the prompts themselves.
//
// Example string:
// udemy-regex git:(master) $ cd exercises
// exercises git:(master) $ ls
// js     python
// exercises git:(master) $ cd python
// python git:(master) $ ls
// 1_characters_and_quantities 5_flags                     9_lookaround
// 2_collections_and_ranges    6_greedy_vs_lazy            run_tests.sh
// 3_whitespace_and_boundaries 7_groups
// 4_character_categories      8_substitution
// python git:(master) $ cd 9_lookaround
// 9_lookaround git:(master) $ ls
// __pycache__ evaluate.py
// 9_lookaround git:(master) $ touch lookaround.py
export const commandsRegex = /(?<=.+ git:\(.+\) \$ ).+/gm;
```

```
// Exercise 43: There's a document formatted so that each
// name is on its own line preceded by "Name: " (no quotes).
// Capture all the names, without the preceding "Name: " string.
// Example document:
//
// Name: Nanny McPhee
// Email: nanny.mcphee@notarealdomain.com
//
// Name: Muhammad Ali
// Email: muhammad.ali@notarealemaildomain.com
const nameRegex = /(?<=Name: ).*\b/;
```

```
// Exercise 44: You have a string containing tags,
// separated by commas. Each tag contains only letters,
// numbers and underscores.
//
// Capture the tags that start with `meta__` (no quotes).
// Capture only the part of the tags AFTER the `meta__`
//
// Example tag string:
// a_tag, meta__another_tag, meta__third_tag, fourth_tag
const metaTagRegex = /(?<=meta__)\w+/;
```

```
// Exercise 45: Find each word directly before a semicolon
// in a block of text (but do not capture the semicolon).
// Text may be multi-line.
const wordBeforeSemiRegex = /\b\w+(?=;)/;
```

```
// Exercise 46: Given a list of file names, capture files
// that contain `py` (without quotes) but NOT at the end of
// the string (that is, not python files)
// HINT: use negative lookbehind from the end of
// the string
//
// Example file list:
// happy.js
// happy.py
// sad.sh
// sad.py
// pyrite.go
//
// Output: happy.js, pyrite.go
const nonPyfileRegex = /py(?!$)/;

```

Find all files without the extensions: .lock | .git

```
\w+\.(?!png|git)
```
