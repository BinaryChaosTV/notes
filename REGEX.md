# Regular Expressions (REGEX)

## General Considerations

RegEx is case sensitive. See [Case sensitivity](#case-sensitivity) on how to ignore this pattern.
If using `/kevin/`, `Hello Kevin` => `false`
If using `/Kevin/` however, => `true`

RegEx is **greedy**, meaning that a regEx like `/t[a-z]_i/` in the word _titanic_ will look for a pattern that starts with t and ends with i. It would return `["titani"]`. See [Lazy matching](#lazy-matching) on how to make Regex "lazy".

## Methods

### Test

There are two different methods to test Regular Expressions: `.test()` & `.match()`
`.test()` evaluates the regex against a string.

```
let myString = "Hello, World!";
let myRegex = /Hello/;
let result = myRegex.test(myString);
```

`.test()` will return either `true` or `false`.

### Match

`.match()` does the opposite of `.test()`. It matches a string to RegEx.

```
let extractStr = "Extract the word 'coding' from this string.";
let codingRegex = /coding/;
let result = extractStr.match(codingRegex);
```

`.match()` will return the matched string component in an array format. In the example above, it would return `["coding"]`.

### Replace

Using RegEx, you can replace a matched pattern with another string. See [String variables](#string-variables) for more details.

`.replace()` takes a string, matches Regex to it (1st argument) and replaces it with the second argument.

```
let wrongText = "The sky is silver.";
let silverRegex = /silver/;
wrongText.replace(silverRegex, "blue");
```

## Flags

### Case sensitivity

The `i` flag will **ignore case sensitivity**. It is added after the RegEx.

```
let myString = "freeCodeCamp";
let fccRegex = /freecodecamp/i; // Ignores case sensitivity
let result = fccRegex.test(myString);
```

### Global match

the `g` flag matches the RegEx to **the whole string**, instead of the first word only. Can be combined with the `i` flag.

let twinkleStar = "Twinkle, twinkle, little star";
let starRegex = /twinkle/gi; // Will find two 'twinkle'
let result = twinkleStar.match(starRegex);

## Operators

### OR

The pipe `|` is the equivalent of the **OR operator**. We can match different words to it.

```
let petString = "James has a pet cat.";
let petRegex = /dog|cat|bird|fish/; // Could replace 'cat' with any of the animals found here, and it would return true.
let result = petRegex.test(petString);
```

## Symbols

### Wild Card (.)

The `.` is a **wild card**. For example: `/.un/` can match run, sun, fun, pun, nun, bun...

```
let exampleStr = "Let's have fun with regular expressions!";
let unRegex = /.un/;
let result = unRegex.test(exampleStr);
```

### The caret (^)

#### Start of string

A caret `^` tests for patterns at the **start of a string**.

```
let rickyAndCal = "Cal and Ricky both like racing.";
let calRegex = /^Cal/; // Checks if Cal is the first word in the string
let result = calRegex.test(rickyAndCal);
```

#### Character negation

You can add a caret '^' inside a bracket to **negate those characters** from a [literal pattern](#literal-patterns). For example, to only find consonents, you could check for `[^aeiou]`. This would still match punctuation and whitespaces though.

```
let quoteSample = "3 blind mice.";
let myRegex = /[^aeiou0-9]/gi; // Ignores vowels and numbers
let result = quoteSample.match(myRegex);
```

### The dollar sign ($)

#### End of string

To search at the **end of a string**, use the dollar sign `$`

```
let caboose = "The last car on a train is the caboose";
let lastRegex = /caboose$/; // Tests for the word caboose at the end of the string
let result = lastRegex.test(caboose);
```

#### String variables

You can access a [Captured group](#captured-groups) with the replacement string using a dollar sign `$`. Particularly useful when wanting to [Replace](#replace) values in the string.

`"Code Camp".replace(/(\w+)\s(\w+)/, '$2 $1');` => `Camp Code`

```
let str = "one two three";
let fixRegex = /(\w+)\s(\w+)\s(\w+)/; // Checks for three words with two whitespaces
let replaceText = "$3 $2 $1"; // Changes the order of 'str' to be 'three two one'.
let result = str.replace(fixRegex, replaceText);
```

### Repeat check (+)

To test if a character is **repeated one or more times** (in a row or otherwise), you can add a plus `+`. For example, `/a+/g` :

- `abc` => `["a"]` (One match)
- `aabc` => `["aa"]` (One match)
- `abab` => `["a", "a"]` (Two matches)

```
let difficultSpelling = "Mississippi";
let myRegex = /s+/g; // Should return twice as ["ss", "ss"]
let result = difficultSpelling.match(myRegex);
```

### Consecutive check (\*)

To test if a character appears **zero or more times** (in a row), you can use the asterisk `*`. For example, `/go*/` :

- `goooooooal` => `["gooooooo"]` (finds all the o's)
- `gut feeling` => `["g"]` (finds no o's)
- `over the moon` => `null` (finds nothing because there are no g's or o's)

```
let chewieRegex = /Aa\*/; // Checks for one capital A, followed by zero or more lowercase a
let result = chewieQuote.match(chewieRegex);
```

### Question mark (?)

#### Element exists

The question mark `?` tests whether the **previous element exists**. It's another way of checking if a character is optional. Useful to check variations in languages (like British vs. American English)

```
let favWord = "favorite";
let favRegex = /favou?rite/; // Checks for existence of the letter u
let result = favRegex.test(favWord);
```

#### Lazy matching

The question mark `?` turns a RegEx into lazy matching. As mentioned in [General considerations](#general-considerations), `/t[a-z]_?i/` in the word _"titanic"_ returns `["ti"]`.

```
let text = "<h1>Winter is coming</h1>";
let myRegex = /<.\*?>/; // Returns <h1>. If the ? was not there, it would return the whole string.
let result = text.match(myRegex);
```

## Sets

### Literal patterns

You can search for literal patterns with character classes. You can combine a list of characters using square brackets `[]`. For example: `/b[aiu]g/` will match bag, big, bug, but **not bog** because `o` isn't within the square brackets.

```
let quoteSample = "Beware of bugs in the above code; I have only proved it correct, not tried it.";
let vowelRegex = /[aeiou]/gi; // will find all vowels in the quote above.
let result = quoteSample.match(vowelRegex);
```

You can test a **range of characters** instead of writing them longhand by adding a hyphen `-`.

- `[a-e]` will test letters a through e.
- `[a-z]` will test letters lowercase a through z.
- `[0-9]` will test numbers 0-9.
- `[a-z0-9]` will test lowercase letters and numbers 0-9.
- `[A-Za-z0-9]` will test ALL alphanumeric characters.

```
let quoteSample = "The quick brown fox jumps over the lazy dog.";
let alphabetRegex = /[a-z]/gi; // Tests all letters in the string
let result = quoteSample.match(alphabetRegex);
```

```
let quoteSample = "Blueberry 3.141592653s are delicious.";
let myRegex = /[h-s2-6]/gi; // Test letters h through s & numbers 2 through 6
let result = quoteSample.match(myRegex);
```

### Quantity patterns

You can test for **quantity of patterns** with the curly brackets `{}`. For example, with the expression `/a{3,5}h/`, we test whether there are 3 to 5 a's in the string.

- `aaaah` => `true`
- `aah` => `false`

```
let ohStr = "Ohhh no";
let ohRegex = /Oh{3,6} no/g; // Checks between 3-6 h
let result = ohRegex.test(ohStr);
```

The numbers in the the quantity pattern are written as `{min#, max#}`. You can establish a `min#` w/o establishing a max. For example, you can check for a mininmum of 3 a's with `/a{3,}/`. Now it's checking if there are at least 3 a in a row before returning true.

```
let haStr = "Hazzzzah";
let haRegex = /Haz{4,}ah/; // Checking for a minimum of 4 z
let result = haRegex.test(haStr);
```

If you want a specific number of characters, omit the comma in the `{}`. For example, If we're looking for specifically 3 a's in _"haaah"_, then we write it as `/ha{3}h/`.

```
let timStr = "Timmmmber";
let timRegex = /Tim{4}ber/; // Check for exactly 4 m's
let result = timRegex.test(timStr);
```

### Grouped patterns

We can check for **groups of characters** with parentheses `()`. For example, to find _Penguin_ or _Pumpkin_ in a string, we could match it with `/P(engu|umpk)in/`.

```
let myString = "Eleanor Roosevelt";
let myRegex = /(Franklin|Eleanor) (\w+.? )?Roosevelt$/; // Checks for Eleanor or Franklin Roosevelt, and allows for middle names.
let result = myRegex.test(myString);
```

#### Captured Groups

When wrapped in parentheses `()`, it's possible to capture the group and refer to it as a temporary variable. To refer to this group later, simply use a backslash, followed by the number of the group. For example `\1`.

```
let repeatRegex = /(\w+) \1 \1/;
repeatRegex.test(repeatStr); // Returns true
repeatStr.match(repeatRegex); // Returns ["row row row", "row"]
```

```
let repeatNum = "42 42 42";
let reRegex = /^(\d+) \1 \1$/; // Checks for numbers exactly three times.
let result = reRegex.test(repeatNum);
```

### Lookaheads

Lookahead patterns checks whether the **pattern exists farther along in the string**. They exist either as _positive_ or _negative_.

- Positive lookahead `(?=)` will make sure the element in the search pattern is there, but won't return it.
- Negative lookahead `(?!)` will make sure the element in the search pattern is **NOT there**. Useful when you don't want something.

See [Password checker](#password-checker) for a concrete example.

## Shortcuts

### Numbers

`\d` === `[0-9]`

```
let movieName = "2001: A Space Odyssey";
let numRegex = /\d/g; // Checks for digits across the whole string.
let result = movieName.match(numRegex).length;
```

### Non-numbers

`\D` === `[^0-9]`

```
let movieName = "2001: A Space Odyssey";
let noNumRegex = /\D/g; // Checks for all non-numbers in a string
let result = movieName.match(noNumRegex).length;
```

### All alphanumeric characters

`\w` === `[A-Za-z0-9_]`

```
let quoteSample = "The five boxing wizards jump quickly.";
let alphabetRegexV2 = /\w/g; // Counts the number of alphanumeric characters in the string.
let result = quoteSample.match(alphabetRegexV2).length;
```

### Punctuation

`\W` === `[^A-Za-z0-9_]`

```
let quoteSample = "The five boxing wizards jump quickly.";
let nonAlphabetRegex = /\W/g; // Checks for symbols and punctuation across the whole string.
let result = quoteSample.match(nonAlphabetRegex).length;
```

### Whitespaces

To test for whitespace, use the shortcut `\s`. Returns the number of spaces in the string.

```
let sample = "Whitespace is important in separating words";
let countWhiteSpace = /\s/g; // Returns [" ", " ", " ", " ", " "]
let result = sample.match(countWhiteSpace);
```

### Non-whitespaces

`\S` === `[^\r\t\f\n\v]`

```
let sample = "Whitespace is important in separating words";
let countNonWhiteSpace = /\S/g; // Returns all non-whitepsaces
let result = sample.match(countNonWhiteSpace);
```

## Examples

### Username Checker

```
let username = "JackOfAllTrades";
const userCheck = /^[a-z]([0-9]{2,}|[a-z]+\d*)$/i;
let result = userCheck.test(username);
```

- `^` - Start of the string
- `[a-z]` - Check for letters at the start only
- `([0-9]{2,}` - Check for two or more numbers at the end
- `[a-z]+` - OR check for more letters
- `\d\*)` - followed by more digits
- `$` - end of the string
- `i` - ignore case sensitivity.

### Password Checker

See [Lookaheads](#lookaheads) to understand what `(?=)` does.

```
let password = "abc123";
let checkPass = /(?=\w{3,6})(?=\D\*\d)/; // Checks whether the string has at least 3-6 alphanumeric characters, followed by at least one number.
checkPass.test(password);
```

```
let sampleWord = "astronaut";
let pwRegex = /(?=\w{6,})(?=\D\*\d{2,})/; // Checks for a password 6 characters or more, with at least two consecutive digits.
let result = pwRegex.test(sampleWord);
```

### Remove whitespace

```
let hello = "   Hello, World!  ";
let wsRegex = /^\s+|\s+$/g; // Checks for whitespaces at start of string, OR at end of string.
let result = hello.replace(wsRegex, ""); // Replace those spaces (matched from RegEx) with "" (no spaces)
```
