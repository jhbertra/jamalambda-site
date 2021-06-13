---
title: Elm - Amazing, Informative, Paternalistic Error Messages
author: jamalambda
---

Welcome to my inaugural blog post! I decided to start things off pretty light by
publishing what is really a pretty pathetic complaint about one of my favorite
languages. If you woke up this morning thinking to yourself "I just want to read
some windbag go on for way longer than necessary about trivial annoyances to a
super niche audience," then you my friend are in _exactly_ the right place. Also,
may I recommend Reddit?

If not - TL;DR accessible design is great, and targeting the lowest common
denominator is preferable to elitism. But better still is responsive, customizable
design, which respects the level of the practitioner, whatever it may be.

The other day I wrote a proof-of-concept for a project I'm working on. I decided to
use Elm to do it. It had been a little while since I had done anything in Elm, but
each time I do, it feels like I'm driving a luxury car. It might not score the highest
in terms of sheer utility, but there is no denying that it is a smooth, sleek,
meticulously curated experience. Every aspect of the language feels like it was
designed with intense scrutiny and craftsmanship. The experience of writing an Elm
app is quite unlike any other programming experience I've ever had. It's a sort of
continuous back-and-forth between you and its famously friendly compiler. You provide
some code, ask the compiler to check it, and it spits out a list of errors that quite
literally tell you what you need to do to get the program to compile. It feels like
you are taking part in a tightly choreographed dance, and that the compiler already
knows what your finished program will look like, and your job is catch up to it.
If you try to do something that doesn't fit the choreography, it will politely tell
you that you've gone astray, and guide you gently back to the path of enlightenment.

At the forefront of this dance are the compiler's error messages. Elm is famous for
its extremely easy to understand error messages. Unlike the approach taken by many
of its contemporaries, such as Haskell or PureScript (or even TypeScript), in which
compiler errors are described in cryptic and low-level jargon, accompanied by
overly-verbose dumps of the type-checker's internal state, leaving the developer to
parse the information and deduce their mistake from it, Elm does a lot of that work
for you. It hides the internal representation of the failure, and presents to you in
plain, simple English the exact mistake t thinks you made, along with suggestions for
fixing it. Of course, it is not infallible, and sometimes it will come to the wrong
conclusion about what your actual mistake was, which then requires you to figure
out why it thinks you made that mistake in order to diagnose the real problem, but
on average, Elm errors are incredibly easy to understand. The language designer
wanted the compiler errors to act as a user guide, and treat them as opportunities
to educate developers. I have to say that this really is _fantastic_ design. Elm
errors are nothing short of inspirational when it comes to designing for human-computer
interaction.

But the other day, as I was working on my project, something started to rub me the
wrong way about them. Where once I smiled in joy as the compiler helpfully explained
exactly what I had done wrong, now I found myself rolling my eyes at it. The more I
started seeing messages like this

```
unbound type variable
Line 42, Column 1
The `HeadingStyles` type alias uses an unbound type variable `bool` in its definition:

42| type alias HeadingStyles =
43|>    { bold : bool
44|     , italic : Bool
45|     , underline : Bool
46|     }

You probably need to change the declaration to something like this:

    type alias HeadingStyles bool = ...

Why? Well, imagine one `HeadingStyles` where `bool` is an Int and another where it is a Bool. When we explicitly list the type variables, the type checker can see that they are actually different types.
```

or this

```
name clash
Line 23, Column 12
This file defines multiple `Heading` type constructors. One here:

19|     = Heading (Heading char)
          ^^^^^^^
And another one here:

23| type alias Heading char =
               ^^^^^^^
How can I know which one you want? Rename one of them!
```

or this

```
unfinished type alias
Line 14, Column 29
I am partway through parsing a type alias, but I got stuck here:

14| type alias TextObject char =
                                ^
I was expecting to see a type next. Something as simple as Int or Float would work!

Note: Here is an example of a valid `type alias` for reference:

    type alias Person =
      { name : String
      , age : Int
      , height : Float
      }

This would let us use `Person` as a shorthand for that record type. Using this shorthand makes type annotations much easier to read, and makes changing code easier if you decide later that there is more to a person than age and height!
```

The more I found myself loathing my dance partner's smug attitude. Like "thanks Elm, didn't
occur to me that `Int` and `Float` were examples of valid types." Instead of seeing the
compiler as a patient, wise instructor, I increasingly began to think of it as a condescending,
paternalistic nag that just could not wait to lord over me with its knowledge. Why the change?
Why were the same error messages I once found so helpful and delightful suddenly so unpalatable
to me?

Well, as it turns out, in the time I'd been away from Elm, I had grown significantly as a
software engineer. I had cut my teeth on Haskell's gnarly compiler errors, and become quite
adept at parsing through the gobs of computed types that TypeScript pukes on your screen when
you manage to confuse its type inference engine (spoiler alert - TypeScript's type inference
engine is very easily confused). I was no longer a novice when it came to working with ML
family languages, and Elm is one of the very most simple flavours of the ML family. There
wasn't anything the compiler was telling me that I didn't already know, and I was able to
spot my mistakes immediately without requiring a paragraph of didactic prose to explain what
was going on.

Now don't get me wrong, I don't want Elm to get rid of its helpful error messages. The fact that
they are so verbose and simple makes the language incredibly accessible to newcomers, which is
good news for anyone wanting to learn to use a pure functional language. I like that the language
is so friendly for beginners. The crux of the problem is that its communication style doesn't
take experience into consideration. It treats _everyone_ like they are a novice. For novices, this
is great, but experts don't generally appreciate having basic concepts spelled out for them at
every turn. To be clear, I prefer communication that treats everyone as a novice, as opposed to
communication that assumes everyone to be an expert. The former is undoubtedly more egalitarian
and accessible. But I think we can do better that designing one-size-fits-all solutions.

That's why I'm proposing a design goal for language designers: design for multiple levels of
experience. Maybe this is a flag that lets _me_ tell the _compiler_ how much help I need.
The compiler could assume that the user wants maximum helpfulness by default, and allow those
with more experience to tailor their experience to suit their skill level. It's not just
the programmer's ego that benefits here - the trade off with these wordy error messages
is that they take up a lot of screen real estate, and make for really huge popups when
you hover over the red squigglies in your editor. If the above messages were shortened to
something like this:

```
unbound type variable
Line 42, Column 1
The `HeadingStyles` type alias uses an unbound type variable `bool` in its definition:

42| type alias HeadingStyles =
43|>    { bold : bool
44|     , italic : Bool
45|     , underline : Bool
46|     }
```

```
name clash
Line 23, Column 12
This file defines multiple `Heading` type constructors. One here:

19|     = Heading (Heading char)
          ^^^^^^^
And another one here:

23| type alias Heading char =
               ^^^^^^^
```

```
unfinished type alias
Line 14, Column 29
I am partway through parsing a type alias, and I expected a type here:

14| type alias TextObject char =
                                ^
```

Then more errors could fit on the output console, and it would be more efficient to
scan through them and fix the problems.

In the end, I think accessible design is important, and designing for the lowest common denominator
is a good place to start (just as mobile-first design with web page layout is a good approach).
However, I think there is room to improve on this, and I think that giving developers the option
to tailor compiler output to their preferred level of detail and instruction would make a
great compiler something truly exceptional.
