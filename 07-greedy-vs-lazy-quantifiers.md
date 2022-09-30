# Greedy vs Lazy Quantifiers

Greedy = take everything you can and still match

Lazy = take as little as you can and still match

```
*?+{}
```

## What is lazy good for?

Eliminate character you don't want "inside" your string

Remember sentence regex?

```
/^[A-Z][^\.?!]+[\.?!]$/
```

Used `[\.?!]+` to control characters before final punctuation

Can replace what with lazy matching:

```
/^[A-Z].+?[\.?!]+$/
```

- Use `.*?` (lazy matching) to control characters before final punctuation
- as few characters as possible before `. ? !`
- this will stop whenever it hits `. ? !`

`^[A-Z]`: String starts with one capital letter (A through Z inclusive)

`.+`: One or more non-newline character

`?`: lazy(as few non-newline characters possible before the next token match)

`[\.?!]+`: String ends with one ore more `. ? !`

```
This is a sentence. // match
This is another sentence. And another! // match

```

## Greedy Quantifier

Simple greedy example (greedy by default)

```
/gre*/
```

gr: The string `gr`

`e*`: Zero or more e's (as many as possible)

```
greeeeeeedy // match greeeee
grep // match gre
grain // match gr
```

---

## Greedy vs. Lazy Syntax

Some platforms (PHP, Go) use flag

Some platforms use `?` after quantifier

- Python, JavaScript, grep

`?` is

- both quantifier and lazy
- it will do even more e.g. groups!

Examples:

- `/gre*?/`

```
greeeeeeeeeeeedy // match only gr
grep // match only gr
grain // match only gr
```

- `/gre??/`
  as little as possible, 0 or 1 (so 0)

```
greeeeeeeedy // match gr
grep // match gr
grain // match gr
```

---

## Useless Lazy Quantifier Examples

Simple lazy example

```
/la*?/
```

l: The character l

a\*: Followed by 0 or more a's

`?`: as few (of the zero or more a's) as possible (lazy)

```
/la*?/
```

```
lazy // match only l
lisa // match only l
laaaaaaaaaaaaa // match only l
```

**Example 2**

Lazy example with `+` quantifier

```
/la+?/
```

l: The character l

a+: Followed by one or more a's

`?`: as few (of the one or more a's) as possible (lazy)

```
lazy // match la
lisa // not
laaaaaaaa // match la
```

but can do the same with `/la/`

---

## Almost Equivalent

lazy matching vs prohibiting characters

- `/^[A-Z].+?[\.?!]+$/` vs `/^[A-Z][^\.?!]+[\.?!]+$/`

If lazy matching isn't an option (can be performance hit)

- can use negated collection instead

One difference (fine print):

- lazy .+? can match a . ? !
- if that's the only way to make the regex work

`/^[A-Z].+?[\.?!]+$/`

```
This is a sentence // match
This is another sentence. And another! // match
```

`/^[A-Z][^\.?!]+[\.?!]+$/`

```
This is a sentence // match
This is another sentence. And another! // not
```

---

## Examples

```
// Exercise 25: Continuing from the exercises from last section:
// using the same Robert Frost text, find the shortest first
// phrase that starts and ends with “and” (no quotes, case doesn’t matter)
// The text:
//
// The woods are lovely, dark, and deep,
// But I have promises to keep,
// And miles to go before I sleep,
// And miles to go before I sleep.
const shortestFirstAndBookendRegex = /and.+?and/is;
```

```
// Exercise 26: Find as many non-overlapping strings as you
// can that start with ‘s’ (no quotes, case sensitive) and ends
// with ‘e’ (no quotes). Matched strings should be all on the
// same line.
const nonOverlappingSeStringsRegex = /s.*?e/g;
```

```
// Exercise 27: In an HTML string, find all the elements
// (including surrounding tags). That is, find strings that start
// with a string in angle brackets (for example, <i>) and end
// with a string in angle brackets that starts with a slash (for
// example, </i>). HTML strings may be multiline.
//
// NOTE: You don't yet have the tools to deal with nested
// elements (like <p>Don't you just <b>love</b> regexes?</p>)
// We will discuss this case in the next lecture on groups!
const htmlElementRegex = /<.+?>.*?<\/.+?>/gm;
```

```
// Exercise 28: For an added challenge, try the last exercise,
// but also capture elements that only have *only one tag* that
// ends with /> (because the element has no contents to put
// between tags, for example,
// <img src=”http://placekitten.com/200/300” /> )
const htmlElementIncludingSingleTagsRegex = /<.+?>?.*?<?\/.*?>/gm;
```

---
