---
title: XML::Tiny | CRONje.ME
label: XML::Tiny
order: 39
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---
## DESCRIPTION

`XML::Tiny` is a simple lightweight parser for a subset of XML

## SYNOPSIS

```perl
use XML::Tiny qw(parsefile);
open($xmlfile, 'something.xml);
my $document = parsefile($xmlfile);
```

This will leave $document looking something like this:

```perl
[
    {
        type   => 'e',
        attrib => { ... },
        name   => 'rootelementname',
        content => [
            ...
            more elements and text content
            ...
       ]
    }
]
```

See the rest at: [https://metacpan.org/pod/XML::Tiny]](https://metacpan.org/pod/XML::Tiny)
