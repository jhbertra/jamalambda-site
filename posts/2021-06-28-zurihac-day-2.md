---
title: ZuriHac 2021 Day 2 Recap
author: jamalambda
---

I'll confess - I vastly overestimated the amount of time I'd have this weekend to write. Not only did it take me until the end of day 2 to get my recap of the first day out - it's now three days after the conference ended at the time of writing, and I still haven't released day 2! I'm still determined to get the posts out as promised, but I've resigned myself to the fact that they will be posted late and slightly irrelevant by the time they are. The lesson I'm taking from this is to limit my conference recaps, if I do them at all, to a single post covering the whole conference and not a daily write-up.

Having said that, let's get on with the show!

## Ollie Charles: Rel8, a new database access library for Haskell
Unfortunately, I have nothing to report on the talk given by Ollie Charles because it took place at 4 AM my time. I also haven't been able to go back and watch it yet, but I am intrigued by rel8 and looking forward to hearing the talk when I have the time.

## Fretlink: Fun Haskell for regular stuff, Q&A
Fretlink immediately stood out by introducing themselves as a "non-financial, non-crypto currency, non-compiler company." Their talk focused on using Haskell and PureScript to build a "boring" B2B software system. Fretlink provides tools and software in the ground transportation domain. They use domain-driven design and microservices to develop and organize their backend.

This stands out because domain-driven design and domain modelling are excellent applications for statically-typed functional programming languages underrepresented in the Haskell ecosystem. Thanks to its ties to the .NET ecosystem and the contributions of thought leaders such as Scott Wlaschin and Mark Seeman, F# has much more recognition in the industry for its use in this so-called line-of-business software. One of the most notable books on domain modelling in functional languages, Domain Modelling Made Functional by Scott Wlaschin, makes a compelling case for F# as a premium choice for domain-driven design. But Haskell is at least as well-suited for this task, if not more so. Because Haskell is a pure language and F# is not, ports and adapters is the default architecture, thanks to its strict segregation of pure and impure logic. It also features a more powerful type system, making it possible to assert business rules at the type level in a way that simply cannot be done in F#. While Haskell is used successfully for these applications, other uses tend to overshadow it in the community spotlight. So hearing from a company that uses Haskell for "boring" business applications at a major Haskell conference is extremely interesting!

A large portion of their presentation was devoted to their migration from NodeJS to Haskell, a topic dear to the hearts of many an aspiring Haskell developer. While I would love to reveal the secret of how you too can convince your company to start using Haskell, dear reader, I am sorry to report that they started with an advantage that you most likely don't: their CTO was the one who wanted to do it. Nevertheless, their story did offer insight regarding how to pull the transition off successfully. Because they already used microservices, their first few Haskell trials could happen in relatively isolated contexts. Their first production Haskell project was writing a drop-in replacement for an existing NodeJS service. This was followed by a brand new service for their carrier catalog.

Next was a corroborating account of many common rebuttals to frequently raised concerns regarding adopting Haskell:
How will this impact recruiting? It became easier because they were able to easily attract qualified and motivated applicants.
How will we manage the learning curve? By investing time upfront in standardizing internal practices and building out a standard tech stack. Knowledge will be shared via pair programming and peer-to-peer training. Meanwhile, it is not risky to put a junior Haskell developer to work in a Haskell codebase because solid types and pure functions limit the damage they can cause by accident. People can start being productive sooner and with less supervision than in Ruby, JavaScript, Java, or even F#.
How will this impact maintenance costs? By reducing them dramatically. The difference between the NodeJS and Haskell services was night and day. Haskell services had orders of magnitude fewer bugs to fix and took less time to build. Engineers could confidently refactor Haskell services with gleeful abandon but were terrified to even touch the NodeJS ones.
How is this choice applicable to our business? I already covered this topic, but an intriguing addition is that algebraic data types and even simple functions read like documentation. Project managers and even top management could read the code and provide feedback on the implementation after minimal training and revealed holes and oversights in the original specifications.
They concluded with a description of their current tech stack, which includes Haskell, PureScrupt, and Nix, and some of the high-level choices and practices they have established, such as avoiding large frameworks and favouring a collection of small libraries and internal shared helpers.

The last tidbit I want to include here is an answer to the question: "I want to use Haskell at work, but many of my colleagues object to it or have no interest in it. How can I convince them to change their minds?" The trick is to avoid this by introducing Haskell in a new project. This is where it pays to be a CTO in making this decision - because CTOs can hire new staff who already know Haskell to work on the project. Once Haskell is established in the organization, it becomes easier to convince people of its value because they can see first-hand evidence. Job satisfaction is also contagious, and when people see others enjoying their work, they are likely to become interested in joining in on the fun.

## Tweag: Building Haskell with Bazel
Tweag is a company that should need no introduction. They are famous in the Haskell community for their work on several high-profile open-source projects, including GHC and Nix, and employing many top-level Haskell engineers. Their presentation all about using Bazel for building polyglot projects.

Bazel is a build tool created by Google with the tagline "{Fast, Correct} - Choose two." It features precise, deterministic dependency graphs, cached build and test runs parallel execution, incremental builds, support for multiple platforms, and the ability to specify cross-language dependencies.

Tweag has contributed a significant amount of open-source work to the Bazel ecosystem and is recognized as a Bazel community expert by Google. For example, they built the rules_haskell and rules_nixpkgs bundles for Bazel, along with rules for standard UNIX commands.

The majority of the presentation consisted of a practical demonstration of Bazel. The presenter started with a TODO MVC codebase written in Haskell and Elm, with elm-bridge and servant-elm used to generate Elm bindings to the Servant API. Since it was a practical demonstration, and the repo was hosted on Github, I decided to follow along at home. What followed was some of the most awkward and frantic coding I have ever done.

I have a secret to confess. I am uniquely skilled at causing build failures. I follow instructions to the letter and inevitably end up scratching my head and staring at a wall of missing dependencies or inexplicable build failures. My best guess is that there is something pathological about how I configure and administer my development environments because I always end up causing other people to say, "well, that's new!"

So my attempt to "follow along" really turned into me trying desperately to keep up with the presenter's typing and periodically trying to build the project, leading inevitably to failure. To my credit, the contest was unfair. There were long lists of dependencies that the presenter has already prepared elsewhere that he could copy over to keep the presentation moving along. I'm sure it made the presentation go over more smoothly, but it made following along next to impossible, and I gave up trying after the second time this happened. Nevertheless, I put in a valiant effort, and now I have a half-complete servant-elm-example repo on my hard drive that doesn't build with either Stack or Bazel.

My overall impression of Bazel was really positive. It's been on my radar for a little while, so I'm glad I got such a good demonstration. Google built it, so naturally, it is ridiculously well polished and has terrific documentation. What really struck me is that nothing about it seems revolutionary. I still can't quite put my finger on what makes it different from any other build tool. It feels entirely natural to specify build rules and cross-language dependencies with it. It doesn't feel like a radical reimagining of what a build tool can be. Instead, it feels like what all build tools have tried, and until now, failed to be. Needless to say, I will be investigating Bazel more in the future. Thanks for the excellent demo Tweag!

## Advanced Track: Optics in the Abstract
This was the first of the masterclass-style sessions of the weekend. There were 5 tracks this year - one beginner, one intermediate, one advanced, and two technology-specific ones: Unison and IHP. Because many of the tracks overlapped, I had to choose between a few options. Luckily, the first such choice was easy for me: Advanced track versus Beginner Track. The first session of the beginner track was an "install party." While I love the idea and think it was a vital session to hold for absolute beginners, attending it would have been objectively pointless for me.

The first advanced track session was hosted by Adam Gundry and moderated by Andres L??h, both of Well-Typed. The topic of the session, as you might have guessed from the section heading, was optics. Specifically, it was an introductory session on optics through the lens (pun intended) of the `optics` library maintained by Well-Typed.

> Editor's note (editor = future Jamie): Do we really need to go into all this detail here past me? We do, don't we? Alright, fine, but I'm going to make it easy for people to skip this part.

-- BEGIN COMPLETELY UNNECESSARILY LONG DIGRESSION --

Of course, many people who have encountered optics before will be familiar with the venerable `lens` library developed by Edward Kmett, category theorist extraordinaire. The `lens` library is notable for its direct use of Van Laarhoven encoding. Van Laarhoven encoding represents lenses, folds, prisms, traversals, and other types of optics with function signatures. A typical example of this would be:

```haskell
type Lens s t a b = forall f. Functor f => (a -> f b) -> s -> f t
```

If you are squinting and scratching your head at this, I don't blame you at all. Van Laarhoven optics are notoriously tricky to grok. How does a function like this represent a lens? And also, what are lenses?!? What the heck am I blabbing about?!

At the risk of descending so far into digresception that we enter limbo and are unable to ever return (in case you are wondering, we're still one level away from that, we've just entered the snow fortress), I'll briefly explain lenses and optics in a nutshell. They're what property accessors would have been in object-oriented languages if they had been designed by a bunch of category theory nerds. I suppose that isn't a very helpful description though, let me try again.

Optics are a family of tools used to create composable pathways into deeply nested immutable data structures. Optics provide an abstract notion of "focusing" on parts of a data structure. Different types of optics focus on things in different ways. For example, a lens always focuses on one - and only one - piece of data in a data structure. An example is focusing on a single field in a record. In this way, lenses are the closest analogy to property accessors on objects in object-oriented languages. Folds are read-only optics that focus on zero or more focuses, and traversals are folds that support reading and writing. Prisms are a bit strange in that they focus on a single constructor in a sum type. For example, `Left_` is a prism that focuses the `Left` case of an `Either` and `Right_` focuses the `Right` case. In other words, a Prism may focus on zero or one target.

You might notice that I've been rather vague about what it means for an optic to "focus" on some data. That's because optics don't do anything by themselves. All they do is describe how to access some data inside a data structure. So if you want to use an optic, you need 3 ingredients: a data structure to focus on, the right optic to find the focus you are interested in and an operation to manipulate it. For example, given a student enrollment record (data structure), focus on their student ID number with a lens (optic), and set it to 0000000000000 (operation). The code for this would look like this:

```haskell
set                  -- operation
    studentId        -- optic
    "0000000000000"  -- parameter for the operation
    enrollmentRecord -- data structure
```

The thing that really makes optics tick, though, is that they compose. For example, suppose you wanted to steal the IDs from a list of student records. In that case, if you can find a traversal that focuses all elements in a list and put it on your functional crafting bench, you can compose it with the lens you already have that focuses on the student ID of a student enrollment record. Presto! You've just crafted a new traversal that focuses the student ID of every student enrollment record in a list of student enrollment records! It just so happens that I have our missing piece lying around: it's called `traversed`. In Haskell:

```haskell
view                        -- operation
    (traversed . studentId) -- optic
    enrollmentRecord        -- data structure
```

This is a mere drop in the ocean of what is possible with optics. They are an incredibly vast field of study, and the power they offer is unrivalled by nearly any other FP abstraction in the hands of an experienced practitioner.

The astute reader may have noticed that we used the function composition `(.)` operator to compose the optics. When I say optics compose, I mean it quite literally! Now is the perfect opportunity to resurface into layer 2 of this bizarre fever dream (I'm sorry if I'm just confusing you with the Inception references - but _come on_. That movie was so popular that if you don't get it, you've only got yourself to blame). The fact that optic composition is the same thing as function composition is a direct consequence of the Van Laarhoven encoding. By some insane miracle, Van Laarhoven encoding turns out to produce a whole family of abstractions that interoperate perfectly like this. The types of optics even form a consistent hierarchy - notice how when we composed a traversal and a lens, the result was a traversal? That's because all lenses are valid traversals - they focus on exactly one location, and support get and set operations. That's nothing but a special case of focusing on zero or more locations, which is what a traversal does. The types confirm our intuition:

```haskell
type Lens s t a b = forall f. Functor f => (a -> f b) -> s -> f t
type Traversal s t a b = forall f. Applicative f => (a -> f b) -> s -> f t
(.) :: (b -> c) -> (a -> b) -> a -> c

-- therefore, given
myLens :: Lens s t a b -- or forall f. Functor f => (a -> f b) -> (s -> f t)
myTraversal :: Traversal x y s t -- or forall f. Applicative f => (s -> f t) -> (x -> f y)

-- we get:
myTraversal . myLens :: forall f. (Applicative f, Functor f) => (a -> f b) -> (x -> f y)

-- eliminating the redundant `Functor f` constraint:
myTraversal . myLens :: forall f. Applicative f => (a -> f b) -> x -> f y

-- replacing the definition of Traversal s t a b with its synonym:
myTraversal . myLens :: Traversal x y a b
```

The type class constraints even conform to our intuitional hierarchy: all lenses are valid traversals, but not all traversals are valid lenses. Analogously, An `Applicative` instance implies a `Functor` instance, but not the other way around. So you can use a Lens anywhere you would use a Traversal because the constraint `Functor f` is satisfied by the constraint `Applicative f`. However, you cannot use a Traversal in place of a Lens because the constraint `Applicative f` is not satisfied by the constraint `Functor f`. If you do not know what `Functor` and `Applicative` mean, then I'm afraid I've probably lost you long ago. Unfortunately, we honestly may never conclude with this blog post if I dive into that rabbit hole now. I might cover those topics on another day. But I'll probably save myself the effort and direct you to the avalanche of existing blog posts that explain the Functor, Applicative, Monad hierarchy (oops, I said the M word...). All optics look like this, and their type class constraints perfectly match what intuition tells us should happen.

The cherry on top is that function composition works in reverse with optics - it reads left-to-right instead of right-to-left. So it even looks like the classic dot-access notation we know and love from our favourite OO languages. This fact is further abused by the infix operator for `view`, which could reasonably be defined like this (it's not, by the way, it's a bit more complicated than this):

```haskell
(^.) :: s -> Lens s t a b -> a
(^.) = flip view
```

Which lets you pretend you're writing Java again if you omit the optional whitespace around the operators:

```haskell
print $ company^.officers.ceo.bankingDetails.accountNumber
```

Cute.

 The point is: it's completely ridiculous and kind of spooky that this works at all. You might be tempted to say that this is evidence of a divine will made manifest by the works of some really dedicated nerds. Wrong. It's all just a crazy stack of coincidences that is way more internally consistent than it has any right to be.

There is a big downside to VL encoding and defining everything with type synonyms the way `lens` does, however. It exposes the implementation details in the API. This prevents the library authors from ever being able to refactor the internals without breaking the API (though one could argue that since their API is "mathematically perfect," there would be no need to do this). It does mean that you can write a lens that will integrate with the `lens` library without ever depending on it, though, which is kind of cool. This is apparently something Ed Kmett's libraries are famous for. Unfortunately, it also places the burden of understanding the types on the library consumer, which is the bigger problem. The type errors that appear when you get your optic composition wrong and the types don't align are notoriously cryptic. We are finally ready to resurface into the first dream layer and talk about the `optics` package again.

-- END COMPLETELY UNNECESSARILY LONG DIGRESSION --

`optics` solves this problem by using `newtype` wrappers. Kind of boring, huh? What's interesting is that they use a single `newtype` wrapper called `Optic`. This type uses phantom type signatures of a specific kind to distinguish optic types. A collection of type classes with hand-written instances represents the relationships between different optics. This might seem rather mundane, even unsatisfyingly contrived when compared to the beautifully constructed insanity of `lens`. Yet, it works exactly the same way, and the type errors are mercifully readable in comparison. Of course, there is a cost: you lose the ability to compose optics with the dot operator. You need to use a custom operator - `(%)` instead. To give a hoot about being approachable, we've had to sacrifice the ability to be cute. Darn.

Anyway, there's really not much more to say about the session after that rambly dive into optics. You can watch the entire 3-hour session on YouTube here if you want https://www.youtube.com/watch?v=AGjTAurqQoY. However, I will say that I was slightly disappointed that the session didn't include practical examples or exercises to follow along with. Instead, it was a 3-hour long slideshow. The content was well-presented, and it is a privilege to learn from Adam Gundry, but the session was decidedly more introductory than what I was hoping for. Nevertheless, if this was your first dive to optics, then you probably got an excellent foundation from it. To be fair, the presenters did give a disclaimer at the start that the "advanced" label was perhaps a bit generous for this session.

The part I found the most interesting was the alternative API the `optics` library offers compared to `lens` and the different design choices that led to its creation. After that, it wasn't long before two of the community's most celebrated mainstays took the stage.

## GHC Panel Discussion with Simon Peyton Jones and Ben Gamari
I will admit that my attention span started to fade a bit at this point in the day. I had been up since 5:15 AM and was feeling a little loopy. I honestly don't remember anything covered in this session because I got too distracted chatting with people I had met on Discord. My main memory of this was realizing I had not paid attention for a few minutes and rewinding the stream to catch up on what I had missed. I played the stream back at 1.5X speed to eventually catch up with the live content. However, this turned out to be a mistake since I became even more distracted by the glorious experience of listening to Simon Peyton Jones talk at 1.5 times his usual speed, resulting in me just missing the content again. I guess I'll just need to rewatch this talk, just like all the other ones. Thus concludes my top-quality, highly professional recap of the GHC panel discussion at Zurihac 2021. Moving on...

## Emily Pillmore: Haskell Foundation Progress Update
The next talk was a progress update on Haskell Foundation projects with Emily Pillmore. For those not in the know, the Haskell Foundation is a (very) recently founded organization with a mandate to advocate for and drive the adoption of Haskell in industry and provide support and guidance for the subcommunities in the Haskell ecosystem. They launched their operations in full in February 2021, which makes them only 4 months old! Emily Pillmore is their CTO.

They have been a busy 4 months, though. Their list of projects is lengthy and ambitious, and they are churning through them at a pretty steady pace. They have received a respectable amount of support so far, both in volunteer effort and funding. It also appears that they have a lot more loaded in the barrel to execute on, so additional support means more good things for the Haskell ecosystem faster!

That being said, they have already completed the majority of their initial roadmap, which they thought would fill two quarters!  Some notable projects include: UTF-8 encoded `Text` by default, working with the GHCup and Stack teams to provide a unified installer experience, Helping fund the CI development for GHC, project Matchmaker (more below), and a book on performance tuning in Haskell. Project Matchmaker is an initiative to match developers interested in contributing to OSS projects with projects looking for help.

Their roadmap going forward includes some exciting projects as well. They want a GHC API, a boon tooling development (HLS would benefit dramatically from this, for instance). They also want to bring program coverage (HPC) back to life and up to date. A community-led Haskell book and improved profiler tooling were also on the list, as were improved compiler error messages. Now, I don't want to make any assumptions, but Emily did explicitly mention the idea of [error messages with a configurable level of human readability](/posts/2021-06-13-elm-errors.html). So I have to wonder, does she read my blog? At a guess, definitely.

The presentation was an insightful peek into the current state of projects in the Haskell Foundation and an exciting glimpse of things to come.

## Unison Workshop
This was one of my most anticipated events of the conference. Unison is a new programming language that carries a lot of influence from Haskell. However, it is a radical departure from what most people would recognize as a programming language. What makes it stand out? It fundamentally re-envisions the concept of a codebase.

Codebases have generally been thought of as directories containing text files for decades. The tools employ to manage these file directories have evolved over the years - package managers to pull upstream dependencies and keep them up-to-date, version control systems to control changes to the codebase - but fundamentally nothing different. Unison completely upends all of this by redefining codebase to mean a structured repository of compiled artifacts stored in a queryable storage layer (currently, SQLite).

As a result, you do not track Unison source code files in git and build them into executables. Instead, you draft your definitions in what are called _scratch files_, and add them to the codebase using an interactive command-line tool called the Unison Codebase Manager (UCM). UCM watches for changes in `.u` files in the current directory and compiles them as the files change. When it detects new definitions to be added to the codebase, it asks you to confirm the addition. By confirming, you "commit" your function, data type, documentation (a first-class language citizen, I might add), or test declaration to the codebase. After that, you can delete it from your scratch file; you don't need to keep the source code around anymore!

Once a definition is in the codebase, it's there permanently. The codebase is append-only. You can, of course, change a definition, but doing so doesn't get rid of the old one. Instead, it creates a new definition and updates the name to point there instead. It can do this because everything in Unison is content-addressed. Named are transient and point to unique hashes determined by the content of the type or function. This means if you have two functions with the same body:

```
foo : Text -> Text
foo t = "foo" ++ t ++ "bar"

bar : Text -> Text
bar t = "foo" ++ t ++ "bar"
```

Then Unison treats them as the same function. They will both be identified by the same hash, and the names "foo" and "bar" will be aliases for them. This approach is very reminiscent of how Nix and Git identify artifacts and has some interesting properties, such as trivial renaming.

Unison is still a growing language. It is far from production-ready (currently, you can only execute Unison programs inside UCM). Still, it is fast-growing and has an incredibly passionate and productive community (join the Slack if you're interested - it's a great time). There is also no shortage of ideas on where the language can go from here. One of the current focuses is on developing a distributed computing library to leverage the codebase design. A cluster of connected nodes could run a program collaboratively. If a node is delegated to run a function and lacks a required definition, it can request it from a sibling node. This level of granularity can also extend to pulling dependencies. Instead of pulling compiled library files, you could choose to pull only a single definition from an upstream codebase and use it in your application. This whole concept is so radical that it essentially obsoletes entire families of tooling - package managers, version control, build pipelines, CI servers. It's all just Unison.

The session its self was a hands-on exploration of the language and its runtime. We wrote a simple yet instructive library to compute the Jaro-Wrinkeler distance between two strings (a fuzzy-search algorithm). This included interacting with UCM and writing code, tests, and documentation.

That was the last session of day 2. I said at the beginning that it was three days after the conference when I wrote this. It has now been over a week since the conference ended, and I am failing miserably at getting these out on time. I can't promise day 3 will be out any time before next week, and by the time it does, it will probably be so irrelevant that it's questionable whether it is even worth it. But I made a commitment (and, foolishly, a public announcement) that I would cover each day, and darn it, I'm going to see it through!
