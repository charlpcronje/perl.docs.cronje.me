---
title: Regex for SSH Clients | CRONje.ME
label: Regex for SSH Clients
order: 35
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---

# 5.7 Capturing and Clustering

``Patterns allow you to group portions of your pattern together into sub-patterns and to remember the strings matched by those sub-patterns. We call the first behavior _clustering_ and the second one _capturing_.

## 5.1 Capturing

### 5.7.1 Capturing and Clustering

Patterns allow you to group portions of your pattern together into sub-patterns and to remember the strings matched by those sub-patterns. We call the first behavior _clustering_ and the second one _capturing_.

### 5.7 Capturing

To capture a substring for later use, put parentheses around the sub-pattern that matches it. The first pair of parentheses stores its substring in, the second pair in, and so on. You may use as many parentheses as you like; Perl just keeps defining more numbered variables for you to represent these captured strings.

### Some examples

```python
/(\d)(\d)/  # Match two digits, capturing them into $1 and $2
> /(\d+)/     # Match one or more digits, capturing them all into $1
> /(\d)+/     # Match a digit one or more times, capturing the last into $1
```

Note the difference between the second and third patterns. The second form is usually what you want. The third form does _not_ create multiple variables for multiple digits. Parentheses are numbered when the pattern is compiled, not when it is matched.

Captured strings are often called _backreferences_ because they refer back to parts of the captured text. There are actually two ways to get at these back references. The numbered variables you've seen are how you get at backreferences outside of a pattern, but inside the pattern, that doesn't work. You have to use 1, 2, etc. So to find doubled words like "the the" or "had had", you might use this pattern:

```python
/\b(\w+) \1\b/i
```

But most often, you'll be using the $1 form, because you'll usually apply a pattern and then do something with the substrings. Suppose you have some text (a mail header) that looks like this:

From: gnat@perl.com
> To: camelot@oreilly.com
> Date: Mon, 17 Jul 2000 09:00:00 -1000
> Subject: Eye of the needle

and you want to construct a hash that maps the text before each colon to the text afterward. If you were looping through this text line by line (say, because you were reading it from a file) you could do that as follows:

```python
while (<>) {
>     /^(.*?): (.*)$/;    # Pre-colon text into $1, post-colon into $2
>     $fields{$1} = $2;
> }
```

These numbered variables are dynamically scoped through the end of the enclosing block or eval string, or to the next successful pattern match, whichever comes first. You can use them in the righthand side (the replacement part) of a substitute, too:

```python
s/^(\S+) (\S+)/$2 $1/;  # Swap first two words
```

Groupings can nest, and when they do, the groupings are counted by the location of the left parenthesis. So given the string "Primula Brandybuck", the pattern:


You can't use `$1` for a backreference within the pattern because that would already have been interpolated as an ordinary variable back when the regex was compiled. So we use the traditional 1 backreference notation inside patterns. For two- and three-digit backreference numbers, there is some ambiguity with octal character notation, but that is neatly solved by considering how many captured patterns are available. For instance, if Perl sees a 11 metasymbol, it's equivalent to `$11` only if there are at least 11 substrings captured earlier in the pattern. Otherwise, it's equivalent to \011, that is, a tab character.

```python
/^((\w+) (\w+))$/
```

would capture "Primula Brandybuck" into `$1`, "Primula" into `$2`, and "Brandybuck" into `$3`. This is depicted in 
#### Figure 5.1. Creating backreferences with parentheses

Patterns with captures are often used in list context to populate a list of values, since the pattern is smart enough to return the captured substrings as a list:

```python
($first, $last)          =  /^(\w+) (\w+)$/;
> ($full, $first, $last) =  /^((\w+) (\w+))$/;
```

With the  modifier, a pattern can return multiple substrings from multiple matches, all in one list. Suppose you had the mail header we saw earlier all in one string (in , say). You could do the same thing as our line-by-line loop, but with one statement:

```python
%fields = /^(.*?): (.*)$/gm;
```

The pattern matches four times, and each time it matches, it finds two substrings. The `/gm` match returns all of these as a flat list of eight strings, which the list assignment to `%fields` will conveniently interpret as four key/value pairs, thus restoring harmony to the universe.

>Several other special variables deal with text captured in pattern matches. >< contains the entire matched string,everything to the left of the match, to the right. `$+` contains the contents of the last backreference.

```python
$_ = "Speak, <EM>friend</EM>, and enter.";
> m[ (<.*?>) (.*?) (</.*?>) ]x;     # A tag, then chars, then an end tag
> print "prematch:  ";          # Speak,
> print "match:     ";          # <EM>friend</EM>
> print "postmatch: ";          # , and enter.
> print "lastmatch: ";          # </EM>
```

For more explanation of these magical Elvish variables (and for a way to write them in English), see [Chapter 28, "Special Names"](ch28_01.htm).

The `@-` `@LAST_MATCH_START` `array` holds the offsets of the beginnings of any `submatches`, and `@+` (`LAST_MATCH_END`) holds the offsets of the ends:

```python
#!/usr/bin/perl
> $alphabet = "abcdefghijklmnopqrstuvwxyz";
> $alphabet =~ /(hi).*(stu)/;
> 
> print "The entire match began at $-[0] and ended at $+[0]\n";
> print "The first  match began at $-[1] and ended at $+[1]\n";
> print "The second match began at $-[2] and ended at $+[2]\n";
```

If you really want to match a literal parenthesis character instead of having it interpreted as a metacharacter, backslash it:

This matches a parenthesized example (e.g., this statement). But since dot is a wildcard, this also matches any parenthetical statement with the first letter e g (ergo, this statement too).

### 5.7.2.3 Clustering

Bare parentheses both cluster and capture. But sometimes you don't want that. Sometimes you just want to group portions of the pattern without creating a backreference. You can use an extended form of parentheses to suppress capturing: the (?:_PATTERN_) notation will _cluster_ without capturing.

There are at least three reasons you might want to cluster without capturing:

1. To quantify something.
2. To limit the scope of interior alternation; for example,/^cat|cow|dog $/ to be (?:cat|cow|dog)$/ so that the cat doesn't run away with the

3. To limit the scope of an embedded pattern modifier to a particular subpattern, such as in /foo(?-i:Case_Matters)bar/i. (See the next section,
4. [Section 5.7.3, "Cloistered Pattern Modifiers"](ch05_07.htm#ch05-sect-cloist))
In addition, it's more efficient to suppress the capture of something you're not going to use. On the minus side, the notation is a little noisier, visually speaking.

In a pattern, a left parenthesis immediately followed by a question mark denotes a regex _extension_. The current regular expression bestiary is relatively fixed--we don't dare create a new metacharacter, for fear of breaking old Perl programs. Instead, the extension syntax is used to add new features to the bestiary.

In the remainder of the chapter, we'll see many more regex extensions, all of which cluster without capturing, as well as doing something else. The (?:_PATTERN_) extension is just special in that it does nothing else. So if you say:

```python
@fields = split(/\b(?:a|b|c)\b/)


it's like:

@fields = split(/\b(a|b|c)\b/)
```

. . but doesn't spit out extra fields. (The split operator is a bit like m//g in that it will emit extra fields for all the captured substrings within the pattern. Ordinarily, split only returns what it `_didn't_` match. For more on split see [Chapter 29, "Functions"](ch29_01.htm).).

### 5.7.3 Cloistered Pattern Modifiers

You may `_cloister_` the `/i`, `/m`, `/s`, and `/x` modifiers within a portion of your pattern by inserting them (without the slash) between the `?` and `: of the clustering notation. If you say:

```python
/Harry (?i:s) Truman/

it matches both "Harry S Truman" and "Harry s Truman", whereas:

/Harry (?x: [A-Z] \.? \s )?Truman/

matches both "Harry S Truman" and "Harry S. Truman", as well as "Harry Truman", and:

/Harry (?ix: [A-Z] \.? \s )?Truman/

matches all five, by combining the /i and /x modifiers within the cloister.

You can also subtract modifiers from a cloister with a minus sign:

/Harry (?x-i: [A-Z] \.? \s )?Truman/i
```

This matches any capitalization of the name--but if the middle initial is provided, it must be capitalized, since the /i applied to the overall pattern is suspended inside the cloister.

By omitting the colon and _PATTERN_, you can export modifier settings to an outer cluster, turning it into a cloister. That is, you can selectively turn modifiers on and off for the cluster one level outside the modifiers' parentheses, like so:

```python
/(?i)foo/            # Equivalent to /foo/i
> /foo((?-i)bar)/i     # "bar" must be lower case
> /foo((?x-i) bar)/    # Enables /x and disables /i for "bar"
```

Note that the second and third examples create backreferences. If that wasn't what you wanted, then you should have been using (?-i:bar) and (?x-i: bar), respectively.

Setting modifiers on a portion of your pattern is particularly useful when you want "." to match newlines in part of your pattern but not in the rest of it. Setting /s on the whole pattern doesn't help you there.

To capture a substring for later use, put parentheses around the subpattern that matches it. The first pair of parentheses stores its substring in $, the second pair in $, and so on. You may use as many parentheses as you like; Perl just keeps defining more numbered variables for you to represent these captured strings.

Some examples:

```python
> /(\d)(\d)/  # Match two digits, capturing them into $1 and $2
> /(\d+)/     # Match one or more digits, capturing them all into $1
> /(\d)+/     # Match a digit one or more times, capturing the last into $1
```

Note the difference between the second and third patterns. The second form is usually what you want. The third form does _not_ create multiple variables for multiple digits. Parentheses are numbered when the pattern is compiled, not when it is matched.

Captured strings are often called _backreferences_ because they refer back to parts of the captured text. There are actually two ways to get at these backreferences. The numbered variables you've seen are how you get at backreferences outside of a pattern, but inside the pattern, that doesn't work. You have to use, etc.[[9]](#FOOTNOTE-9) So to find doubled words like "the the" or "had had", you might use this pattern:

But most often, you'll be using the $ form, because you'll usually apply a pattern and then do something with the substrings. Suppose you have some text (a mail header) that looks like this:


and you want to construct a hash that maps the text before each colon to the text afterward. If you were looping through this text line by line (say, because you were reading it from a file) you could do that as follows:

```python
while (<>) {
    /^(.*?): (.*)$/;    # Pre-colon text into $1, post-colon into $2
    $fields{$1} = $2;
}
```

Like , and , these numbered variables are dynamically scoped through the end of the enclosing block or `eval` string, or to the next successful pattern match, whichever comes first. You can use them in the righthand side (the replacement part) of a substitute, too:

> `s/^(\S+) (\S+)/$2 $1/;`  # Swap first two words

Groupings can nest, and when they do, the groupings are counted by the location of the left parenthesis. So given the string "Primula Brandybuck", the pattern:

You can't use $ for a backreference within the pattern because that would already have been interpolated as an ordinary variable back when the regex was compiled. So we use the traditional \ backreference notation inside patterns. For two- and three-digit backreference numbers, there is some ambiguity with octal character notation, but that is neatly solved by considering how many captured patterns are available. For instance, if Perl sees a \1 metasymbol, it's equivalent to $1 only if there are at least 11 substrings captured earlier in the pattern. Otherwise, it's equivalent to \01, that is, a tab character.

 `/^((\w+) (\w+))$/`

would capture "Primula Brandybuck" into $, "Primula" into $, and "Brandybuck" into $3. This is depicted in [Figure 5-1](ch05_07.htm#perl3-backrefs).

#### Figure 5.1 Creating backreferences with parentheses

Patterns with captures are often used in list context to populate a list of values, since the pattern is smart enough to return the captured substrings as a list:

```python
> ($first, $last)        =  /^(\w+) (\w+)$/;
> ($full, $first, $last) =  /^((\w+) (\w+))$/;
```

With the /g modifier, a pattern can return multiple substrings from multiple matches, all in one list. Suppose you had the mail header we saw earlier all in one string (in $_, say). You could do the same thing as our line-by-line loop, but with one statement:

```python
> %fields = /^(.*?): (.*)$/gm;
```

The pattern matches four times, and each time it matches, it finds two substrings. The /gm match returns all of these as a flat list of eight strings, which the list assignment to %fields will conveniently interpret as four key/value pairs, thus restoring harmony to the universe.

Several other special variables deal with text captured in pattern matches. contains the entire matched string, everything to the left of the match, /tt> everything to the right. $+ contains the contents of the last backreference.

```python
$_ = "Speak, <EM>friend</EM>, and enter.";
m[ (<.*?>) (.*?) (</.*?>) ]x;     # A tag, then chars, then an end tag
print "prematch:              #`Speak,"
print "match:                 #fri"
print "postmatch:             "\n";       
print "lastmatch:             "\n";       
```

For more explanation of these magical Elvish variables (and for a way to write them in English), see [Chapter 28, "Special Names"](ch28_01.htm).

The `@-` (`@LAST_MATCH_START`) array holds the offsets of the beginnings of any submatches, and `@+` (`@LAST_MATCH_END`) holds the offsets of the ends:

```python
#!/usr/bin/perl
$alphabet = "abcdefghijklmnopqrstuvwxyz";
$alphabet =~ /(hi).*(stu)/;

print "The entire match began at $-[0] and ended at $+[0]\n";
print "The first  match began at $-[1] and ended at $+[1]\n";
print "The second match began at $-[2] and ended at $+[2]\n";
```

If you really want to match a literal parenthesis character instead of having it interpreted as a metacharacter, backslash it:

This matches a parenthesized example (e.g., this statement). But since dot is a wildcard, this also matches any parenthetical statement with the first letter e and third letter g (ergo, this statement too).

### 5.7.2 Clustering

>Bare parentheses both cluster and capture. But sometimes you don't want that. Sometimes you just want to group portions of the pattern without creating a backreference. You can use an extended form of parentheses to suppress capturing: the (?:_PATTERN_) notation will _cluster_ without capturing.

There are at least three reasons you might want to cluster without capturing:

1. To quantify something.
2. To limit the scope of interior alternation; for example, `/^cat|cow|dog$`/ needs to be  `/^(?:cat|cow|dog)$/` so that the cat doesn't run away with the
3. To limit the scope of an embedded pattern modifier to a particular subpattern, such as in /foo(?-i:Case_Matters)bar/i. (See the next section, [Section 5.7.3, "Cloistered Pattern Modifiers"](ch05_07.htm#ch05-sect-cloist))

In addition, it's more efficient to suppress the capture of something you're not going to use. On the minus side, the notation is a little noisier, visually speaking.

In a pattern, a left parenthesis immediately followed by a question mark denotes a regex _extension_. The current regular expression bestiary is relatively fixed--we don't dare create a new metacharacter, for fear of breaking old Perl programs. Instead, the extension syntax is used to add new features to the bestiary.

In the remainder of the chapter, we'll see many more regex extensions, all of which cluster without capturing, as well as doing something else. The (?:_PATTERN_) extension is just special in that it does nothing else. So if you say:

```python
> @fields = split(/\b(?:a|b|c)\b/)
```

it's like:

```python
> @fields = split(/\b(a|b|c)\b/)
```

### 5.7.3\. Cloistered Pattern Modifiers

You may _cloister_ the `/i`, `/m`, `/s`, and `/x` modifiers within a portion of your pattern by inserting them (without the slash) between the ? and : of the clustering notation. If you say:

> `/Harry (?i:s) Truman/`

it matches both "Harry S Truman" and "Harry s Truman", whereas:
> `/Harry (?x: [A-Z] \.? \s )?Truman/`

matches both "Harry S Truman" and "Harry S. Truman", as well as "Harry Truman", and:

> `/Harry (?ix: [A-Z] \.? \s )?Truman/`

matches all five, by combining the /i and /x modifiers within the cloister.
You can also subtract modifiers from a cloister with a minus sign:

> `/Harry (?x-i: [A-Z] \.? \s )?Truman/i`

This matches any capitalization of the name--but if the middle initial is provided, it must be capitalized, since the `/i` applied to the overall pattern is suspended inside the cloister.

By omitting the colon `and _PATTERN_`, you can export modifier settings to an outer cluster, turning it into a cloister. That `is`, you can selectively turn modifiers on and off for the cluster one level outside the modifiers' parentheses, like so:

```python
> /(?i)foo/            # Equivalent to /foo/i
> /foo((?-i)bar)/i     # "bar" must be lower case
> /foo((?x-i) bar)/    # Enables /x and disables /i for "bar"
```

Note that the second and third examples create backreferences. If that wasn't what you wanted, then you should have been using (?-i:bar) and (?x-i: bar), respectively.

Setting modifiers on a portion of your pattern is particularly useful when you want "." to match newlines in part of your pattern but not in the rest of it. Setting /s on the whole pattern doesn't help you there
