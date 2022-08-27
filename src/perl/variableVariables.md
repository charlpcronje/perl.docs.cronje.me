---
title: Variable Variables | CRONje.ME
label: Variable Variables
order: 52
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---

```perl
use strict;
use warnings;
use 5.10.0;

our $x = '5';
my $field_name = 'x';

my $contents;
{ # no strict is lexially scoped
    no strict 'refs';
    $contents = ${$field_name};
}
```