---
title: Singleton Perl Module | CRONje.ME
label: Singleton Perl Module
order: 54
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---

```sh
#============================================================================
#
# Class::Singleton.pm
#
# Implementation of a "singleton" module which ensures that a class has
# only one instance and provides global access to it.  For a description
# of the Singleton class, see "Design Patterns", Gamma et al, Addison-
# Wesley, 1995, ISBN 0-201-63361-2
#
# Written by Andy Wardley <abw@wardley.org>
#
# Copyright (C) 1998 Canon Research Centre Europe Ltd.
# Copyright (C) 1998-2008 Andy Wardley.  All rights reserved.
# Copyright (C) 2014 Steve Hay.  All rights reserved.
#
# This module is free software; you can redistribute it and/or modify it under
# the same terms as Perl itself, i.e. under the terms of either the GNU General
# Public License or the Artistic License, as specified in the F<LICENCE> file.
#
#============================================================================

package Class::Singleton;
use 5.008001;
use strict;
use warnings;

our $VERSION = 1.6;
my %_INSTANCES = ();

instance();
```

Module constructor.  Creates an `Class::Singleton` (or derived) instance
if one doesn't already exist.  The instance reference is stored in the
%_INSTANCES hash of the `Class::Singleton` package.  The impact of this is
that you can create any number of classes derived from `Class::Singleton`
and create a single instance of each one.  If the instance reference
was stored in a scalar `$_INSTANCE` variable, you could only instantiate
*ONE* object of *ANY* class derived from `Class::Singleton`.  The first
time the instance is created, the `_new_instance()` constructor is called
which simply returns a reference to a blessed hash.  This can be
overloaded for custom constructors.  Any additional parameters passed to
`instance()` are forwarded to `_new_instance().`
Returns a reference to the existing, or a newly created `Class::Singleton`
object.  If the `_new_instance()` method returns an undefined value
then the constructor is deemed to have failed.


```js
sub instance {
    my $class = shift;

    # already got an object
    return $class if ref $class;

    # we store the instance against the $class key of %_INSTANCES
    my $instance = $_INSTANCES{$class};
    unless(defined $instance) {
        $_INSTANCES{$class} = $instance = $class->_new_instance(@_);
    }
    return $instance;
}
```

```js
has_instance()

sub has_instance {
    my $class = shift;
    $class = ref $class || $class;
    return $_INSTANCES{$class};
}
```

```js
 _new_instance(...)

 Simple constructor which returns a hash reference blessed into the
 current class.  May be overloaded to create non-hash objects or
 handle any specific initialisation required.


sub _new_instance {
    my $class = shift;
    my %args  = @_ && ref $_[0] eq 'HASH' ? %{ $_[0] } : @_;
    bless { %args }, $class;
}

END()
```

`END` block to explicitly destroy all `Class::Singleton` objects since
destruction order at program exit is not predictable. See `CPAN RT`
bugs #23568 and #68526 for examples of what can go wrong without this.

```js
END {
    # dereferences and causes orderly destruction of all instances
    undef(%_INSTANCES);
}
```

__END__

=== NAME

Class::Singleton - Implementation of a "Singleton" class

=== SYNOPSIS

    use Class::Singleton;

    my $one = Class::Singleton->instance();   # returns a new instance
    my $two = Class::Singleton->instance();   # returns same instance

=== DESCRIPTION

This is the `Class::Singleton` module.  A Singleton describes an object class
that can have only one instance in any system.  An example of a Singleton
might be a print spooler or system registry.  This module implements a
Singleton class from which other classes can be derived.  By itself, the
`Class::Singleton` module does very little other than manage the instantiation
of a single object.  In deriving a class from `Class::Singleton`, your module
will inherit the Singleton instantiation method and can implement whatever
specific functionality is required.

- For a description and discussion of the Singleton class, see
"Design Patterns", Gamma et al, Addison-Wesley, 1995, ISBN 0-201-63361-2.

=== head2 Using the `Class::Singleton` Module

To import and use the `Class::Singleton` module the following line should
appear in your Perl program:

```js
use Class::Singleton;
```

The `instance()` method is used to create a new `Class::Singleton` instance,
or return a reference to an existing instance. Using this method, it is only
possible to have a single instance of the class in any system.

```js
my $highlander = Class::Singleton->instance();
```

Assuming that no `Class::Singleton` object currently exists, this first call
`instance()` will create a new `Class::Singleton` and return a reference
to it. Future invocations of `instance()` will return the same reference.

```js
my $macleod    = Class::Singleton->instance();
```

In the above example, both `$highlander` and `$macleod` contain the same
reference to a `Class::Singleton` instance.  There can be only one.

=== Deriving Singleton Classes

A module class may be derived from `Class::Singleton` and will inherit the
`instance()` method that correctly instantiates only one object.

```R
package PrintSpooler;
use base 'Class::Singleton';

# derived class specific code
sub submit_job {
    ...
}

sub cancel_job {
    ...
}

The PrintSpooler class defined above could be used as follows:

use PrintSpooler;

my $spooler = PrintSpooler->instance();

$spooler->submit_job(...);
```

The `instance()` method calls the `_new_instance()` constructor method the
first and only time a new instance is created. All parameters passed to the
`instance()` method are forwarded to `_new_instance()`. In the base class
the `_new_instance()` method returns a blessed reference to a hash array
containing any arguments passed as either a hash reference or list of named
parameters.

```R
package MyConfig;
use base 'Class::Singleton';

sub foo {
    shift->{ foo };
}

sub bar {
    shift->{ bar };
}

package main;

# either: hash reference of named parameters
my $config = MyConfig->instance({ foo => 10, bar => 20 });

# or: list of named parameters
my $config = MyConfig->instance( foo => 10, bar => 20 );

print $config->foo();   # 10
print $config->bar();   # 20
```

Derived classes may redefine the `_new_instance()` method to provide more
specific object initialisation or change the underlying object type (to a list
reference, for example).

```R
    package MyApp::Database;
    use base 'Class::Singleton';
    use DBI;

    # this only gets called the first time instance() is called
    sub _new_instance {
        my $class = shift;
        my $self  = bless { }, $class;
        my $db    = shift || "myappdb";
        my $host  = shift || "localhost";

        $self->{ DB } = DBI->connect("DBI:mSQL:$db:$host")
            || die "Cannot connect to database: $DBI::errstr";

        # any other initialisation...

        return $self;
    }
```

The above example might be used as follows:

```R
    use MyApp::Database;

    # first use - database gets initialised
    my $database = MyApp::Database->instance();
```

Some time later on in a module far, far away...

```R
    package MyApp::FooBar
    use MyApp::Database;

    # this FooBar object needs access to the database; the Singleton
    # approach gives a nice wrapper around global variables.

    sub new {
        my $class = shift;
        bless {
            database => MyApp::Database->instance(),
        }, $class;
    }
````

The `PrintSpooler` instance() method uses a private hash to store
a reference to any existing instance of the object, keyed against the derived
class package name.

This allows different classes to be derived from `PrintSpooler` that can
co-exist in the same system, while still allowing only one instance of any one
class to exist. For example, it would be possible to derive both
'PrintSpooler' and 'MyApp::Database' from `PrintSpooler` and have a
single instance of I<each> in a system, rather than a single instance of
I<either>.

You can use the has_instance() method to find out if a particular class
already has an instance defined.  A reference to the instance is returned or
undef if none is currently defined.

```R
    my $instance = MyApp::Database->has_instance()
        || warn "No instance is defined yet";

=head2 Methods

=over 4

=item instance()
```

This method is called to return a current object instance or create a new
one by calling `_new_instance()`.

```R
=item has_instance()
```

This method returns a reference to any existing instance or undef if none
is defined.

```R
    my $testing = MySingleton1->has_instance()
        || warn "No instance defined for MySingleton1";

=item _new_instance()
```

This "private" method is called by instance() to create a new object
instance if one doesn't already exist. It is not intended to be called
directly (although there's nothing to stop you from calling it if you're
really determined to do so).

It creates a blessed hash reference containing any arguments passed to the
method as either a hash reference or list of named parameters.

```pl
    # either: hash reference of named parameters
    my $example1 = MySingleton1->new({ pi => 3.14, e => 2.718 });

    # or: list of named parameters
    my $example2 = MySingleton2->new( pi => 3.14, e => 2.718 );
```

It is important to remember that the `instance()` method will only call
the `_new_instance()` method once, so any arguments you pass may be silently
ignored if an instance already exists. You can use the has_instance()
method to determine if an instance is already defined.

Patches, bug reports, suggestions or any other feedback is welcome.

Patches can be sent as GitHub pull requests at [https://github.com/steve-m-hay/Class-Singleton/pulls](https://github.com/steve-m-hay/Class-Singleton/pulls)

Bug reports and suggestions can be made on the CPAN Request Tracker at [https://rt.cpan.org/Public/Bug/Report.html?Queue=Class-Singleton](https://rt.cpan.org/Public/Bug/Report.html?Queue=Class-Singleton)

Currently active requests on the CPAN Request Tracker can be viewed at [https://rt.cpan.org/Public/Dist/Display.html?Status=Active;Queue=Class-Singleton](https://rt.cpan.org/Public/Dist/Display.html?Status=Active;Queue=Class-Singleton)

Please test this distribution.  See CPAN Testers Reports at
[https://www.cpantesters.org/](https://www.cpantesters.org/) for details of how to get involved.

Previous test results on CPAN Testers Reports can be viewed at [https://www.cpantesters.org/distro/C/Class-Singleton.html)](https://www.cpantesters.org/distro/C/Class-Singleton.html)

Please rate this distribution on CPAN Ratings at [https://cpanratings.perl.org/rate/?distribution=Class-Singleton](https://cpanratings.perl.org/rate/?distribution=Class-Singleton)

=== AVAILABILITY

The latest version of this module is available from CPAN (see [perlmodlib/"CPAN"](perlmodlib/"CPAN") for details) at

- [https://metacpan.org/release/Class-Singleton](https://metacpan.org/release/Class-Singleton) or
- [https://www.cpan.org/authors/id/S/SH/SHAY/](https://www.cpan.org/authors/id/S/SH/SHAY/) or
- [https://www.cpan.org/modules/by-module/Class/](https://www.cpan.org/modules/by-module/Class/)

The latest source code is available from GitHub at [https://github.com/steve-m-hay/Class-Singleton](https://github.com/steve-m-hay/Class-Singleton)
