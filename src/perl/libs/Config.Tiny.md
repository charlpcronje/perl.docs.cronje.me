---
title: Config::Tiny | CRONje.ME
label: Config::Tiny
order: 43
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---
## Reading configuration INI files in Perl

The [INI file format](https://en.wikipedia.org/wiki/INI_file) is a simple file format that allows 2-level configuration options. It was extensively used in MS DOS and MS Windows, and it can be used in lots of places as it is very simple.

## examples/config.ini

```ini
[openweathermap]
api = asdahkaky131
 
[aws]
api_key = myaws7980
api_code = qkhdkadyday
 
[digital_ocean]
api = seaworld
```

This format consists of section names (in square brackets) and in each section key-value pairs separated by and = sign and padded by spaces.

There are several articles showing how to read them using plain Perl for example [processing config file](https://perlmaven.com/beginner-perl-maven-process-config-file) and the exercise [parse INI file](https://perlmaven.com/beginner-perl-maven-exercise-parse-ini-file) however there are several modules on `CPAN` that would do the work for you in a more generic way.

We'll use [`Config::Tiny`](https://metacpan.org/pod/Config::Tiny)

## examples/read_config.pl

```perl
use strict;
use warnings;
use 5.010;
 
use Config::Tiny;
use Data::Dumper qw(Dumper);
 
my $filename = shift or die "Usage: $0 FILENAME\n";
 
my $config = Config::Tiny->read( $filename, 'utf8' );
 
say $config->{digital_ocean}{api};     # seaworld
 
say $config->{openweathermap}{api};    # asdahkaky131
 
say $config->{aws}{api_key};           # myaws7980
say $config->{aws}{api_code};          # qkhdkadyday
 
 
print Dumper $config;
```

The line that reads and parses the configuration file i `Config::Tiny->read`.

It returns a reference to a 2-dimensional hash. The main hash has the sections as keys and the values are the hashes representing each individual section.

We can then access the specific values using the `$config->{section}{key}` construct.

If we use Data::Dumper we can see the whole data structure:

```perl
$VAR1 = bless( {
    'digital_ocean' => {
                        'api' => 'seaworld'
                    },
    'openweathermap' => {
                        'api' => 'asdahkaky131'
                        },
    'aws' => {
            'api_code' => 'qkhdkadyday',
            'api_key' => 'myaws7980'
            }
}, 'Config::Tiny' );
```

## Write config file

[Config::Tiny](https://metacpan.org/pod/Config::Tiny) can also write config files and it can also handle key-value pairs without any section. Check the documentation for details.