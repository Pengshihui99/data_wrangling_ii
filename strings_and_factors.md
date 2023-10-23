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

``` r
fac_sex = factor(c("male", "male", "female", "female"))

fac_sex
```

    ## [1] male   male   female female
    ## Levels: female male

``` r
as.numeric(fac_sex)
```

    ## [1] 2 2 1 1

- male and female have implemented some rules to determine what level
  order are. so if convert this to numeric type, 2 and 1 will be
  distributed

- what happen if i relevel…

``` r
fac_sex_level = fct_relevel(fac_sex, 'male')

as.numeric(fac_sex_level)
```

    ## [1] 1 1 2 2

- here, i relevel ‘male’ as the 1st cat, so when converting to numeric
  type, it shows 1122 instead of 2211 (when female is the 1st cat)
- **`fct_relevel`**: reorder factor levels by hand
- **`fct_recode`**: change factor labels by hand
- **`fct_reorder`**: reorder factors by sorting along another variable
  - lots of factor functions and string functions we can google or
    search in r!

# NSDUH - string

### import nsduh dara

``` r
nsduh_url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

table_marj = 
  read_html(nsduh_url) |> 
  html_table() |> 
  first() |>
  slice(-1)
```

### need to tidy this ds

``` r
data_marj =
  table_marj |> 
  select(-contains('P Value')) |> 
  pivot_longer(
    -State,
    names_to = 'age_year',
    values_to = 'percent'
  ) |> 
  separate(age_year, into = c('age', 'year'), sep = '\\(') |> 
  mutate(
    year = str_replace(year, '\\)', ''),
    percent = str_replace(percent, '[a-c]$', ''),
    percent = as.numeric(percent)
  ) |> 
  filter(!(State %in% c("Total U.S.", "Northeast", "Midwest", "South", "West")))
```

- **`select(-contains('P Value'))`**: remove any column that contains ‘P
  Value’

- **`-State`**: i want to pivot everything that is not in the State col

- **`separate(age_year, into = c('age', 'year'), sep = '\\(')`**

  - separate the col `age_year` into `age` and `year`
  - where to separate? – `sep =`
    - separate from the separate character, which is `(` here, and split
      it apart at this character, and get rid of this separation stuff.
      but the `(` is a special character, so we need to use `\\` b4 it.
      – `'\\('`

- inside the `mutate()`:

  - **\``str_replace(year, '\\)', '')`**: in year col, replace the `)` w
    nothing (nothing inside the ’’)

- **`filter(!(State %in% c("Total U.S.", "Northeast", "Midwest", "South", "West")))`**:
  filters OUT rows where the “State” col is one of the specified values
  (“Total U.S.”, “Northeast”, “Midwest”, “South”, “West”)

- so for `stringr` part and regular expressions in this code:

  - in separate, we split age and year at the open parentheses using
    `"\\("`
  - we stripped out the close parenthesis in `mutate`
  - to remove character superscripts, we replaced any character using
    `"[a-c]$"`

# NSDUH – factors

so now i want to make a plot. Let’s quickly visualize these data for the
12-17 age group; to make the plot readable, we’ll treat State as a
factor are reorder according to the median percent value.

``` r
data_marj |> 
  filter(age == '12-17') |> 
  mutate(
    State = fct_reorder(State, percent)
    ) |> 
  ggplot(aes(x = State, y = percent, color = year)) +
  geom_point() +
  theme(axis.text.x = element_text(angle = 90, hjust = 1))
```

<img src="strings_and_factors_files/figure-gfm/unnamed-chunk-13-1.png" width="90%" />

- `mutate(State = fct_reorder(State, percent))`: reorder the State col
  in the order of percent col. Since we have 2 percents for 1 state, r
  will use the median value of these 2 percents and put state in this
  order.
  - we can also use sth like `fct_relevel(State, 'Texas', 'New York')`
    to reorder specific state order.
  - we can use functions from the forcats package (eg.fct\_ ), inside
    tidyverse, to work with character columns as if they were factor
    variables, without explicitly converting the column to a factor.
    These functions are designed to provide factor-like behavior for
    character vectors.
- `theme(axis.text.x = element_text(angle = 90, hjust = 1))`: make the
  lable in x-axis rotate 90 degree
