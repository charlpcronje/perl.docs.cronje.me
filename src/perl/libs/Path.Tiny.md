---
title: Path::Tiny | CRONje.ME
label: Path::Tiny
order: 41
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---

This module provides a small, fast utility for working with file paths. It is friendlier to use than File::Spec and provides easy access to functions from several other core file handling modules. It aims to be smaller and faster than many alternatives on CPAN, while helping people do many common things in consistent and less error-prone ways.

`Path::Tiny` does not try to work for anything except Unix-like and Win32 platforms. Even then, it might break if you try something particularly obscure or tortuous. (Quick! What does this mean: `///../../..//./././a//b/.././c/././?` And how does it differ on `Win32?`)

All paths are forced to have `Unix-style` forward slashes. `Stringifying` the object gives you back the path (after some clean up).

File input/output methods flock handles before reading or writing, as appropriate (if supported by the platform and/or filesystem).

The `*_utf8` methods (`slurp_utf8`, `lines_utf8`, etc.) operate in raw mode. On Windows, that means they will not have CRLF translation from the `:crlf` IO layer. Installing Unicode::UTF8 0.58 or later will speed up *_utf8 situations in many cases and is highly recommended. Alternatively, installing PerlIO::utf8_strict 0.003 or later will be used in place of the default ':encoding(UTF-8)'.

This module depends heavily on PerlIO layers for correct operation and thus requires Perl 5.008001 or later.

```perl
use Path::Tiny;
 
# creating Path::Tiny objects
 
$dir = path("/tmp");
$foo = path("foo.txt");
 
$subdir = $dir->child("foo");
$bar = $subdir->child("bar.txt");
 
# stringifies as cleaned up path
 
$file = path("./foo.txt");
print $file; # "foo.txt"
 
# reading files
 
$guts = $file->slurp;
$guts = $file->slurp_utf8;
 
@lines = $file->lines;
@lines = $file->lines_utf8;
 
($head) = $file->lines( {count => 1} );
($tail) = $file->lines( {count => -1} );
 
# writing files
 
$bar->spew( @data );
$bar->spew_utf8( @data );
 
# reading directories
 
for ( $dir->children ) { ... }
 
$iter = $dir->iterator;
while ( my $next = $iter->() ) { ... }
```

## CONSTRUCTORS

### path

```perl
$path = path("foo/bar");
$path = path("/tmp", "file.txt"); # list
$path = path(".");                # cwd
$path = path("~user/file.txt");   # tilde processing
```

Constructs a `Path::Tiny` object. It doesn't matter if you give a file or directory path. It's still up to you to call directory-like methods only on directories and file-like methods only on files. This function is exported automatically by default.

The first argument must be defined and have non-zero length or an exception will be thrown. This prevents subtle, dangerous errors with code like `path( maybe_undef() )->remove_tree`.

If the first component of the path is a tilde ('~') then the component will be replaced with the output of glob('~'). If the first component of the path is a tilde followed by a user name then the component will be replaced with output of `glob('~username')`. Behaviour for non-existent users depends on the output of glob on the system.

On Windows, if the path consists of a drive identifier without a path component (C: or D:), it will be expanded to the absolute path of the current directory on that volume using `Cwd::getdcwd()`.

If called with a single `Path::Tiny` argument, the original is returned unless the original is holding a temporary file or directory reference in which case a stringified copy is made.

```perl
$path = path("foo/bar");
$temp = Path::Tiny->tempfile;
 
$p2 = path($path); # like $p2 = $path
$t2 = path($temp); # like $t2 = path( "$temp" )
```

See the rest at: [https://metacpan.org/pod/Path::Tiny](https://metacpan.org/pod/Path::Tiny)
