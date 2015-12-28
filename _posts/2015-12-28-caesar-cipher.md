---
layout:     post
title:      taking (another) look at caesar cipher
date:       2015-12-28 20:00:00
categories: code
thumbnail:  pencil-square-o
---

Sometime ago, I had to write a piece of code that would cipher some text. The most
simplest cipher there is; the Caesar Cipher.

Now, this isn't a post to tell you that I'm going to introduce a completely new
algorithm, because I'm not. I'm trying to relook how it's being implemented.

Since this was just a demo program, and with me thinking that the users should
have some control, I decided to do this:
> Allow users to set the _offset_ for the cipher.

That, turned out later, to be a **big** mistake.

No, I did parse for integers, and even did a small while loop catching the return
value of scanf (_...overkill for demo code..._), but the problem with users is
that they quickly try to make the program crash; and which is exactly what they
figured out to do within a couple of tries:
> _offsets_ > -26

Let us quickly look at the basic algorithm for Caesar Cipher'ing text: Take a char,
add or subtract the offset, modulo 26. And that should have worked. In short,
this;

``` cpp
ch = (ch - 'a' + offset) % 26
```

should've worked.

For some reason, this didn't seem to work. Why? I had the slightest idea.

I quickly put together this program, with a ```C_CIPHER``` of -36. I immediately
saw why the program broke.

<script src="https://gist.github.com/thewisenerd/478a343d6b0ccbd79689.js"></script>

The output was as follows:

```
(0, 10), (1, 9), (2, 8), (3, 7), (4, 6), (5, 5), (6, 4), (7, 3),
(8, 2), (9, 1), (10, 0), (11, 25), (12, 24), (13, 23), (14, 22),
(15, 21), (16, 20), (17, 19), (18, 18), (19, 17), (20, 16),
(21, 15), (22, 14), (23, 13), (24, 12), (25, 11), 
```

An offset of ```-36``` should've given me a value of 16 for 0, but that didn't
happen. So, why?

> That's the way ```%``` operates (in C).

So, the result of the modulo operator can do two things:

 * take the sign of the dividend (truncated division), or
 * take the sign of the divisor (floored division).

tl;dr; I'll tell you how this affects negative numbers.
```truncated division``` counts up to zero, while
```floored division``` counts down from zero.

I'll let the wikipedia page about the [Modulo Operation](https://en.wikipedia.org/wiki/Modulo_operation)
to explain more about the two.

It is important to note one of the pitfalls being mentioned in the same page.

When the result of the modulo operation takes the sign of the dividend, some
suprising mistakes may occour. One among them, is the testing for whether an
integer is odd using the modulo operator.

Run the below code, and you'll know immediately what I'm talking about.

<script src="https://gist.github.com/thewisenerd/4d52880d17b1c0455028.js"></script>

Fortunately, you don't have to worry about these pitfalls since python uses
floored division.

_sidenote: ```n & 1``` might be the only test you need for odd integers_

So, how do you fix this?

Simple. Use a modulo operator that does floored division.

The fix for this program, is this:

``` cpp
#define pyMod(n, M) (((n % M) + M) % M)
```

(_credits to [Alex B](http://stackoverflow.com/users/23643/alex-b) for his answer [here](http://stackoverflow.com/a/1907585/2873157)_)

And then, in my program, I go ahead and replace:

``` cpp
(ch + C_CIPHER) % 26
```

with

``` cpp
pyMod((ch + C_CIPHER), 26)
```

... and the program is good to go!

---

It is improtant to note that two of the major up and coming languages, [Go](https://golang.org/)
and [Rust](https://www.rust-lang.org/) too go with truncated division, instead
of floored. Even Python's ```math.fmod``` goes to use truncated divison (_of sorts_).

Maybe it's because of the rigid definition for a modulo operator we've given.

Eitherways, just keep in mind what your ```%``` operator does _\*wink\* \*wink\*_.