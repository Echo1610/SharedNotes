---
title: Evolution of Mammalian Diving Capacity Traced by Myoglobin Net Surface Charge
tags: Study
notebook: Biochemistry
---

# An overview of Perl

Perl is a programming language that highly resembles **natural language** rather than machine language. _It is very interesting_

[TOC]



## Variable types and uses

---------------------------
|    Type    | Sigil |   Example   |                   Is a Name For                   |
|------------|-------|-------------|---------------------------------------------------|
| Scalar     | `$`   | `$cents`    | An individual value (number, string or a pointer) |
| Array      | `@`   | `@large`    | A list of values, keyed by numbers                |
| Hash       | `%`   | `%interest` | A group of values, keyed by string                |
| Subroutine | `&`   | `&how`      | A callable chunk of Perl code                     |
| Typeglob   | `*`   | `*struck`   | Everything named struck                           |

### Singularities

A **Scalar** could save a numeric number, a string, and a reference.

```perl
my $answer = 42;#an integer
my $per = "Camel"; #string
my $cwd = `pwd`; #string output from a command
my $arr = \@myarray; #reference to an array
my $ary = [1, 2, 3, 4, 5]; #reference to an unnamed array, attention the square brackets.
my $hsh = {Na => 19, Cl => 35}; # reference to an unamed hash
```

### Pluralities
#### Arrays

An *array* is an ordered list of *scalars*,[^2] accessed[^1] by the scalar's position in the list. The array is started with `@`

```perl
my @home = ("couch", "chair", "tennis", "pipe"); # attention to the cound brackets.
# list assignments:
($alpha, $omega) = ($omega, $alpha);
my ($potato, $lift, $tennis, $pipe) = @home;
```
However, every element chosen from an array is started with `$`, for every element is a *scalar*.

```perl
$home[1] = "sofa";
```
Since arrays are ordered, you can do various operations on them. Such as stack oprations `pop` and `push`.

#### Hashes

A *hash* is an **unordered** set of *scalars*, accessed by some string value that is associated with each scalar. For this reason hash are often called *associative arrays*. A hash has no beginning or end, and `push` or `pop` doesn't make sense in the occasion.

```perl
my %longday = (
    "Sun" => "Sunday",
    "Mon" => "Monday",
    "Tue" => "Tuesday",
    "Wed" => "Wednesday",
    "Thu" => "Thursday",
    "Fri" => "Friday",
    "Sat" => "Saturday",
);
```

Similarly, every value is started with `$`


```perl
my %wife;
$wife{"Jacob"} = ['Leah', 'Rachel', "Bilhah", "Zilpah"];
```

### Simplicities
Perl has its own `package` declaration. Suppose you want to talk about `Camel`s in Perl, you'd likely start off your `Camel` module by saying:

```perl
package Camel;
```

This has several notable effects. One of them is that Perl will assume from this point on that any global verbs or nouns are about Camels. It does this by automatically prefixing any global name with the module name "`Camel::`". So if you say:

```perl
package Camel;
our $fido = &fetch();
```

then the real name of `$fido` is `$Camel::fido`.
This means that if another module says:

```perl
package Dog;
our $fido = &fetch();
```

Perl won't get confused, because the real name of fido is `$Dog::fido`.

In fact, some of the built-in modules don’t actually introduce verbs at all, but simply wrap the Perl language in various useful ways. We call these special modules **pragmas**. For instance, you’ll often see people use the pragma `strict`, like this:

```perl
use strict;
```

What the `strict` module does is tighten up some of the rules so that you have to be more explicit about various things that Perl would otherwise guess about, such as how you want your variables to be scoped.



[^1]: Or keyed, or indexed, or subscripted, or looked up.
[^2]: This means that you can store every *scalar* in your array, regardless of what the scalar is.

## Verbs

Many verbs in Perl are commands: they tell the Perl interpreter to do something. The meanings of Perl verbs tend to mush off in various directions depending on the context. **A statement starting with a verb is  generally purely imperative and evaluated entirely for its side effects**.[^3] A frequently seen built-in command is the `say` command:

```perl
say "Adam's wife is $wife{'Adam'}.";
```

This has the side effect of producing the desired output:

```
Adam's wife is Eve
```

There are verbs that translate their input parameters into return values. We tend to call these verbs *functions*.

## Filehandles

**A filehandle is just a name you give to a file, device, socket, or pipe to help you remember which one you are talking about, and to hind some of the complexities of buffering and such.**

You create a filehandle and attach it to a file by using `open`. The `open` function takes at least two parameters: the filehandle and filename you want to associate it with. Perl also gives you some predefined (and preopened) filehandles. `STDIN` is your program’s normal input channel, while `STDOUT` is your programs’s normal output channel. And `STDERR` is an additional output channel that allows your program to make snide remarks off to the side while it transforms your input into your output.

You can simply add characters to the filename to specify which behavior you want:

```perl
open(SESAME, "filename"); 						# read from existing file
open(SESAME, "<", "filename");					# (same thing, explicitly)
open(SESAME, ">", "filename");					# create file and write to it
open(SESAME, ">>", "filename");					# append to existing file
open(SESAME, "|-", "output-pipe-command")		# set up an output filter
```

This form of `open` also lets you specify the character encoding of the file:

```perl
open(SESAME, "< :encoding(UTF-8)", 		$somefile);
open(SESAME, "> :crlf",					 $somefile);
open(SESAME, ">> :encoding(MacRoman)",	$somefile);
```

Once opened, the filehandle SESAME can be used to access the file or pipe until it is explicitly close with `close(SESAME)`, or <u>until the filehandle is attached to another file by a subsequent `open` on the same filehandle.</u>

Sometimes it happens accidentally, like when you say `open($handle, $file)`, and `$handle` happens to contain a constant string, then you’ll just open a new file on the same filehandle. A much better idea is to leave `$handle` undefined, letting Perl fill it in for you. Like this:

```perl
open(my $handle, "< :crlf : encoding(cp1252)", $somefile)
	|| die "Cannot open $somefile: $!";
```

Once you’ve opened a filehandle for input, you can read a line using the line reading operator, `<>` (also know as the angle operator). The angle operator encloses the filehandle (`<SESAME>` for a direct one or `<$handle>` for a indirect one) you want to read lines from. The empty angle operator `<>`, will read lines from all the files specified on the command line, or `STDIN` if no arguments were specified. An example:

```perl
print STDOUT "Enter a number: ";		# ask for a number
$number = <STDIN>;					 # input the number
say STDOUT "The number is $number"  # print the number
```

The `STDOUT` could be replaced by other filehandles you want to write to, it is the same with `STDIN`.

A practical function matched for line-reading operator is `chomp`. This function will remove the newline from your input file (the `“\n”`  in `“9\n”`, for example).  You’ll often see this idiom for inputting a single line:

```perl
chomp($number = <STDIN>);
```

which means the same thing as:

```perl
$number = <STDIN>;			# input a number
chomp($number);				# remove trailing newline
```

## Regular Expressions

**A regular expression is a way of describing a set of strings without having to list all the strings in your set.** When you see something that looks like `/foo/` in a conditional, you know you are looking at an ordinary *patttern-matching* operator:

```perl
if (/Windows7/) { print "Time to upgrade?\n"; }
```

Second, if you can locate patterns within a string, you can replace them with something else. So when you see something that looks like `s/foo/bar/`, you know it’s asking Perl to substitute “bar” for “foo”, if possible. It returns true or flase depending on whether it succeeded.

```perl
s/IBM/lenovo/;
```

The regular expression can define*seperators* that delimit the fields of data:

```perl
my ($good, $bad, $ugly) = split(/,/, "vi,emacs,teco");
```

If you match on several characters in a row, they all have to match sequentially. That is, the pattern looks for a substring, much as you’d expect. Let’s say you want to show all the lines of an HTML file that contain HTTP links. We could loop through our file with this:

```perl
while (my $line = <FILE>) {
    if ($line =~ /http:/) {
        print $line;
    }
}
```

Here, the =~ (pattern binding) is telling Perl to look for a match of the regular expression `“http:”` in the variable `$line`. If it finds the expression, the operator returns a true value the block (a `print` statement) is executed.

By the way, if you don’t use the =~ binding operator, Perl will search a default string instead of `$line`. This default string is actually a special scalar variable that goes by the odd name `$_`. In fact, it’s not the default just for pattern matching; many operators in Perl default to using the `$_` variable, so a veteran Perl programmer would likely write the last example as:

```perl
while(<FILE>) {
    print if /http:/;
}
```

***Shortcuts for alphabetic characters***

| Name           | ASCII Definition | Unicode Definition                        | Shortcut |
| -------------- | ---------------- | ----------------------------------------- | -------- |
| Whitespace     | `[ \t\n\r\f]`    | `\p{Whitespace}`                          | `\s`     |
| Word character | `[a-zA-Z_0=9]`   | `[\p{Alphabetic}\p{Digit}\p{Mark}\p{Pc}]` | `\w`     |
| Digit          | `[0-9]`          | ` \p{Digit}`                              | `\d`     |

Note that these match single characters. A `\w` will match any single word character, not an entire word. We should note that **`\w` is not always equivalent to `[a-zA-Z_0-9]`** (and `\d` is not always `[0-9]`). Some locales define additional alphabetic characters outside the ASCII sequence, and `\w` respect them.

There is one other very special character class, written with a “.”, that will match any character whatsoever. For example, `/a./` will match any string containing an “`a`” that is not the last character in the string (“`oasis`”, “`camel`” for a successful match and “`sheba`” for a unsuccessful match). <u>It stops after it finds the first suitable match, searching from left to right.</u>

 ### Quantifiers

The characters and character classes we’ve talked about **all match single characters**. The *quantifiers* specify the numbers and methods a regular expression matches.

| Quantifiers          | Description                                                  | Examples                                                     |
| -------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `{numbers}`          | Specify the explicit numbers of matches. All unquantified items have an implicit `{1}` quantifier. | `\d{7}` match explicitly 7 digits.                           |
| `{minimum, maximum}` | Specify the minimum and maximum for a single match. If maximum is not given, the default is infinity. | `\d{7, 11}` match at least 7 digits, but no more than eleven 11 digits. |
| `+`                  | Equivalent to `{1, }`. Match at least one of the preceding item. |                                                              |
| `*`                  | Equivalent to `{0, }`. Match zero or more of the preceding item. |                                                              |
| `?`                  | Equivalent to `{0, 1}`. Match zero or one of the preceding item (that is, the preceding item is optional). |                                                              |

**Perl quantifiers are all *Greedy*. This means that they will attempt to match as much as they can as long as the whole pattern still matches**. This is something to watch out for especially when you are using “.”, any character. There is a string like:

```
larry:JYHtPh0./NJTU:100:10:Larry Wall:/home/larry:/bin/bash
```

and someone tries to match “`larry:`” with `/.+:/`. However, since the `+` quantifier is greedy, this pattern will match everything up to and includiing “`home/larry:`”, because it matches as much as possible before the laso colon, including the other colons. To avoid this, you can use a negated character class, that is, by saying `/[^:]+:/` or `/.+?:/`

In the above case, the `^` in the `[]` is a negated character, it means that match a pattern that is not listed in the `[]`. So `[^:]` means everything that is not “:”.  The `?` behind `+` is a command that forces the match method of `+` to be minimal match.

The other point to be careful about is that regular expressions will try to match as *early* as possible. This even takes precedence over being greedy. Since scanning happens left to right, the pattern will match as far as possible, even if there is some other place where it could match no longer. For example, suppose you’re using the substitution command (`s///`) on the default string (variable `$_`), and you want to remove a string of x’s from the middle of the string. If you say:

```perl
$_ = "fred xxxxxxx barney";
s/x*//;
```

if will have absolutely no effect! This is because the `x*` (meaning zero or more “x” characters) will be able to match the “nothing” at the beginning of the string, since the null string happens to be zero characters wide and there’s a null string sitting there before “f”.

There is one other thing you need to know. By default, quantifiers apply to a single preceding character, so `/bam{2}/` will match **“bamm”** but not “bambam” (a correct match for this is `/(bam){2}/`).

 ### Nailing Things Down

Whenever you try to match a pattern, it’s going to try to match in every location until it finds a match. An *Anchor* allows you to restrict where a pattern can match. Essentially, an anchor is something that matches a “nothing”, but a special kind of nothing that dependes on its surroundings. You could also call it a constraint, a rule, or an assertion.

The special symbol `\b` matches at a word boundary, which is defined as the “nothing” between a word character (`\w`) and a nonword character (`\W`), in either order. For example:

```perl
/\bFred\b/
```

would match “Fred” in both “The Great Fred” and “Fred the Great”, but not in “Frederick the Great” because the “d”   in “Frederick” is not followed by a non-word character.

There are also anchors for the beginning and the end of the string. If it is the first character of a pattern, the caret (`^`) matches the “nothing”  at the beginning of the string. The dollar sign (`$`) matches the nothing at the end of the string instead of beginning.



[^3]: We sometimes call these verbs *procedures*, especially when they’re user defined.

 