# Capture Groups

## Using Groups to Extract Text

Extract filename minus extension (root file name)

- only for files with the extension png, jpg, jpeg or pdf

So if the file was cute_kittens.jpeg

- we'd return cute_kittens

if the file was cute_kittens.mpeg

- we would return null

Extract root filename for extensions png, jpg, jpeg or pdf

```
/(.+)\.(png|jpe?g|pdf)/
```

`( )`: Group to capture filename before extension

`.+`: One or more non-newline characters in filename before extension

`\.`: literal `.` character

`( )`: Group for multiple multi-character options

`png|jpe?g|pdf`: png, jpg, jpeg, pdf

```
cute_kittens.pdf // match
cute_kittens.mpg // not
adorable_puppies.frolicking.jpeg // match
```

---

## Non-Capturing Groups

Regex from last lecture had two groups:

```
/(.+)\.(png|jpg?g|pdf)/
```

We didn't care about group 2(the extension)

Only needed a group for multi=character option

What if we didn't capture it?

Non-capturing group syntax: `(?:)`

- used a lot as first character of groups
- specifies what kind of group

**Example**

```
/(.+)\.(?:png|jpe?g|pdf)/
```

`( )`: Group to capture filename before extension

`.+`: One or more non-newline characters in filename before extension

`\.`: literal `.` character

`(?: )`: Non-capturing group for multiple multi-character options

`png|jpe?g|pdf`: png, jpg, jpeg, pdf

```
cute_kittens.pdf // match but only 1 capture group
cute_kittens.mpg // not
adorable_puppies.frockling.jpeg // match but only 1 capture group
```

---

## Group Numbers

What are those group numbers good for, anyway?

Within regular expression

- refer to previously captured group later in the regular expression

When replacing(next section)

- transform a particular group number for replacement

Syntax varies, but in JavaScript we refer to the group using a backslash

- For example: \1

**Example**

Find the complete HTML elements, including tags

```
/<(\w+)>.*?<\/\1>/g
```

`<` : Open angle bracket

`( )`: Group to capture first tag contents

`\w+`: One ore more word characters

`>`: closing angle bracket

`.*`: zero or more non-newline characters

`?`: as few as possible non-newline characters while still matching regex (lazy)

`<\/`: Open angle bracket and literal slash (/) character

`\1`: Whatever was captured in the first capturing group

`>`: Closing angle bracket

```
<span>this is a <b>span</b></span> // match
```

It's only going to recognize the second part if it contains the same thing within the closing bracket as the opening bracket had (with the addition of forward slash)

---

## Named Groups

Back to our example of extracting root file name for certain extensions

```
/(.+)\.(png|jpe?g|pdf)/
```

What if you wanted to name the groups

- better code readability than referring to them by number

Another use for the hard-working `?`

Syntax varies by platform!

- Using JavaScript syntax here as usual

**Example**

Extract root file name and extension, with named groups

```
/(?<rootname>.+)\.(?<extension>png|jpe?g|pdf)/
```

`(?<rootname>) `: Group to capture filename before extension, named rootname

`.+`: One or more non-newline characters in filename before extension

`\.`: literal `.` character

`(?<extension) `: Group to capture extension, named extension

`png|jp?g|pdf`: options for extensions: png, jpg, jpeg, pdf

```
cute_kittens.pdf
```

Useful in you are referring to the groups in your code later on.

---

## Exercises

```
// Exercise 33: Match strings that start and end with the same word.
// We do want to capture the word.
// Assume all strings do not contain newlines.
const wordBookendRegex = /^(\w+)\b.*\b\1$/;
```

```
// Exercise 34: Given a list of files in a directory (separated
// by newlines), identify which files have a vi swap file
// vi swap files look like this: .filename.swp
// So if there were a file in the directory called .favoriteRegexes.txt.swp
// You would want to include favoriteRegexes.txt in your results
const viSwapRegex = /^\.(.*)\.swp$/mg;
```

```
// Exercise 35: Given data shaped like this:
//   03Sep2020 04:55:38 This is a message
//   03Oct2020 23:44:01 This is another message
// Extract:
//   the day as a group named day
//   the month as “month”
//   the year as “year”
//   the time as a group named “time”
//   the message as a group named “message”
//
// NOTE: you do not need to validate the date and time (for example, you don’t
// need to confirm that the date has a valid month or that the time is valid
// for a 24 hr clock) -- you can assume the data is clean. Simply extract the
// parts.
//
// Example result from the first line of sample data:
//   day=03
//   month=Sep
//   year=2020
//   time=04:55:38
//   message=This is a message
// SYNTAX: In JavaScript, named groups are designated by (?<>)

// IMPORTANT NOTE: Udemy's code exercise engine is not capable of handling this
// regular expression. If you would like to do this exercise, please check your answers
// in the course repository.
// https://github.com/bonnie/udemy-regex-syntax-examples/tree/master/exercises/javascript/8_capture_groups
const extractLogPartsRegex = /(?<day>\d{2})(?<month>\w{3})(?<year>\d{4}) (?<time>\d{2}:\d{2}:\d{2}) (?<message>.*)/;
```

---
