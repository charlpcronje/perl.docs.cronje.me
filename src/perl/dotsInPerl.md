---
title: Dots and Perl | CRONje.ME
label: Dots and Perl
order: 55
authors:
  - name: Charl Cronje
    email: charl@CRONje.ME
    link: https://blog.cronje.me
    avatar: https://assets.cronje.me/avatars/darker.jpg
---

I was running a training course this week, and a conversation I had with the class reminded me that I have been planning to write this article for many months.

There are a number of operators in Perl that are made up of nothing but dots. How many of them can you name?

There are actually five dot operators in Perl. If the people on my training courses are any guide, most Perl programmers can only name two or three of them. Here’s a list. It’s in approximate order of how well-known I think the operators are (which is, coincidentally, also the order of increasing length).

## One Dot

Everyone knows the one dot operator, right? It’s the concatenation operator. It converts its two operands to strings and concatenates them.

```perl
$string contains 'one stringanother string'
my $string = 'one string' . 'another string';
```

It’s also sometimes useful to use it to force scalar context onto an expression. Consider the difference between these two statements.

```perl
say "Time: ", localtime;
say "Time: " . localtime;
```

The difference between the two statements is tiny, but the output is very different.

There’s not much more to say about the single dot.

## Two Dots

Things start to get a little more interesting when we look at two dots. The two dot operator is actually two different operators depending on how it is used. Most people know that it can be used to generate a range of values.

```perl
my @numbers = (1 .. 100);
my @letters = ('a' .. 'z');
```

You can also use it in something like a for loop. This is an easy way to execute an operation a number of times.

```perl
do_something() for 1 .. 3;
```

In older versions of Perl, this could potentially eat up a lot of memory as a temporary array was created to contain the range and therefore something like

```perl
do_something() for 1 .. 1000000;
```

could be a problem. But in all modern versions of Perl, that temporary array is not created and the expression causes no problems.

Two dots acts as a range operator when it is used in list context. In scalar context, its behaviour is different. It becomes a different operator – the flip-flop operator.

The flip-flop operator is so-called because it flip-flops between two states. It starts as returning false and continues to do so until something “flips” it into its true state. It then continues to return true until something else “flops” it back into a false state. The cycle then repeats.

So what causes it to flip-flop between its two states? It’s the evaluation of its left and right operands. Imagine you are processing a file that contains text that you are interested in and other text that you can ignore. The start of a block that you want to process is marked with a line containing “START” and the end of the block is marked with “END”. Between and “END” marker and the next “START”, there can be lots of text that you want to ignore.

A naive way to process this would involve some kind of “process this” flag.

```perl
my $process_this = 0;
while (<$file>) {
  $process_this = 1 if /START/;
  $process_this = 0 if /END/;
  process_this_line($_) if $process_this;
}
```

I’ve often seen code that looks like this. The author of that code didn’t know about the flip-flop operator. The flip-flop operator encapsulates all of that logic for you. Using the flip-flop operator, the previous code becomes this:

```perl
while (<$file>) {
  process_this_line($_) if /START/ .. /END/;
}
```

The flip-flop operator returns false until its left operand (/START/) evaluates as true. It then returns true until its right operand `(/END/)` evaluates as true. And then the cycle repeats.

The flip-flop operator has one more trick up its sleeve. One common requirement is to only process certain line numbers in a file (perhaps we just want to process lines 20 to 40). If one of the operands is a constant number, then it is compared the the current record number (in $.) and the operand is fired if the line number matches. So processing lines 20 to 40 of a file is a simple as:

```perl
while (<$file>) {
  process_this_line($_) if 20 .. 40;
}
```

## Three Dots
It was the three dot operator that triggered the conversation which reminded me to write this article. A three dot operator was added in Perl 5.12. It’s called the “yada-yada” operator. It is used to stand for unimplemented code.

```perl
sub reverse_polarity {
  # TODO: Must write this
  ...
}
```

Programmers have been leaving `“TODO”` comments in their code for decades. And they’ve been using ellipsis (a.k.a. three dots) to signify unimplemented code for just as long. But now you can actually put three dots in your source code and it will compile and run. So what’s the benefit of this over just leaving a TODO comment? Well, what happens if you call a function that just contains a comment? The function executes and does nothing. You might not realise that you haven’t implemented that function yet. With the `yada-yada` operator standing in for the unimplemented code, Perl throws an “unimplemented” error and you are reminded that you need to write that code before calling it.

But the yada-yada operator wasn’t the first three dot operator in Perl. There has been another one in the language for a very long time (you might say since the year dot!) And I bet very few of you know what it is.

The original three dot operator is another flip-flop operator. And the difference between it and the two dot version is subtle. It’s all to do with how many tests can be run against the same line. With the two dot version, when the operator flips to true it also checks the right-hand operand in the same iteration – meaning that it can flip from false to true and back to false again as part of the same iteration. If you use the three dot version, then once the operator flips to true, then it won’t check the right-hand operand until the next iteration – meaning that the operator can only flip once per iteration.

When does this matter? Well if our text file sometimes contains empty records that contain START and END on the same line, the difference between the two and three dot flip-flops will determine exactly which lines are processed.

If the two dot version encounters one of these empty records, it flips to true (because it matches `/START/`) and then flops back to false (because it matches `(/END/)`. However, the flop to false doesn’t happen until after the expression returns a value (which is true). The net effect, therefore is that the line is printed, but the flip-flop is left in the false state and the following lines won’t be printed until one contains a START.

If the three dot version encounters one of these empty records, it also flips to true (because it matches `/START/`) but then doesn’t check the right-hand operand. So the line gets printed and the flip-flop remains in its true state so the following lines will continue to be printed until one contains an end.

Which is correct? Well, of course, it depends on your requirements. In this case, I expect that the two dot version gives the results that most people would expect. But the three dot version is also provided for the cases when its behaviour is required. As always, Perl gives you the flexibility to do exactly what you want.

But I suspect that the relatively small number of people who seem to know about the three dot flip-flip would indicate that its behaviour isn’t needed very often.

So, there you have them. Perl’s five dot operators – concatenation, range, flip-flop, yada-yada and another flip-flop. I hope that helps you impress people in your next Perl job interview.

Update: dakkar points out that yada-yada isn’t actually an operator. He’s absolutely right, of course, it stands in place of a statement and has no operands. But being slightly loose with our terminology here makes for a more succinct article.
