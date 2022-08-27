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


# Hash::Digger

Hash::Digger - Access nested hash structures without vivification

## VERSION

Version 0.0.5

## SYNOPSIS

Allows accessing hash structures without triggering autovivification.

```perl
my %hash;
 
$hash{'foo'}{'bar'} = 'baz';
 
diggable \%hash, 'foo', 'bar';
# Truthy
 
diggable \%hash, 'xxx', 'yyy';
# Falsey
 
dig \%hash, 'foo', 'bar';
# 'baz'
 
dig \%hash, 'foo', 'bar', 'xxx';
# undef
 
exhume 'some default', \%hash, 'foo', 'bar';
# 'baz'
 
exhume 'some default', \%hash, 'foo', 'xxx';
# 'some default'
 
# Hash structure has not changed:
use Data::Dumper;
Dumper \%hash;
# $VAR1 = {
#           'foo' => {
#                      'bar' => 'baz'
#                    }
#         };
```

## EXPORT

dig, diggable, exhume

## SUBROUTINES/METHODS

### `diggable`

Check if given path is diggable on the hash (`exists` equivalent)

### `dig`

Dig the hash and return the value. If the path is not valid, it returns undef.

### `exhume`

Dig the hash and return the value. If the path is not valid, it returns a default value.