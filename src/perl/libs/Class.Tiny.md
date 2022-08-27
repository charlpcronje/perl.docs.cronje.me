---
title: Hash::Digger | CRONje.ME
label: Hash::Digger
order: 50
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---
# Class::Tiny

## DESCRIPTION

- This module offers a minimalist class construction kit in around 120 lines of code. Here is a list of features:
- defines attributes via import arguments
- generates read-write accessors
- supports lazy attribute defaults
- supports custom accessors
- superclass provides a standard new constructor
- new takes a hash reference or list of key/value pairs
- new supports providing `BUILDARGS` to customize constructor options
- new calls BUILD for each class from parent to child
- superclass provides a DESTROY method
- DESTROY calls DEMOLISH for each class from child to parent

Multiple-inheritance is possible, with superclass order determined via mro::get_linear_isa.

It uses no non-core modules for any recent Perl. On Perls older than v5.10 it requires `MRO::Compat`. On Perls older than v5.14, it requires `Devel::GlobalDestruction`.

## USAGE

### Defining attributes

Define attributes as a list of import arguments:

```perl
package Foo::Bar;
 
use Class::Tiny qw(
    name
    id
    height
    weight
);
```

For each attribute, a read-write accessor is created unless a subroutine of that name already exists:

```perl
$obj->name;               # getter
$obj->name( "John Doe" ); # setter
```

See the rest at: [https://metacpan.org/pod/Class::Tiny](https://metacpan.org/pod/Class::Tiny)
