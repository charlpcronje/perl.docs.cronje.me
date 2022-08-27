---
title: HTML::Tiny | CRONje.ME
label: HTML::Tiny
order: 42
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---
HTML::Tiny - Lightweight, dependency free HTML/XML generation

```perl
use HTML::Tiny;
 
my $h = HTML::Tiny->new;
 
# Generate a simple page
print $h->html(
  [
    $h->head( $h->title( 'Sample page' ) ),
    $h->body(
      [
        $h->h1( { class => 'main' }, 'Sample page' ),
        $h->p( 'Hello, World', { class => 'detail' }, 'Second para' )
      ]
    )
  ]
);
 
# Outputs
<html>
  <head>
    <title>Sample page</title>
  </head>
  <body>
    <h1 class="main">Sample page</h1>
    <p>Hello, World</p>
    <p class="detail">Second para</p>
  </body>
</html>
```

## DESCRIPTION

`HTML::Tiny` is a simple, dependency free module for generating `HTML (and XML)`. It concentrates on generating syntactically correct XHTML using a simple Perl notation.

In addition to the HTML generation functions utility functions are provided to

- encode and decode URL encoded strings
- entity encode HTML
- build query strings
- JSON encode data structures

## INTERFACE

`new`
Create a new `HTML::Tiny`. The constructor takes one optional argument: `mode. mode` can be either `'xml'` (default) or `'html'`. The difference is that in HTML mode, closed tags will not be closed with a forward slash; instead, closed tags will be returned as single open tags.

Example:

```perl
# Set HTML mode.
my $h = HTML::Tiny->new( mode => 'html' );
 
# The default is XML mode, but this can also be defined explicitly.
$h = HTML::Tiny->new( mode => 'xml' );
```

See the rest at: [https://metacpan.org/pod/HTML::Tiny](https://metacpan.org/pod/HTML::Tiny);