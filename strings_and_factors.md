strings and factors
================
Shihui Peng
2023-10-22

# strings

## detect

### str_detect()

``` r
string_vec = c('my', 'name', 'is', 'shihui')

str_detect(string_vec, 'shihui')
```

    ## [1] FALSE FALSE FALSE  TRUE

``` r
str_detect(string_vec, 'm')
```

    ## [1]  TRUE  TRUE FALSE FALSE

``` r
str_detect(string_vec, 'S')
```

    ## [1] FALSE FALSE FALSE FALSE

- to identify the elements of this string that corresponds to my name –
  detect a particular pattern with .
  - **`str_detect()`**: given a string or a vector of character obs,
    detect the presence or absence of a pattern in a string.
    - don’t need the whole string to match w the pattern (the ‘e’
      example).
    - uppercase and lowercase are different values when working w
      strings. (the ‘S’ example).

### start/end with

``` r
string_vec_2 = c(
  "i think we all rule for participating",
  "i think i have been caught",
  "i think this will be quite fun actually",
  "it will be fun, i think"
  )

str_detect(string_vec_2, '^i think')
```

    ## [1]  TRUE  TRUE  TRUE FALSE

``` r
str_detect(string_vec_2, 'i think$')
```

    ## [1] FALSE FALSE FALSE  TRUE

- We can designate matches at the beginning or end of a line.
  - inside the `str_detect()`:
    - **`'^i think'`**: testing strings that start w ‘i think’ but not
      end w ‘i think’.
    - **`'i think$'`**: testing strings that end w ‘i think’ rather than
      begin w ‘i think’.

### design a list of char

``` r
string_vec_3 = c(
  "Time for a Pumpkin Spice Latte!",
  "went to the #pumpkinpatch last weekend",
  "Pumpkin Pie is obviously the best pie",
  "SMASHING PUMPKINS -- LIVE IN CONCERT!!"
  )

str_detect(string_vec_3, '[Pp]umpkin')
```

    ## [1]  TRUE  TRUE  TRUE FALSE

- We can designate a list of characters that will count as a match.
  - **`'[Pp]umpkin'`**: i want both p and P for ‘pumpkin’ to be
    detected, so i put P and p into `[]`. the last string is false b/c
    the remaining characters don’t match.

### provide a range of letters/numbers

``` r
string_vec_4 = c(
  '7th inning stretch',
  '1st half soon to begin. Texas won the toss.',
  'she is 5 feet 4 inches tall',
  '3AM - cant sleep :('
  )

str_detect(string_vec_4, '[0-9][a-zA-Z]')
```

    ## [1]  TRUE  TRUE FALSE  TRUE

- We don’t have to list these; instead, you can provide a
  `range of letters or numbers` that count as a match.
  - **`'[0-9][a-zA-Z]'`**: i want to get a number from 0 to 9 first,
    followed immediately by a letter from a to z and A to Z
  - if `'^[0-9][a-zA-Z]'`: i want all those above to be at the start of
    the string.

### `.` matches everything

``` r
string_vec_5 = c(
  'Its 7:11 in the evening',
  'want to go to 7-11?',
  'my flight is AA711',
  'NetBios: scanning ip 203.167.114.66'
  )

str_detect(string_vec_5, '7.11')
```

    ## [1]  TRUE  TRUE FALSE  TRUE

``` r
str_detect(string_vec_5, '7\\.11')
```

    ## [1] FALSE FALSE FALSE  TRUE

- The character `.` matches anything.
  - **`'7.11'`**: the `.` here means i want 7 with another character
    (can be anything) with 11.
    - the 3rd string does not match b/c there is nothing between 7 and
      11 there.
  - **`'7\\.11'`**: what if we just want exactly `'7.11'`? – need to put
    2 slashes before the `.`.

### `\\` for special characters

``` r
string_vec_6 = c(
  'The CI is [2, 5]',
  ':-]',
  ':-[',
  'I found the answer on pages [6-7]'
  )

str_detect(string_vec_6, "\\[")
```

    ## [1]  TRUE FALSE  TRUE  TRUE

- Some characters are “special”. These include `[` and `]`, `(` and `)`,
  and `.`. If you want to search for these, you have to indicate they’re
  special using **`\`**. Unfortunately, `\` is also special, so things
  get weird.
  - so we need to use `\\`.

## Replace

is there a way to modify that string vector to replace a string or
character w another?

``` r
str_replace(string_vec, 'shihui', 'Shihui Peng')
```

    ## [1] "my"          "name"        "is"          "Shihui Peng"

``` r
str_replace(string_vec, 'm', 'Mac')
```

    ## [1] "Macy"   "naMace" "is"     "shihui"

- **`str_replace()`**: replace the string/character w another

# factors