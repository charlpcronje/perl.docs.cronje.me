---
title: Set::Tiny | CRONje.ME
label: Set::Tiny
order: 40
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---
## DESCRIPTION

`Set::Tiny` is a thin wrapper around regular Perl hashes to perform often needed set operations, such as testing two sets of strings for equality, or checking whether one is contained within the other.

For a more complete implementation of mathematical set theory, see `Set::Scalar`. For sets of arbitrary objects, see `Set::Object`.

## SYNOPSIS

```perl
use Set::Tiny;
 
my $s1 = Set::Tiny->new(qw( a b c ));
my $s2 = Set::Tiny->new(qw( b c d ));
 
my $u  = $s1->union($s2);
my $i  = $s1->intersection($s2);
my $s  = $s1->symmetric_difference($s2);
 
print $u->as_string; # (a b c d)
print $i->as_string; # (b c)
print $s->as_string; # (a d)
 
print "i is a subset of s1"   if $i->is_subset($s1);
print "u is a superset of s1" if $u->is_superset($s1);
 
# or using the shorter initializer:
 
use Set::Tiny qw( set );
 
my $s1 = set(qw( a b c ));
my $s2 = set([1, 2, 3]);
```\

Read more at: [https://metacpan.org/pod/Set::Tiny](https://metacpan.org/pod/Set::Tiny)