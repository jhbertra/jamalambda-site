---
title: ZuriHac 2021, Day 1 Recap
author: jamalambda
---

ZuriHac!!!! This year I'm attending ZuriHac, an annual Haskell conference & hackathon hosted by the
[Zürich Friends of Haskell](https://zfoh.ch/). The programming is extremely varried, including talks by notable
community members, masterclass sessions for Haskellers of all levels, presentations by partner companies and
organizations, activities like mob programming, and a focus on hacking / contributing fixes and improvements to open
source projects. This year, like last year, of course, it is not being hosted in Zürich, but online via Discord. Though
the festivities technically started last night with some pre-event socializing, things officially kicked off today.
There was already a significant amount of activity going on before the official kick-off, with people getting started on
projects and chatting online.

First of all, if you're here expecting thoroughly researched, scholarly, or insightful content, you are bound to be
disappointed. These posts are intended to be a (super) casual and brief recap of the event, and may contain gross
oversimplifications or inaccuracies that may be offensive to some readers. Why? Because attending a convention is super
tiring and time consuming, which means very little time or mental energy left over to be extremely thorough or rigorous
and still get these out in a reasonable time frame! As such, I will aim for at least coherent and entertaining!

This is the first Haskell event I've ever attended, and so far I've found it to be a very warm and welcoming community
to join. People are enthusiastic and conversations are intelligent. Each project has its own Discord channel, and I am
pleased to report that the #ghc channel was filled with nothing but top-quality memes about Ben Gamari secretly being
either a C, Bash, LaTeX, Python, or Perl programmer, and GHC being a compiler. Good stuff.

I think I'm going to focus my project work mostly on helping improve [smos](https://smos.online/) and making
contributions to library or compiler code for [Unison](https://www.unisonweb.org/).

It's been a little overwhelming to be honest, there are so many things happening all over the place on discord that it's
hard to know where to look. For someone who hates missing out and likes to keep tabs on as much as possible, it quickly
became apparent that I'd need to moderate my content exposure. I muted the majority of the channels and only kept around
the ones I was primarily interested in. Even then, it was sometimes a struggle to keep up with everything that was going
on!

Things officially kicked off at 6 PM CEST (12 PM EST for me, which is a lovely civilized time). After a short opening
ceremony held by the conference organizers, it was straight into _Testing smart contracts with QuickCheck_ with John
Huges.


## Ladies and Gentlemen, the Fabulous John Hughes

YouTube link [here](https://www.youtube.com/watch?v=n4IgYrc0pes). The very first thing I noticed is that this talk had a
far cooler title slide than most Haskell talks do. I am now convinced that John leads a double life as either an action
film star, or a mafioso. I found the talk extremely interesting, and I might have to take a deeper dive and do a more
detailed writeup about the ideas involved in writing property tests for state machines. However, between the
frenzy of note taking and the constant distraction from Discord, I found myself missing non=insignificant chunks of the
talk, so it's definitely one I'll want to re-watch later on. For now, here are some of the main things I got out of it,
as well as some of my own thoughts on the content.

First of all, I really appreciate that the enormous waste of energy that older blockchain systems like Bitcoin and
Ethereum incur is being talked about more now. It is a big problem. The fact that Bitcoin mining is responsible for
enough CO2 emissions that it would be the world's third largest polluter if it was a nation is, frankly, unacceptable.
Cardano is a relatively new blockchain that hosts the Ada cryptocurrency, and it uses a novel approach that is far more
scalable, and requires orders of magnitude less energy. It is also more or less totally written in Haskell!

Politics of blockchain and cryptocurrency isn't what I want to talk about though. The talk focuses on testing properties
of smart contracts - which are programs that are executed in a blockchain environment automatically, and are the
specification of how to carry out a type of transaction. They are the business logic of blockchain-based systems.
Property based testing involves writing tests that assert invariants of a system, and then generating randomly sampled
data and running it through the test hundreds or thousands of times to verify that the property holds in most (if not
every possible) combination of conditions. It is extremely effective at finding edge cases you may have forgotten to
account for.

There are lots of examples in literature about writing property tests for pure functions. The classic example is testing
the list reverse function. The common example is that `reverse . reverse` should be the same as `id` (that is,
reversing a list and reversing it again is the same as doing nothing. Pure functions are child's play though - they are
definitely the easiest to test in general, but also not terribly interesting. The interesting thing about testing smart
contracts is that they are stateful computations. Their behaviour depends on the current state of the ledger, and their
result is a modification to that ledger. This means you need to create a test driver that can model a state machine and
run it through arbitrary transitions. To do this, you need to be able to generate A) An initial state for the system and
B) a sequence of actions, where each action may depend on the current state. This means that the generation and the
simulation of the state machine need to occur in iteration with one another. In general, the way you can do this in a
Haskell-based framework like QuickCheck is to write a function to model your state machine. This should look very
familiar to anyone with Redux or Elm experience.

```haskell
nextState :: MonadState State m => Action -> m ()
```

and a generator to create a random action:

```haskell
genAction :: State -> Gen Action
```

Which, if you squint, are reciprocal signatures. One takes an action and produces a state (via MonadState),
and the other takes a state and produces an action. So, you can weave them together using monad transformers:

```haskell
genState :: Gen State
genState = do
  initialState <- genInitialState
  numActions <- genNat
  execStateT (replicateM_ numActions $ nextState =<< (lift . genAction) =<< get) initialState
```

~~Unrelated - I need to figure out how to get syntax highlighting for my Markdown.~~ And also that Haskell code is totally
untested, no idea if it works or even compiles...

EDIT: I figured out syntax highlighting!

What are the kinds of things you might want to test in a system like this? Well, you will undoubtedly have invariants
you want to always hold. If you are managing a cryptocurrency for instance, you want to make sure that money goes to the
correct places, and does not disappear or materialize out of nowhere. Actions will likely also have preconditions that
must be met before they can be executed. For instance, that a buyer has sufficient funds in their wallet before they can
complete a transaction. Given a method for generating arbitrary sequences of actions, and an arbitrary initial state, it
is possible to test that these conditions hold in the majority of cases (actually proving this would require a proof
assistant and formal verification, property tests are something of a pragmatic comprimise on this).

I won't go into more detail here - you should watch the talk if you haven't already! There was one more application
which I found really interesting. Say you have a goal in mind - a final state of the system you would like to be able to
reliably reach no matter where you start from. If you can design a strategy for how to get there, then you can test
whether or not that strategy is sound by throwing random inputs at it. If the goal is always reached in every test case
your strategy is sound. Otherwise, it may be impossible, or you've found a bug, and can fix it! I thought this was a
very interesting application of this technique.

It is worth noting that there are libraries published that support testing these kinds of state machines - in
particuilar, for Haskell, [quickcheck-state-machine](https://hackage.haskell.org/package/quickcheck-state-machine) is
a library designed to test stateful programs in this way.

## Digital Asset and DAML

Shortly after, the first industry partner presentation happened. The company presenting was Digital Asset, and their
product is a programming language and runtime environment for private blockchainss called DAML. It is built in Haskell,
and the language its self shares many similarities with Haskell. The immediate impression I got is that this product
looks very smooth to use. They were showing various editor integrations and web apps and CLIs and other developer tools
that seemed very intuitive and powerful. I think the visual and interactive nature of these tools is something that the
Haskell community can learn from and incorporate into our own debugging utilities - granted that DAML is a much more
application-specific language, so there is a smaller set of features and use cases you need to design for.

I also learned that they were the originators of the ghcide project, which has now evolved into the Haskell Language
Server. Digital Asset is a clear example of Haskell being used for something that it is universally recognized at being
excellent for: implementing programming languages. Though the runtime is written in Scala, the compiler is written in
Haskell.

## General Chit-Chat

The official programming ended here. There was plenty of ad hoc discussion going on in the various voice channels on
Discord though, which was all very spirited. Here are some of the highlights:

Lots of math discussion going on in hallway-voice. There was a discussion about slide rules, and why using them made
people stronger mathematicians. There was also a discussion of a Tetris puzzle that Ed Kmett had tweeted about earlier.

Lakeside voice was pretty chill, but there were some interesting discussions about a particular opportunity for
optimizing empty type classes. The question was whether it would be possible to erase empty typeclass constraints when
generating Core, rather than passing an empty dictionary, which is what happens quite often. Unfortunately, this gets in
the way of optimisation, by thwarting attempts to share function parameters, because they get wrapped in a lambda when
desugaring the typeclass constraints. It was the kind of low-level, nitty-gritty discussion with GHC devs that make up
part of the convention experience.

That's it for today's recap - I'll be back with another insightful, informative, thought-provoking recap of tommorrow's
sessions!
