---
title: Perl Regex | CRONje.ME
label: Perl Regex
order: 28
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---
# Perl Text Patterns for Search and Replace

## Perl’s Rich Support for Regular Expressions

Perl was originally designed by Larry Wall as a flexible text-processing language.  Over the years, it has grown into a full-fledged programming language, keeping a strong focus on text processing.  When the world wide web became popular, Perl became the de facto standard for creating CGI scripts.  A CGI script is a small piece of software that generates a dynamic web page, based on a database and/or input from the person visiting the website.  Since CGI script basically is a text-processing script, Perl was and still is a natural choice.

Because of Perl’s focus on managing and mangling text, `[regular expression text patterns](https://www.regular-expressions.info/index.html)` are an integral part of the Perl language.  This in contrast with most other languages, where regular expressions are available as add-on libraries.  In Perl, you can use the `m//` operator to test if a regex can match a string, e.g.:

```perl
if ($string =~ m/regex/) {
  print 'match';
} else {
  print 'no match';
}
```

## Performing a regex search-and-replace is just as easy:

```perl
$string =~ s/regex/replacement/g;
```

I added a “g” after the last forward slash.  The “g” stands for “global”, which tells Perl to replace all matches, and not just the first one.  Options are typically indicated including the slash, like “/g”, even though you do not add an extra slash, and even though you could use any non-word character instead of slashes.  If your regex contains slashes, use another character, like `s!regex!replacement!g`

You can add an “i” to make the regex match case insensitive.  You can add an “s” to make the [dot](https://www.regular-expressions.info/dot.html) match newlines.  You can add an “m” to make the [dollar and caret](ahttps://www.regular-expressions.info/anchors.html) match at newlines embedded in the string, as well as at the start and end of the string.

Together you would get something like `m/regex/sim;`

## Regex-Related Special Variables

Perl has a host of special variables that get filled after every `m//` or `s///` regex match.  `$1`, `$2`, `$3`, etc. hold the [backreferences](https://www.regular-expressions.info/backref.html).  `$+` holds the last (highest-numbered) back reference. `$&` (dollar ampersand) holds the entire regex match.

`@-` is an array of match-start indices into the string.  `$-[0]` holds the start of the entire regex match, `$-[1]` the start of the first back reference, etc.  Likewise, `@+` holds match-end positions.  To get the length of the match, subtract `$+[0]` from `$-[0]`.

In Perl 5.10 and later you can use the associative array `%+` to get the text matched by [named capturing groups](https://www.regular-expressions.info/named.html).  For example, `$+{name}` holds the text matched by the group “name”.  Perl does not provide a way to get match positions of capturing groups by referencing their names.  Since named groups are also numbered, you can use `@-` and `@+` for named groups, but you have to [figure out the group’s number](https://www.regular-expressions.info/named.html#number) by yourself.

`$'` (dollar followed by an apostrophe or single quote) holds the part of the string after (to the right of) the regex match.  `$‘` (dollar backtick) holds the part of the string before (to the left of) the regex match.  Using these variables is not recommended in scripts when performance matters, as it causes Perl to slow down *all* regex matches in your entire script.

All these variables are read-only, and persist until the next regex match is attempted.  They are dynamically scoped, as if they had an implicit ‘local’ at the start of the enclosing scope.  Thus if you do a regex match, and call a sub that does a regex match, when that sub returns, your variables are still set as they were for the first match.

## Finding All Matches In a String

The “/g” modifier can be used to process all regex matches in a string.  The first `m/regex/g` will find the first match, the second `m/regex/g` the second match, etc.  The location in the string where the next match attempt will begin is automatically remembered by Perl, separately for each string.  Here is an example:

```perl
while ($string =~ m/regex/g) {
  print "Found '$&'.  Next attempt at character " . pos($string)+1 . "\n";
}
```

The `pos()` function retrieves the position where the next attempt begins.  The first character in the string has position zero.  You can modify this position by using the function as the left side of an assignment, like in `pos($string) = 123;`