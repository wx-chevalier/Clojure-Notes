### Rich Already Answered That!

A list of commonly asked questions, design decisions, reasons why Clojure is the way it is as they were answered directly by Rich (even when from many years ago, those answers are pretty much valid today!).
Feel free to point friends and colleagues here next time they ask (again). Answers are pasted verbatim (I've made small adjustments for readibility, but never changed a sentence) from mailing lists, articles, chats.

How to use:

- The link in the table of content jumps at the copy of the answer on this page.
- The link on the answer itself points back at the original post.

## Table of Content

### FP vs OOP

- [What is the meaning of Data?](#what-is-the-meaning-of-data)
- [Should I use a `namespace` to bundle functions to model a domain object like a "customer" or "product"?](#should-i-use-a-namespace-to-bundle-functions-to-model-a-domain-object-like-a-customer-or-product)
- [Why Clojure does not implement something like CLOS?](#why-clojure-does-not-implement-something-like-clos)
- [How is Clojure polymorphism different from Java or other languages?](#how-is-clojure-polymorphism-different-from-java-or-other-languages)
- [How Clojure compares to Scala?](#how-clojure-compares-to-scala)

### Clojure Design

- [Why no pattern matching?](#why-no-pattern-matching)
- [Why Clojure does not allow to introduce new syntax like Common Lisp?](#why-clojure-does-not-allow-to-introduce-new-syntax-like-common-lisp)
- [Why did you choose to make your language a Lisp dynamically typed as opposed to ML?](#why-did-you-choose-to-make-your-language-a-lisp-dynamically-typed-as-opposed-to-ml)
- [Why no tail call optimization?](#why-no-tail-call-optimization)
- [Are there cons cells in Clojure?](#are-there-cons-cells-in-clojure)
- [Aren't chunked sequences completely subverting the point of laziness?](#arent-chunked-sequences-completely-subverting-the-point-of-laziness)
- [Why Clojure doesn't have a generic `insert`, `lookup`, `append` that works the same on all collections?](#why-clojure-doesnt-have-a-generic-insert-lookup-append-that-works-the-same-on-all-collections)
- [Why Clojure compiler is single pass? Aren't many possible optimizations lost this way?](#why-clojure-compiler-is-single-pass-arent-many-possible-optimizations-lost-this-way)

### Clojure Features

- [Table of Content](#table-of-content)
  - [FP vs OOP](#fp-vs-oop)
  - [Clojure Design](#clojure-design)
  - [Clojure Features](#clojure-features)
  - [Learning, Process and Community](#learning-process-and-community)
- [Answers](#answers)
    - [What is the meaning of data?](#what-is-the-meaning-of-data)
    - [Should I use a `namespace` to bundle functions to model a domain object like a "customer" or "product"?](#should-i-use-a-namespace-to-bundle-functions-to-model-a-domain-object-like-a-customer-or-product)
    - [How Clojure compares to Scala?](#how-clojure-compares-to-scala)
    - [Why no pattern matching?](#why-no-pattern-matching)
    - [Why Clojure does not allow to introduce new syntax like Common Lisp?](#why-clojure-does-not-allow-to-introduce-new-syntax-like-common-lisp)
    - [Why no tail call optimization?](#why-no-tail-call-optimization)
    - [Why you don't accept pull requests?](#why-you-dont-accept-pull-requests)
    - [Are there cons cells in Clojure?](#are-there-cons-cells-in-clojure)
    - [Why Clojure does not implement something like CLOS?](#why-clojure-does-not-implement-something-like-clos)
    - [Isn't the behavior of the STM unpredictable compared to manual locking?](#isnt-the-behavior-of-the-stm-unpredictable-compared-to-manual-locking)
    - [deftype, gen-class, proxy, reify, defprotocol: why there are so many ways to define a type in Clojure?](#deftype-gen-class-proxy-reify-defprotocol-why-there-are-so-many-ways-to-define-a-type-in-clojure)
    - [How does Clojure solve the potential ambiguity of multiple inheritance?](#how-does-clojure-solve-the-potential-ambiguity-of-multiple-inheritance)
    - [Couldn't locals be mutable?](#couldnt-locals-be-mutable)
    - [When should I use atoms/refs/agents?](#when-should-i-use-atomsrefsagents)
    - [Why there is a "rest" and a "next"?](#why-there-is-a-rest-and-a-next)
    - [What is the difference between collections and sequences?](#what-is-the-difference-between-collections-and-sequences)
    - [Why lazy sequences are cached?](#why-lazy-sequences-are-cached)
    - [Aren't chunked sequences completely subverting the point of laziness?](#arent-chunked-sequences-completely-subverting-the-point-of-laziness)
    - [Why cannot "last" be fast on vector?](#why-cannot-last-be-fast-on-vector)
    - [Why Clojure is not licensed under GPL, MIT, etc?](#why-clojure-is-not-licensed-under-gpl-mit-etc)
    - [I want to study SICP and use Clojure instead of Scheme. Is that a good idea?](#i-want-to-study-sicp-and-use-clojure-instead-of-scheme-is-that-a-good-idea)
    - [Why did you choose to make your language a Lisp (dynamically typed) as opposed to ML?](#why-did-you-choose-to-make-your-language-a-lisp-dynamically-typed-as-opposed-to-ml)
    - [What lessons did you learn porting some data structures from Okasaki ML into Java?](#what-lessons-did-you-learn-porting-some-data-structures-from-okasaki-ml-into-java)
    - [Shouldn't `(nth nil 1)` throw outofbound exception?](#shouldnt-nth-nil-1-throw-outofbound-exception)
    - [Why Clojure doesn't have a generic `insert`, `lookup`, `append` that works the same on all collections?](#why-clojure-doesnt-have-a-generic-insert-lookup-append-that-works-the-same-on-all-collections)
    - [Why Clojure compiler is single pass? Aren't many possible optimizations lost this way?](#why-clojure-compiler-is-single-pass-arent-many-possible-optimizations-lost-this-way)
    - [What is the convention for arguments order?](#what-is-the-convention-for-arguments-order)
    - [Why doesn't contains? do what I expect on vectors and lists?](#why-doesnt-contains-do-what-i-expect-on-vectors-and-lists)
    - [Why commas are allowed as whitespaces?](#why-commas-are-allowed-as-whitespaces)
    - [How Clojure transients work?](#how-clojure-transients-work)
    - [How is Clojure polymorphism different from Java or other languages?](#how-is-clojure-polymorphism-different-from-java-or-other-languages)
    - [How can I achieve programming mastery?](#how-can-i-achieve-programming-mastery)

### Learning, Process and Community

- [How can I achieve programming mastery?](#how-can-i-achieve-programming-mastery)
- [Why you don't accept pull requests?](#why-you-dont-accept-pull-requests)
- [Why Clojure is not licensed under GPL, MIT, etc?](#why-clojure-is-not-licensed-under-gpl-mit-etc)
- [I want to study SICP and use Clojure instead of Scheme. Is that a good idea?](#i-want-to-study-sicp-and-use-clojure-instead-of-scheme-is-that-a-good-idea)
- [What lessons did you learn porting some data structures from Okasaki ML into Java?](#what-lessons-did-you-learn-porting-some-data-structures-from-okasaki-ml-into-java)

## Answers

#### [What is the meaning of data?](https://news.ycombinator.com/item?id=11962116)

The defining aspect of data is that it reflects a recording of some facts/observations of the universe at some point in time (this is what 'data' means, and meant long before programmers existed and started applying it to any random updatable bits they put on disk). A second critical aspect of data is that it doesn't and can't do anything, i.e. have effects. A third aspect is that it does not change. That static nature is essential, and what makes data a "good idea", where a "good idea" is an abstraction that correlates with reality - people record observations and those recordings (of the past) are data. Other than in this conversation apparently, if you say you have some data, I know what you mean (some recorded observations). Interpretation of those observations is completely orthogonal.

Nothing about the idea of 'data' implies a lack of formatting/labeling/use of common language to convey the facts/observations, in fact it requires it. Data is not merely a signal and that is why we have two different ideas/words. '42' is not, itself, a fact (datum). What constitutes minimal sufficiency of 'data' is a useful and interesting question. E.g. should data always incorporate time, what are the tradeoffs of labeling being in- or out-of-band, per datom or dataset, how to handle provenance etc. That has nothing to do with data as an idea and everything to do with representing data well.

But equating any such labeling with more general interpretation is a mistake. For instance, putting facts behind a dynamic interpreter (one that could answer the same question differently at different times, mix facts with opinions/derivations or have effects) certainly exceeds (and breaks) the idea of data. Which is precisely why we need the idea of data, so we can differentiate and talk about when that is and is not happening - am I dealing with facts, an immutable observation of the past ("the king is dead") or just a temporary (derived) opinions ("there may be a revolt"). Consider the difference between a calculation involving (several times) a fact (date-of-birth) vs a live-updated derivation (age). The latter can produce results that don't add up. 'date-of-birth' is data and 'age' (unless temporally-qualified, 'as-of') is not.

When interacting with an ambassador one may or may not get the facts, and may get different answers at different times. And one must always fear that some question you ask will start a war. Science couldn't have happened if consuming and reasoning about data had that irreproducibility and risk.

'Data' is not a universal idea, i.e. a single primordial idea that encompasses all things. But the idea that dynamic objects/ambassadors (whatever their other utility) can substitute for facts (data) is a bad idea (does not correspond to reality). Facts are things that have happened, and things that have happened have happened (are not opinions), cannot change and cannot introduce new effects. Data/facts are not in any way dynamic (they are accreting, that's all). Sometimes we want the facts, and other times we want someone to discuss them with. That's why there is more than one good idea.

Data is as bad an idea as numbers, facts and record keeping. These are all great ideas that can be realized more or less well. I would certainly agree that data (the maintenance of facts) has been bungled badly in programming thus far, and lay no small part of the blame on object- and place-oriented programming.

#### [Should I use a `namespace` to bundle functions to model a domain object like a "customer" or "product"?](https://groups.google.com/d/msg/clojure/Pk-3z8RYsx4/OJtKaEQrAnkJ)

There's a presumption built-in to that question that I think needs to be addressed first. That is, that there should be specialized, named, data structures/classes corresponding to imagined information-based entities in a system, and a partitioning of functions by their association with same entities, either in classes or modules. I've come to the conclusion after 20 years of C++/Java/C#/CLOS/OODBMS that this is a bad idea.

OO is not bad when there is an actual entity - i.e. a stream, a socket, a window, etc to which an object corresponds, or in a simulation of actual entities. That's where it was born and where it shines. When applied to the information/data in a system, however, it causes code to be 10x as large and 1/10th as useful.

For instance, don't customers buy products? Which should own the functions that involve both? Do databases have different functions for different tables? One would hope not. And if a DB query returns a recordset with fields from more than one table, should a custom entity be defined that maps to it? No - ORM is an even worse idea.

Data is data, and having very few general data structures to hold it and very many general functions to manipulate it is the key. That is the Clojure philosophy. (I'm not claiming that this is unique to Clojure, btw)

Look at the way `clojure.xml` processes XML - it deals with an external entity, a SAX parser fronting some XML source, gets the data out of it and into a map, and it's done. There aren't and won't be many more functions specific to XML. The world of functions for manipulating maps will now apply. And if it becomes necessary to have more XQuery-like capabilities in order to handle XML applications, they will be built for maps generally, and be available and reusable on all maps. Contrast this with your typical XML DOM, a huge collection of functions useful nowhere else.

Similarly, `resultset-seq` pulls the data out of a db query and into a sequence of maps right away, where it can be manipulated as generally as possible. That isn't to say you can't or shouldn't tag your data with some information/metadata helping you discern its source, use, or categorization in one or more contexts. But you should avoid the ghettoization of your logic, lest you have to repeat it endlessly.

That's a long way to say you might have to think a bit differently about your problem in Clojure in order to achieve the desired result of writing less, more powerful, code.

#### [How Clojure compares to Scala?](https://groups.google.com/d/msg/clojure/dnfXbbee9qE/zJRpoXQpkU8J)

I think Scala is interesting work, and I haven't spent much time with
it, so I'll just speak for Clojure, other than to say I've seen enough
to know that Clojure is substantially less complex than Scala.

Clojure is predicated on my personal opinion, after many years of
doing it, that imperative programming is not a very good idea in most
situations. So, I consider it an attribute, and not a restriction of
Clojure that it makes 'doing the right thing' (FP) the easy and
default way. Even without concurrency in the picture, I think it
yields better programs, and in the face of concurrency it is, IMO, the
only way to go. You do have an option for imperative programming in
Clojure - calling Java. But imperative programming is not something I
want to promote, and Java is perfectly adequate at it, so Clojure adds
no imperative constructs of its own.

However, Clojure acknowledges the need for the illusion of change in
most real programs that are not merely calculations. That is, multiple
parts of a program may want to associate an identity with state that,
at different times in the program, may be different. It takes the
necessary step, as does Erlang, of making sure that change happens
only inside a language construct that has enforced concurrency
semantics. Again, this is not restrictive, this is enabling, because
with unconstrained mutation the correctness of a concurrent program is
the onus of the programmer.

Someone would choose Clojure who valued its simplicity, extensibility,
dynamism and holistic functional-programming/persistent-data-
structures/concurrency model, and who understood, or sought to attain,
the power offered by Lisp.

If that's not you, that's ok. Neither Clojure nor Scala is better than
the other - they're different.

#### [Why no pattern matching?](https://groups.google.com/d/msg/clojure/CqV6smX3R0o/3Xs_OEuHAQ0J)

If you are talking about the aspect of pattern matching that acts as a
conditional based upon structure, I'm not a big fan. I feel about them
the way I do about switch statements - they're brittle and
inextensible. I find the extensive use of them in some functional
languages an indication of the lack of polymorphism in those
languages. Some simple uses are fine - i.e. empty list vs not, but
Clojure's if handles that directly. More complex pattern matches end
up being switches on type, with the following negatives:

- They are closed, enumerated in a single construct.
- They are frequently replicated.
- The binding aspect is used to rename structure components because the language throws the names away, presuming the types are enough. Again, redundantly, all over the app, with plenty of opportunity for error.

Polymorphic functions and maps are, IMO, preferable because:

- They are open, no single co-located enumeration of possibilities.
- There is no master switch, each 'type' (really situation, with Clojure's multimethods) owner takes care of themselves.
- Maps allow for structures with named components where the names don't get lost.

I'd much rather use `(defrecord Color [r g b a])` and end up with maps
with named parts than do color of `real*real*real*real`, and have to
rename the parts in patterns matches everywhere (was it rgba or argb?)

I'm not saying pattern matching is wrong or has no utility, just that
I would hope it would be less needed in idiomatic Clojure code.
Certainly I'd want the binding part of pattern matching independent of
the conditional part, and I would make any destructuring-bind for
Clojure work with all of the data structures, not just lists, i.e.
vectors and maps and metadata.

#### [Why Clojure does not allow to introduce new syntax like Common Lisp?](https://groups.google.com/d/msg/clojure/uUiuQm5bDwU/6N2Po-OAypAJ)

People are going to put their Clojure programs in text files they expect to be read by the standard reader and evaluated, and it is a design goal of Clojure that programs by different people be able to be combined. User-defined reader macros don't compose, they clash.

You could make a version of Clojure with a modified reader - the reader source is there. But the programs you wrote to your version of the reader would be incompatible with those written by someone else who did the same thing. So, it has less to do with technical or licensing issues than it has to do with it being a bad idea, if you don't want to encourage programmers to create islands. CL has this feature and it is very rarely used for that exact reason - programs that require custom reader macros are a pain.

People have mentioned wanting to use the reader to read programs in other languages or data formats, and while I'm not generally opposed
to the idea of a data reader whose output is not destined for the compiler, I'm also skeptical of the generality of such a thing. A lisp-
style reader is a page or two of Clojure code. People are making parser combinator libs etc. There are ways to solve this problem without making the standard reader fungible, with the added complexity of providing an API and semantics for reader macros, and permanently leaving characters available for customization that that entails.

If you want to modify the reader to create syntax for yourself, convince me and the community that the syntax has wide utility and I'll consider putting it in the standard reader so everyone can use it portably.

#### [Why no tail call optimization?](https://groups.google.com/d/msg/clojure/4bSdsbperNE/tXdcmbiv4g0J)

When speaking about general TCO, we are not just talking about
recursive self-calls, but also tail calls to other functions. Full TCO
in the latter case is not possible on the JVM at present whilst
preserving Java calling conventions (i.e without interpreting or
inserting a trampoline etc).

While making self tail-calls into jumps would be easy (after all,
that's what recur does), doing so implicitly would create the wrong
expectations for those coming from, e.g. Scheme, which has full TCO.
So, instead we have an explicit recur construct.

Essentially it boils down to the difference between a mere
optimization and a semantic promise. Until I can make it a promise,
I'd rather not have partial TCO.

Some people even prefer 'recur' to the redundant restatement of the
function name. In addition, recur can enforce tail-call position.

Full TCO optimizes all calls in the tail position,
whether to the same function or another. No language that uses the JVM
stack and calling convention (e.g. Clojure and Scala) can do TCO since
it would require either stack manipulation capabilities not offered in
the bytecode spec or direct support in the JVM. The latter is the best
hope for functional languages on the JVM, but I'm not sure is a
priority for Sun as tail-calls are not idiomatic for JRuby/Jython/
Groovy.

IMO, either a language has full TCO or it simply doesn't support using
function calls for control flow, and shouldn't use the term. I didn't
implement self-tail-call optimizations because I think, for people who
rely on TCO, having some calls be optimized and some not is more a
recipe for error than a feature.

So, Clojure has recur, which clearly labels the only situations that
won't grow the stack, apologizes for no real TCO, doesn't pretend to
support function calls for control flow, and anxiously awaits TCO in
the JVM, at which point it will fully support it.

In addition to recur, there are lazy sequences and lazy-cons.
To the extent you can structure your processing using the lazy
sequence model, including making your own calls to lazy cons, you can
write traditional-looking functional and recursive solutions that not
only don't grow the stack, but also don't create fully-realized result
lists on the heap, a terrific efficiency boost. I am not contending
that this is as general as TCO, but it has other nice properties, like
the ability to make results that are not tail calls (e.g. list
builders) space efficient.

#### [Why you don't accept pull requests?](https://groups.google.com/d/msg/clojure/jWMaop_eVaQ/3M4gddaXDZoJ)

I prefer patches. I understand some people don't. Can't we just agree to disagree? Why must this be repeatedly gone over?

Here's how I see it. I've spent at least 100,000x as much time on Clojure as you will in the difference between producing a patch vs a pull request. The command is:

    git format-patch master --stdout > your-patch-file.diff

There are two sides to change management - production/submission, and management/assessment/application/other-stewardship. People who argue that the process should be optimized for ease of submission are simply wrong. What if I said patches were twice as efficient for me to assess and manage as pull requests? (it's more than that) Do the math and figure out how the effort should best be distributed.

I don't think asking for patches is asking too much, and I truly appreciate the people who are going to the extra effort. And, I respect the fact that people disagree, and they might decide not to participate as a result. However, I don't need to justify my decision over and over. How about a little consideration for me, and the other list participants? There is a real diluting effect of get-it-off-my-chest messages such as these on the value of the list and the utility of reading it for myself and others.

Sometimes it's better to save it for your bartender :)

#### [Are there cons cells in Clojure?](https://groups.google.com/d/msg/clojure/YY14rlMBuZk/5W43wvTEWqwJ)

There certainly is a cons cell, it's called `Cons`, and `cons` produces one. It differs from CL cons cell in not being an arbitrary pair (i.e. the rest must be a sequence or nil).

Because Clojure supports a seq abstraction, there is heterogeneity in the implementations. In fact there are two cons-cell-like data structures:

```clojure
user=> (class (cons 4 '(1 2 3)))
clojure.lang.Cons
user=> (counted? (cons 4 '(1 2 3)))
false
```

```clojure
user=> (class (conj '(1 2 3) 4))
clojure.lang.PersistentList
user=> (counted? (conj '(1 2 3) 4))
true
```

So, `list?` is more specific because seqs are more general. `seq?` is probably what is desired. `cons` onto an `IPersistentList` could be made to return another `IPersistentList`, but currently doesn't.

#### [Why Clojure does not implement something like CLOS?](https://groups.google.com/d/msg/clojure/forsorhucag/t_RV5WhmKHIJ)

The reason Clojure doesn't have an elaborate system like CLOS, or even like Python et al is that all of those systems have limits and quirks and hardwiring one into a language is a bad idea, IMO. Clojure's hierarchies and multimethods are just libraries - there's no reason you couldn't define a CLOS-like object system for Clojure (although any such system will have problems integrating Java's type hierarchy because there is no definitive superclass ordering). I'm just trying to tease out the useful ingredients of OO and make them available a la carte.

Two things contribute to the sometimes-convenient default resolution policy of CLOS:

One is left-to-right argument order precedence, which roughly translates to vector precedence in Clojure (although the mapping to a vector need not follow arg order). The other is default precedence of classes based upon left-to-right declaration in defclass, and topological sort based thereupon.

Right now, both matching and precedence follow isa?, but the latter need not, or could be supplemented. Here are the characteristics I think are important:

- It should be possible to superimpose a parent on an existing child without modifying the child. This is a critical feature of Clojure's a la carte hierarchies and one which I have desired in every OO language (including CLOS) I have ever worked in. Requiring a client of (independently developed) A and B to have to modify A in order to have it work with B is unworkable.

- It should work transparently with Java hierarchies, which also implies the above.

- It should support independent and order-independent assertions about the hierarchy. Anything based upon canonic master definitions is brittle and inextensible.

What I like about preference declarations is that you are acknowledging a definite ambiguity in a pure derivation hierarchy
rather than poisoning the notion of hierarchy with directionality.

That said, perhaps the preference specifications at the method level aren't as reusable/convenient as preferences declared at another level or somewhere else.

I do care about making this easier to use, but arguments from familiarity are insufficient - we have to acknowledge the tradeoffs involved. I am not willing to repeat this behavior of CLOS for the sake of convenience:

```clojure
(defclass a () ())
(defclass b () ())
(defclass c (a b) ())
(defclass d (b a) ())
(defclass e (c d) ())

(make-instance 'e)

Error: Error during finalization of class #<STANDARD-CLASS E
216B409B>: Cannot compute class precedence list for class: #<STANDARD-
CLASS E 216B409B>
```

Yuck.

#### [Isn't the behavior of the STM unpredictable compared to manual locking?](https://groups.google.com/d/msg/clojure/XHqWLMcsH-c/sXwYNZ6LoQMJ)

One could as generically argue that systems with manual locking are
usually broken, and therefore their behavior itself, never mind their
performance, is unpredictable. That's been my experience with manual
locking in the hands of mere mortal developers. It can be as difficult
to understand the behavior of any single manually-concurrent program
as to understand the performance characteristics of an STM. In the
latter case, at least the STM is handling correctness for you, and all
users of the same STM can share knowledge (and bug fixes and
performance improvements), vs each application being its own Gordian
knot. And in any case in which the STM is failing you performance-
wise, you can opt out and use manual locks outside of transactions. To
the extent you can use it, STM can drastically reduce the complexity
of a system.

I'm wary of unifying the discussion of concurrency with one about
performance, as the problems of concurrency start with correctness,
where the manual locking story, due to its complexity, is quite bad.
Scalability is not a universal problem - some systems need to handle
thousands of simultaneous connections - most don't, some need to
leverage hundreds of CPUs in a single application - most don't. But
every application that wants to leverage multiple cores needs to be
correct.

It would be no problem to instrument the Clojure STM references with
fault counters that would allow one to know the exact per-reference
contention level. Once known, the answers in both cases are similar -
share less mutable state, don't have both long and short-lived
transactions contend for the same data, check your success conditions
ASAP etc.

STM designs differ from one another quite a bit, so any general
statements are difficult to assess. I think the level of granularity
of the STM comes into play. Most of the STM papers I've read
concentrate on creating lots of transactional cells with which they
envision constructing concurrent data structures like the ones in
java.util.concurrent. I don't think that's a good idea at all.

Clojure encourages the use of immutable persistent data structures,
which need no locking or coordination whatsoever in order to perform
correctly in a concurrent situation, and STM references to support the
appearance of identity-preserving change. It also encourages the use
of java.util.concurrent and the like for caches and work queues - the
best tools for the job.

As far as I know, Clojure is the first STM that uses snapshot MVCC,
avoiding both read logging and transaction restarts due to read
invalidations. Clojure's STM also gives you the ability to 'ensure' a
reference you later intend to read or write, thus providing some
manual control over resource acquisition order (rather than just
relying on the side effect of access). It also supports explicit
commute, further reducing retries for commutative writes. I'll not
claim its performance characteristics are at all proven, but its
contention due to, and for, reading is lower than in programs that use
manual locking, and lower than in STMs in which write transactions can
cause read transactions to retry.

Once you get to record-level STM granularity, like Clojure's, it's
also a bit easier to draw parallels to the performance characteristics
of the database systems it mimics, which have been doing similar
things for decades.

I don't consider STM performance any more or less of a research
problem than determining the correctness of programs that do manual
locking - there's still work to do.

And of course, neither STM, nor any other mechanism, is a panacea.

#### [deftype, gen-class, proxy, reify, defprotocol: why there are so many ways to define a type in Clojure?](https://groups.google.com/d/msg/clojure/pZFl8gj1lMs/qVfIjQ4jDDMJ)

A big part of the design thinking behind these features went like this.

Clojure is built on a set of abstractions, and leverages/requires that the host platform provide some sort of high-performance polymorphism construct in order to make that viable. That said, Clojure was bootstrapped on the host language and didn't really provide similar constructs itself (multimethods are more powerful but slower), leaving people that wanted to do things similar to what I did, in order to write Clojure and its data structures, to either write Java or use Clojure interop to, effectively, write Java in Clojure clothing.

So I took a step back and said, what part of Java did I _need_ in order to implement Clojure and its data structures, what could I do without, and what semantics was I willing to support - for Clojure - i.e. not in terms of interop. What I ended up with was - a high- performance way to define and implement interfaces. What I explicitly left out was - concrete derivation and implementation inheritance.

`reify` is Clojure semantics and `proxy` is Java/host semantics. Why doesn't it replace `proxy`? Because `proxy` can derive from concrete classes with constructors that take arguments. Supporting that actually brings in a ton of semantics from Java, things I don't want in Clojure's semantics. `reify` should be possible and portable in any port of Clojure, `proxy` may not. Will the performance improvements of `reify` make it into `proxy`? Probably at some point, not a priority now.

**Prefer `reify` to `proxy` unless some interop API forces you to use `proxy`. You shouldn't be creating things in Clojure that would require you to use `proxy`.**

`defstruct` is likely to be completely replaced by `deftype`, and at some point could be deprecated/removed.

**Prefer `deftype` to `defstruct`, unconditionally.**

AOT `deftype` vs `gen-class` touches on the same Clojure semantics vs Java/ host semantics, with the objectives from before - support implementing interfaces but not concrete derivation. So, no concrete base classes, no super calls, self-ctor calls, statics, methods not implementing interface methods etc. Will the performance improvements of `deftype` make it into `gen-class`? Probably at some point, not a priority now. Like `proxy`, `gen-class` will remain as an interop feature.

**Prefer `deftype` to `gen-class` unless some interop API forces you to use gen-class.**

There will be a `definterface` similar to and probably replacing `gen-interface`, with an API to match `deftype`.

So, with `definterface`, `deftype`, and `reify` you have a very clean way to specify and implement a subset of the Java/C# polymorphism model, that subset which I find clean and reasonable, with an expectation of portability, and performance exactly equivalent to the same features on the host. I could have stopped there, and almost did. But there are three aspects of that polymorphism model that aren't sufficient for Clojure:

1. It is insufficiently dynamic. There is a static component - named interfaces, that must be AOT compiled.
2. Client code must use the interop style `(.method x)`, and type hints, in order to tap into the performance.
3. It is 'closed' polymorphism, i.e. the set of things a type can do is fixed at the definition time of the type. This results in the 'expression problem', in this case the inability to extend types with new capabilities/functions.

We've all experienced the expression problem - sometimes you simply can't request/require that some type implement `YourInterface` in order to play nicely with your design. You can see this in Clojure's implementation as well - `RT.count/seq/get` etc all try to use Clojure's abstraction interface first, but then have hand-written clauses for types (e.g. String) that couldn't be retrofitted with the interface.

`multimethod`, OTOH, don't suffer from this problem. But it is difficult to get something as generic as Clojure's multimethods to compete with interface dispatch in Java. Also, multimethods are kind of atomic, often you need a set of them to completely specify an abstraction. Finally, multimethods are a good story for the Clojure side of an abstraction, but should you define a valuable abstraction and useful code in Clojure and want to enable extension or interoperation from Java or other JVM langs, what's the recipe?

`defprotocol` takes a subset of multimethod power, open extension, combine it with a fixed, but extremely common, dispatch mechanism (single dispatch on 'type' of first arg), allow a set of functions constituting an abstraction to be named, specified, and implemented as group, and provide a clear way to extend the protocol using ordinary capabilities of the host `(:on interface)`.

**Prefer using protocols to specify your abstractions, vs interfaces.**

This will give you open extension and a dynamic system. You can always make your protocol reach any type, and, you can always make your protocol extensible through an interface using :on interface. In particular note, calls to a protocol fn to an instance of the :on interface go straight through, and are as fast as calls using `(.method #^AnInterface x)`, so there is no up-front performance compromise in choosing protocols.

The possibility of extending from abstract classes in Java was an important consideration in the deftype/protocol design. One reasonable argument for concrete implementation is abstract superclasses, especially when used correctly. And Clojure's implementation does use them. Some of the problems with abstract classes are:

- They create a hierarchical type relationship (for no good reason).
- Unless you are going to open that huge can of worms that is multiple concrete inheritance, you only get a single inheritable implementation.
- They, too, are closed. If you are going to allow open extension, but implementation reuse requires derivation, there is an open/closed mismatch.

Protocols are designed to support hierarchy-free, open, multiple, mechanical mixins. This is enabled by the fact that `extend` is an ordinary function, and the mappings of names to implementation functions are ordinary maps. One can create mixins by simply making maps of names to functions. And one can use mixins in an ad hoc manner, merging and replacing functions using ordinary map manipulation:

```clojure
(extend ::MyType AProtocol
  (assoc a-mixin-map :a-fn-to-replace a-replacement-fn))
```

I think people will find this quite powerful and programmable.

#### [How does Clojure solve the potential ambiguity of multiple inheritance?](https://groups.google.com/g/clojure-dev/c/-zoLA78--Mo/m/Y5tQewxpvS0J)

The key question is, what should be the basis of choosing one [strategy] over the
other? Clearly, the ambiguity is inherent. Java allows multiple
inheritance of interfaces, and by hanging implementations off of
interfaces one gets multiple inheritance of implementation. It could
be resolved:

a) as an error - MI is bad, no MI for you

b) mechanically - I don't see a mechanical solution significantly more
meaningful than alphabetical order of classname, given the information
we have available, and it is unlikely to please anyone

c) using external information

Multimethods encountered the same problem and chose (c), using the
prefer system. While that usually enables you to proceed, proceed in
what is a good question. MI is inherently difficult to reason about.
Adding preferences ad hoc and dynamically begets a race to determine
what to prefer, and there is no good way to arbitrate conflicting
preferences. In addition, they are not at all declarative, so one has
to do dynamic querying to see what the relationships are (at the
moment). Finally, moving
forward there will be fewer interfaces and more protocols, so there
won't necessarily be interfaces to hang things from. As we move away
from concrete static type hierarchy, to what should we attach any
categorization/disambiguation system (and can it be made fast)?

It seems weird to complain about having to connect the abstraction
(protocol) to a concrete class, when that is exactly what the author
of the concrete class had to do in choosing its interfaces (once and
for all). In one sense, all protocols do is open up that capability.
Defining concrete behavior for abstractions themselves (rather than
their concrete instances) is somewhat broken from a logic standpoint.
I see the appeal (define something once to cover an open set of
cases), but when combined with interface MI, the logic problem
remains. Preferences just patch up the logic by inserting sequentially
considered conditionals.

```
A is true of Xs
B is true of Ys
Fred is an X and a Y, but only A true of Fred.
```

In short, I think preferences are an ok idea, very much tied to
interfaces, and I'd like to see something better for protocols. Until
then, I've been punting and waiting to see what people really need.

#### [Couldn't locals be mutable?](https://groups.google.com/d/msg/clojure/PCKzXweeDeY/H3y3SjML_JUJ)

One of the primary objectives of Clojure is that it makes it straightforward to build programs that behave well in the context of multiple threads. That means that any mutation facilities must be thread aware, as are Clojure's Vars, Refs and Agents. If you use these constructs (and Clojure's persistent data structures) and avoid Java side-effects, you can write robust multi-threaded programs easily.

If locals were variables, i.e. mutable, then closures could close over mutable state, and, given that closures can escape (without some extra prohibition on same), the result would be thread-unsafe. And people would certainly do so, e.g. closure-based pseudo-objects. The result would be a huge hole in Clojure's approach.

Without mutable locals, people are forced to use recur, a functional looping construct. While this may seem strange at first, it is just as succinct as loops with mutation, and the resulting patterns can be reused elsewhere in Clojure, i.e. recur, reduce, alter, commute etc are all (logically) very similar. Even though I could detect and prevent mutating closures from escaping, I decided to keep it this way for consistency. Even in the smallest context, non-mutating loops are easier to understand and debug than mutating ones. In any case, Vars are available for use when appropriate.

I would hope that programmers without prior functional programming experience would grow accustomed to Clojure's functional style, and, even if they never do multithreaded programming, would benefit from the more robust and easier-to-understand programs it yields.

#### [When should I use atoms/refs/agents?](https://groups.google.com/d/msg/clojure/U5uWf6r4SmU/M-k4cutxA_0J)

To address the general question about when to use `atoms/refs/agents`, it helps to think of things this way.

First, note that in talking about `atoms/refs/agents` we are talking about Clojure's reference types that allow changes to be seen by multiple threads, so these three reference types are all shared reference types.

There are two dimensions to the choice about using them, the first is - will the changes be synchronous or asynchronous, and the second is, will a change to this reference ever need to be coordinated with a change to another reference or references.

For coordinated changes, `ref` + transactions are the only game in town, and asynchrony (beyond a set of commutes) doesn't
make much sense. For independent change, you have two choices, `agent` and `atom`.

Atoms are synchronous, the change happens on the calling thread. They are as close to a plain variable as you get from Clojure, with a critical benefit - they are thread safe, in particular, they are not subject to read-modify-write race conditions. Your writes don't happen unless they are a function of what was read. But modifications to atoms are side effects, and thus need to be avoided in transactions.

Agents are asynchronous, and that can have important benefits. In particular, it means actions get queued, and the sender can proceed immediately. They provide a transparent interface to the threading system and thread pools. Agents also cooperate with transactions in ways that atoms cannot - e.g. agent sends are allowed in transactions and get held until commit.

What's nice is the unified model underlying the reference types. All can be read via `deref/@`, all are designed to refer to an immutable data value, and to model change as a function of that value. All support validators.

What that means is that, if you build your state transformation functions as pure functions, you can freely choose/switch between the different reference types, even using the same logic for two different reference types.

However, they are different, and they have not been unified in the areas in which they differ, in particular, they each have a unique modification vocabulary - `ref-set/alter/commute/send/send-off/swap!/compare-and-set!`.

In the end, Clojure is a tool, and will never be able to make architectural decisions for you. Hopefully the above will help you make informed choices.

The memoization example is a prime motivating case for atoms - a local cache. It's also one that people routinely get multithread-wrong when trying to implement with simple mutable variables.

#### [Why there is a "rest" and a "next"?](https://groups.google.com/d/msg/clojure/gWvXoHa7-t4/HjdiQu3dqbwJ)

It can be very difficult to enumerate (or even remember :) all of the contending tradeoffs around something like Clojure's nil handling.

The is no doubt nil punning is a form of complecting. But you don't completely remove all issues merely by using empty collections and empty?, you need something like Maybe and then things get really gross (IMO, for a concise dynamic language).

I like nil punning, and find it to be a great source of generalization and reduction of edge cases overall, while admitting the introduction of edges in specific cases. I am with Tim in preferring CL's approach over Scheme's, and will admit to personal bias and a certain comfort level with its (albeit small) complexity.

However, it couldn't be retained everywhere. In particular, two things conspire against it. One is laziness. You can't actually return nil on rest without forcing ahead. Clojure old timers will remember when this was different and the problems it caused. I disagree with Mark that this is remains significantly complected, nil is not an empty collection, nil is nothing.

Second, unlike in CL where the only 'type' of empty collection is nil and cons is not polymorphic, in Clojure conj _is_ polymorphic and there can only be one data type created for (conj nil ...), thus we have [], {}, and empty?. Were data structures to collapse to nil on emptying, they could not be refilled and retain type.

At this point, this discussion is academic as nothing could possibly change in this area.

The easiest way to think about is is that nil means nothing, and an empty collection is not nothing. The sequence functions are functions of collection to (possibly lazy) collection, and seq/next is forcing out of laziness. No one is stopping you from using rest and empty?, nor your friend from using next and conditionals. Peace!

#### [What is the difference between collections and sequences?](https://groups.google.com/d/msg/clojure/eWOXIgQhrcU/naz3E0X2kDAJ)

If you look at the docs for Collections you'll see that they all must support 3 functions:

- `count`
- `conj`
- `seq`

and that `seq` returns an `ISeq` instance (Java interface) _on_ the collection. That is the critical point - a seq is a logical sequential _view_ of a collection and not (necessarily) the collection itself.

There are about 20 concrete implementations of the `ISeq` interface in Clojure, supporting sequential access to all of its data structures. One way to think of the seq on a collection is as a cursor or iterator over the collection. The `ISeq` Java interface in turn has 2 _methods_, `first` and `rest`, provided by all implementations.

The Clojure library has 2 _functions_, `first` and `rest`, that are defined on _collections_ and specified to first call seq on their argument. Calling seq on something that is already a seq just returns it.

So `[1 2 3]` is not a seq, nor is `{:a 1 :b 2}`, but `first` and `rest` and most of the sequence library work on them because they first obtain a seq on the collection using the `seq` function. Otherwise it would be quite tedious to have to call seq everywhere. Thus, while they are functions of collections (i.e. they take collections), they operate on
the sequential views of those collections, thus the name sequence functions.

Some collections are their own sequential view, i.e. they implement `ISeq` directly because they inherently have linked structure (e.g. lists):

```clojure
(instance? clojure.lang.ISeq [1 2 3])
-> false
(instance? clojure.lang.ISeq '(1 2 3))
-> true

(.getClass [1 2 3])
-> clojure.lang.PersistentVector

(.getClass (seq [1 2 3]))
-> clojure.lang.PersistentVector$ChunkedSeq

(.getClass '(1 2 3))
-> clojure.lang.PersistentList

(.getClass (seq '(1 2 3)))
-> clojure.lang.PersistentList
```

Of course, it's quite straightforward for ISeqs to behave like collections (i.e. implement `count`, `conj` and `seq`), and so they do, etc - the Clojure library is designed to be maximally polymorphic. In the end, unless you are interested in extending the sequence library in Java, it's not important to know the type hierarchy. Generally, all the sequence functions work on all of the collections. `cons` is an exception because it creates linked structure, and I want to avoid the
accidental creation of linked structure on top of associative structure, whereas all of the other sequence functions are view-only or create independent structure. Not that there's anything wrong with `(cons 1 (seq {:a 1 :b 2 :c 3}))` if that's what the user really intends.

#### [Why lazy sequences are cached?](https://groups.google.com/d/msg/clojure/FfBGPZbY9PA/uRqM7p6PYBAJ)

I think it's very important not to conflate different notions of sequences. Clojure's model a very specific abstraction, the Lisp list, originally implemented as a singly-linked list of cons cells. It is a persistent abstraction, first/second/third/rest etc, it is not a stream, nor an iterator. Lifting that abstraction off of cons cells doesn't change its persistent nature, nor does lazily realizing it. After my experimentation with a non-caching version, I am convinced it is incompatible with the abstraction.

If a seq was originally generated from an imperative source, you need it to be cached in order to get repeatable read persistence, if it was calculated at some expense, you need to cache it in order to get the performance characteristics you would expect from a persistent list. An abstraction is more than just an interface.

Everything with side-effects breaks. Lots of performance problems, clear multiple traversals, but often casual multiple local references to (rest x) and other parts of the seq, causing multiple evaluations and corresponding runtime multipliers. In short, an entirely different set of things you need to be careful of, and far more often.

#### [Aren't chunked sequences completely subverting the point of laziness?](https://www.reddit.com/r/programming/comments/afyav/clojure_11_rc1_out_cuts_some_overhead_of/c0headd/)

Were avoiding some non-terminating/exploding computation the only point of laziness, that might be true. But it's not. Remember, Clojure is primarily a strict/eager language. The only bits of Clojure that are lazy (other than explicit delays) are the sequence functions. This has several implications.

First, there simply aren't many non-terminating computations floating around in Clojure code. Second, there is another benefit to lazy sequences, of primary importance to Clojure: avoiding full realization of interim results. This isn't a simple question of a binary eager/lazy switch, it is rather about the granularity of laziness and sequential processing. Should the default be one item? I am increasingly convinced that the answer for languages like Clojure is no. See for instance this work, which I think is relevant to Clojure: [MonetDB/X100: Hyper-Pipelining Query Execution](http://cidrdb.org/cidr2005/papers/P19.pdf)

I don't disagree about the importance of an alternative providing item-at-a-time processing, and would have been reluctant for this to go forward without it were it not for the fact that the majority of users of Clojure have been using these chunked sequences without problems or complaint for months. I'm simply not done yet with an official API, but have provided an unofficial one for those who've asked.

#### [Why cannot "last" be fast on vector?](https://groups.google.com/d/msg/clojure/apkNXk08Xes/CGCQqLMhlHwJ)

It is quite easy to come up to some part of Clojure, with some need, and some current perspective, and find it unsatisfying. It certainly is not perfect. But a good presumption is that there might be more context, more constraints, or more issues in play than one might recognize at first glance.

Mark was most correct in identifying that the algorithmic complexity dominates this decision. As far as the functions being defined in terms of abstractions: while true, the abstractions are still something that could be refactored. They are a means, not an end.

History: 'last' is a Lisp function. In Common Lisp it operates on lists only. It is a function that, there, advertises slowness by its mere presence. Code using it has a presumption it will only be used on short lists, and in fact it generally is; in macros and things that process code. People reading that code can presume the same.

I do think that, in general, it is bad to have a single polymorphic function that, depending on the target arguments, transitions between different algorithmic complexity categories (N.B. that is not equivalent to using polymorphism to get better performance by leveraging some implementation specifics, it is a specific subset). 'nth' for seqs is a counterexample, was requested by the community, and I still think is somewhat of a mistake. At least, it would be good to also have an 'elt' with specific non-linear complexity (for the reasons I give below).

Usually, there is a 'fast' function and someone wants it to work on something that would be in a slower category. This case is the opposite, 'last' is slow and people want it to be faster where possible. The end result is the same.

Why not just be as polymorphic as possible? In Common Lisp, there is a set of functions that lets you use lists as associative maps. The performance is not good as things get larger (i.e the sky falls). While this made sense at one point in time (when there were only lists), is it still a good idea now? Should we have functions that expect maps accept lists?

In my experience, having everything work on/with lists made it really easy to get a system together that worked on small data sets, only to have it crash and burn on larger ones. And then, making it perform well was extremely challenging. Some of that was because of the lack of interface-conforming implementations of e.g. hashmaps to swap in. but another category of problem was not being able to see what calls mattered for performance, when lists would still be ok etc. Thus Clojure 'forces' you to make some of these decisions earlier, and make them explicit. Even so, it is still highly polymorphic.

At the moment, I'm not sure it would be bad to have 'last' behave better for vectors et al while still promising in documentation to be not necessarily better than linear. But I do know this - it is seriously unimportant. We all have much better things to do, I hope, than argue over things like this.

There is a perfectly adequate and well documented way to get the last element of a vector quickly, and code for which that matters can (and should, _even if last were made faster_) use it. Code for which it doesn't matter can use 'last' on vectors because - ipso facto _it doesn't matter_! People reading either code will know what is and isn't important, and that seems to matter most.

#### [Why Clojure is not licensed under GPL, MIT, etc?](https://groups.google.com/d/msg/clojure/bpnKr88rvt8/AOtLHW4Lhh4J)

MIT and BSD are not reciprocal licenses. I want a reciprocal license.
But I don't want the license to apply to, or dictate anything about,
non-derivative work that is combined with mine, as GPL does. I think
doing so is fundamentally wrong.

The fact that GPL is not compatible with that approach is a problem
with GPL, and for users of GPL software.

I will not be using any 'customized' license. Using a well known
license intact is the only way to make it easy for users to vet the
license for use. Anything else requires lawyers for me and them.

I will not be dual licensing with GPL or LGPL. Both licenses allow the
creation of derived works under GPL, a license I cannot use in my
work. Allowing derived works I cannot use is not reciprocal and make
no sense for me.

#### [I want to study SICP and use Clojure instead of Scheme. Is that a good idea?](https://groups.google.com/d/msg/clojure/jyOuJFukpmE/aZjWHBnsQ74J)

Everyone's experience will be different, of course. Here's my 2 cents:

I don't think SICP is a book about a programming language. It's a book
about programming. It uses Scheme because Scheme is in many ways an
atomic programming language. Lambda calculus \+ TCO for loops \+
Continuations for control abstraction \+ syntactic abstraction (macros)
\+ mutable state for when you need it. It is very small. It is
sufficient.

The book really deals with the issues in programming. Modularity,
abstraction, state, data structures, concurrency etc. It provides
descriptions and toy implementations of generic dispatch, objects,
concurrency, lazy lists, (mutable) data structures, 'tagging' etc,
designed to illuminate the issues.

Clojure is not an atomic programming language. I'm too tired/old/lazy
to program with atoms. Clojure provides production implementations of
generic dispatch, associative maps, metadata, concurrency
infrastructure, persistent data structures, lazy seqs, polymorphic
libraries etc etc. Much better implementations of some of the things
you would be building by following along with SICP are in Clojure
already.

So the value in SICP would be in helping you understand programming
concepts. If you already understand the concepts, Clojure lets you get
on with writing interesting and robust programs much more quickly,
IMO. And I don't think the core of Clojure is appreciably bigger than
Scheme's. What do Schemers think?

I think the Lisps prior to Clojure lead you towards a good path with
functional programming and lists, only to leave you high and dry when
it comes to the suite of data structures you need to write real
programs, such data structures, when provided, being mutable and
imperative. Prior Lisps were also designed before pervasive in-process
concurrency, and before the value of high-performance polymorphic
dispatch (e.g. virtual functions) as library infrastructure was well
understood. Their libraries have decidedly limited polymorphism.

Alas, there is no book on Clojure yet. But, to the extent Schemes go
beyond the standard to provide more complete functionality (as most
do), there are no books on that either. Just docs in both cases.

Learning Scheme or Common Lisp on your way to Clojure is fine. There
will be specifics that don't translate (from Scheme - no TCO, false/
nil/() differences, no continuations; from CL - Lisp-1, symbol/var
dichotomy). But I personally don't think SICP will help you much with
Clojure. YMMV.

#### [Why did you choose to make your language a Lisp (dynamically typed) as opposed to ML?](https://groups.google.com/d/msg/clojure/N8xdXsJplv4/KiECTMeDpWoJ)

I think the ML, and even more so, Haskell, are extremely interesting type systems and languages. But, personally, I haven't found their type systems to be sufficiently expressive or sufficiently dynamic for my needs. I like dynamic polymorphism, I like heterogeneous collections. I'm sure there are probably people working on extensions to GHC to allow for similar things, but I don't have time for that. I don't think the benefits of those type systems outweigh their costs, for me. And, Haskell is already doing a brilliant job in delivering that kind of solution.

OTOH, I think immutability is a huge benefit, so Clojure is an effort to bring that over to a dynamic language. (Erlang is another). I think ML's restriction of mutation to atomic refs (arrays aside) is a good idea and was definitely inspiration for Clojure, which adds transactional semantics.

#### [What lessons did you learn porting some data structures from Okasaki ML into Java?](https://groups.google.com/d/msg/clojure/CqV6smX3R0o/_ZnnimboYjQJ)

I won't argue for the verbosity of Java vs. SML or Haskell, and I readily acknowledge the beauty of the original formulation (although the remove code, not in the book, isn't pretty in any language). But it was an interesting exercise to move code from a pattern-matching language to a non-pattern-matching language. This was my experience:

I started with pretty much a transliteration of the original, replacing datatype tags with enums and matches with conditionals. The result was none too fast. Also, I wanted to optimize the memory use, as I knew many nodes would be non-branching leaves, and often maps would be used as sets, i.e. without values. (Also, at that time I didn't have persistent hash maps, and thought the rb tree would have to serve all the purposes for which hash maps now serve). One could imagine extending the Tree ADT (of the SML version) from E|T to E|T| Leaf|EmptyT|EmptyLeaf or some such, but, without polymorphism, the effect on the pattern matching is a multiplicative growth of combinations.

So, given I was in Rome (Java), I decided to do the idiomatic thing, and pursued a hybrid strategy, maximizing the use of polymorphism, (which I know is optimized in Java), and minimizing the use of conditionals. (The remove code is still pretty much conditionals, as the logic is too impenetrable to safely recast :)

The result is the hierarchy of Nodes, implementing the memory usage strategy, and, for add/lookup at least, mostly polymorphic method dispatch. The result is more efficient and _much_ faster. The logic is distributed - each node type takes care of itself to some degree. It was, IMO, easier to add the with/without value/branches variations this way to maintain a conditional approach, with or without patterns. The result might seem a bit verbose, but comparing it to the Okasaki book example is not apples-to-apples. You would have to have an SML/Haskell version that:

- Was a map, not just a set.
- Had the remove code.
- Had optional values, and used no storage for no value.
- Didn't store branches for leaves.
- Allowed the option of comparing elements _or_ supplying a comparator.

I'm sure all of this is elegantly possible, but it won't be the pretty little thing in the book anymore.
Here are some of my takeaways:

- Adding types can have a multiplicative, rather than an additive, effect on pattern matches.
- Without a pattern-match optimizer, naive mechanical translation of patterns to if-else constructs can produce slow code.

The latter is particularly important. I'm sure SML/Haskell/Erlang optimize pattern matching, and while it may be easy to
superficial behavior with a macro, the optimizers are likely non-trivial.

#### [Shouldn't `(nth nil 1)` throw outofbound exception?](https://groups.google.com/d/msg/clojure/NtrmBtOCVio/yQdtKgXk27oJ)

Thank goodness it doesn't (and first/rest etc) or speculative
destructuring (and many other Clojure idioms) would be impossible.

It is a CL influence to have semantics for many functions when applied
to nil rather than generate errors, and, once one knows the rules, it
can be used to great effect to produce elegant code. I've mentioned this before: http://people.cs.uchicago.edu/~wiseman/humor/large-programs.html

#### [Why Clojure doesn't have a generic `insert`, `lookup`, `append` that works the same on all collections?](https://groups.google.com/d/msg/clojure/bSrSb61u-_8/uAaWlLuEMBYJ)

_Also answers_: why only some functions are more polymorphic (e.g. `into`, `conj`, `count`) while others are more type specific (e.g. `contains?`, `assoc`)?

It is an important aspect of Clojure that, in general, performance guarantees are part of the semantics of functions. In particular, functions are not supported on data structures where they are not performant.

You can't just swap out sequences or lists for maps or sets in a program. So, you need to use the right data structure for the job, and then the right functions will be available. When it makes sense, some functions are maximally polymorphic (e.g. seq, or into). But lookup, under any name, shouldn't be, IMO, so it isn't in Clojure. Similarly there is no insert at the beginning of vectors, append to end of lists, lookup for values of maps, insertion in the middle of sequences etc. I simply don't want to provide the tools for people to write programs in Clojure with N^2 performance - that benefits no one. But I've seen such performance degradation in many a Lisp program as people build upon O(n) functions on lists.

If you have a lookup table in your program, please use a set or map, or, if by index, a vector. You will have get, contains? or nth available, performance will be good, and that will be clear from the code.

seq-contains? exists because sometimes, say in a macro, you have a known short sequence and need to test for the presence of something. There's no need to copy it into a set. And people have been rolling their own includes? etc for this job. Now everyone can use seq- contains? for that. Its poor performance is indicated by its name and documentation. Should someone try to take a piece of code based on seq- contains? and try to scale it, they will see that function as an obvious bottleneck.

Your type-check argument is a straw man that doesn't hold up in Clojure practice. People tend to use the right data structures for the job and don't choose algorithms via type checks.

Clojure is not particularly duck-type-y, in general things are not unified by the presence of same-named functions but by underlying shared abstractions. Just because protocols can be used to make things reach many types doesn't mean they should be used to unify things that aren't uniform.

#### [Why Clojure compiler is single pass? Aren't many possible optimizations lost this way?](https://news.ycombinator.com/item?id=2467809)

The issue is not single-pass vs multi-pass. It is instead, what constitutes a compilation unit, i.e., a pass over what?
Clojure, like many Lisps before it, does not have a strong notion of a compilation unit. Lisps were designed to receive a set of interactions/forms via a REPL, not to compile files/modules/programs etc. This means you can build up a Lisp program interactively in very small pieces, switching between namespaces as you go, etc. It is a very valuable part of the Lisp programming experience. It implies that you can stream fragments of Lisp programs as small as a single form over sockets, and have them be compiled and evaluated as they arrive. It implies that you can define a macro and immediately have the compiler incorporate it in the compilation of the next form, or evaluate some small section of an otherwise broken file. Etc, etc. That "joke from the 1980's" still has legs, and can enable things large-unit/multi-unit compilers cannot. FWIW, Clojure's compiler is two-pass, but the units are tiny (top-level forms).

What Yegge is really asking for is multi-unit (and larger unit) compilation for circular reference, whereby one unit can refer to another, and vice versa, and the compilation of both units will leave hanging some references that can only be resolved after consideration of the other, and tying things together in a subsequent 'pass'. What would constitute such a unit in Clojure? Should Clojure start requiring files and defining semantics for them? (it does not now)
Forward reference need not require multi-pass nor compilation units. Common Lisp allows references to undeclared and undefined things, and generates runtime errors should they not be defined by then. Clojure could have taken the same approach. The tradeoffs with that are as follows:

1. less help at compilation time
2. interning clashes

While #1 is arguably the fundamental dynamic language tradeoff, there is no doubt that this checking is convenient and useful. Clojure supports 'declare' so you are not forced to define your functions in any particular order.

#2 is the devil in the details. Clojure, like Common Lisp, is designed to be compiled, and does not in general look things up by name at runtime. (You can of course design fast languages that look things up, as do good Smalltalk implementations, but remember these languages focus on dealing with dictionary-carrying objects, Lisps do not). So, both Clojure and CL reify names into things whose addresses can be bound in the compiled code (symbols for CL, vars for Clojure). These reified things are 'interned', such that any reference to the same name refers to the same object, and thus compilation can proceed referring to things whose values are not yet defined.

But, what should happen here, when the compiler has never before seen bar?

```clojure
(defn foo [] (bar))
```

or in CL:

```lisp
(defun foo () (bar))
```

CL happily compiles it, and if `bar` is never defined, a runtime error will occur. Ok, but, what reified thing (symbol) did it use for bar during compilation? The symbol it interned when the form was read. So, what happens when you get the runtime error and realize that `bar` is defined in another package you forgot to import. You try to import other-package and, BAM!, another error - conflict, other-package:bar conflicts with read-in-package:bar. Then you go learn about uninterning.

In Clojure, the form doesn't compile, you get a message, and no var is interned for bar. You require other-namespace and continue. I vastly prefer this experience, and so made these tradeoffs. Many other benefits came about from using a non-interning reader, and interning only on definition/declaration. I'm not inclined to give them up, nor the benefits mentioned earlier, in order to support circular reference.

#### [What is the convention for arguments order?](https://groups.google.com/d/msg/clojure/iyyNyWs53dc/Q_8BtjRthqgJ)

One way to think about sequences is that they are read from the left, and fed from the right:

`<- [1 2 3 4]`

Most of the sequence functions consume and produce sequences. So one way to visualize that is as a chain:

`map<- filter<-[1 2 3 4]`

and one way to think about many of the seq functions is that they are parameterized in some way:

`(map f)<-(filter pred)<-[1 2 3 4]`

So, sequence functions take their source(s) last, and any other parameters before them, and partial allows for direct parameterization as above. There is a tradition of this in functional languages and Lisps.

Note that this is not the same as taking the primary operand last. Some sequence functions have more than one source (concat, interleave). When sequence functions are variadic, it is usually in their sources.

I don't think variable arg lists should be a criteria for where the primary operand goes. Yes, they must come last, but as the evolution of assoc/dissoc shows, sometimes variable args are added later.

Ditto partial. Every library eventually ends up with a more order- independent partial binding method. For Clojure, it's #().

What then is the general rule?

Primary collection operands come first.That way one can write -> and its ilk, and their position is independent of whether or not they have variable arity parameters. There is a tradition of this in OO languages and CL (CL's slot-value, aref, elt - in fact the one that trips me up most often in CL is gethash, which is inconsistent with those).

So, in the end there are 2 rules, but it's not a free-for-all. Sequence functions take their sources last and collection functions take their primary operand (collection) first. Not that there aren't are a few kinks here and there that I need to iron out (e.g. set/select).

#### [Why doesn't contains? do what I expect on vectors and lists?](https://en.m.wikibooks.org/wiki/Clojure_Programming/FAQ)

Sequential lookup is not an important operation. Clojure includes sets and maps, and if you are going to be looking things up you should be using them. contains? maps to java.util.Set.contains. The fact that java.util.Collection also has contains is, IMO, a mistake, as you really can't write code to an interface with dramatically varying performance characteristics. So, lookups in sets and maps take priority and get the best name - contains? People write naive programs that do linear lookups with contains() in other languages, and get correspondingly bad n-squared performance - that's not an argument for encouraging that in Clojure. If they get an explanation of why it's a bad idea, and how to use sets and maps, that's fine.

#### [Why commas are allowed as whitespaces?](https://www.reddit.com/r/programming/comments/74tbn/rss_to_twitter_in_clojure_the_good_the_bad_and/c05p20u/?st=jdnpi9h1&sh=6993c19e)

The motivation came from having map literals. I wanted to avoid syntax, e.g. ':'s between key and value or mandatory commas, as well as structure, grouping key-value pairs with parens, for instance. But printing large maps with uniform keys and values with no grouping makes it really easy to lose your place: {1 2 3 2 5 3 7 2...}

Commas as whitespace give you the best of both worlds. You can enter in short maps or maps with distinct keys and values, e.g. {:fred 41 :ethel 42} with no commas, but use commas whenever they help. By default, maps print with commas between key/value pairs: {1 2, 3 2, 5 3, 7 2...}. Because it is not map-specific syntax, you can use them anywhere, making changing a map to a vector a matter of changing the {} to [].

People have started to use them elsewhere, esp. in let bindings, as Clojure doesn't have the traditional parens around each binding, and there too, it is optional, non-structural, a matter of personal preference and need. (let [a 1 b 2]...) might by fine but (let [x a, b y, c z] ...) might be clearer than without commas.

#### [How Clojure transients work?](https://www.reddit.com/r/programming/comments/977mq/think_persistent_data_structures_and_functional/c0bon6t/?st=jdnp8ytz&sh=37c896f9)

Transients are related to doing functional programming in that, if you are otherwise using persistent data structures, conversion to/from transients is `O(1)`. There is no full copy to produce the mutable version (nor the other way), nor is use of the mutable version in any way unsafe for its persistent source. In order to do this, there needs to be a symbiotic design. It is not simply a matter of "we'll let you switch to these other, different, mutable data structures".

Certainly, using transients is not functional programming, nor are they persistent. But they are an important part of the story for those who might otherwise eschew functional programming and immutable data for fear that, should they encounter a performance bottleneck, they will have to change their data structures and the shape of their code.

Clojure's persistent vectors and hashmaps are trees with high branching factors. Creating a transient version means copying the root node into a new object that implements the transient interface, otherwise sharing the rest of the structure. As edits are made, the transient will clone nodes for which it doesn't own a writable copy. Once it has its own copy of a node, subsequent edits affecting that node can be made in place. When done editing, the root is cloned again, this time into an object implementing the persistent interface, and a flag is flipped in the transient disabling further use. Essentially the transient just builds a persistent in place, there is no change tracking or collapsing.
The threading and passing requirement is designed to ensure that the code preserves the same shape as functional code it replaces.

#### [How is Clojure polymorphism different from Java or other languages?](https://groups.google.com/d/msg/clojure/3WS9SRkfgGI/ddJPkHV-0E0J)

Polymorphism in this context means being able to say `(foo x)` or `x.foo()` and have what happens be different depending on some characteristic of `x`. In traditional OO languages (single-dispatch, methods in classes) the only characteristic of x that can be leveraged is its type/class, say X. There is a second, more subtle aspect, which is, which foo are we talking about? In traditional OO languages the call usually takes the second form, as the question is answered by looking up foo in the scope of the class of x. That scope may include superclasses etc, but what is essential is that it constitutes a namespace. So, traditional OO languages unify namespaces and
polymorphism.

In static OO languages (C++/Java et al), the scope is closed on definition of the class, the original author having the final say. No
more methods can be added, and no more names introduced. In dynamic OO languages (Smalltalk, Python, Ruby, et al), there is usually some
means whereby the set of methods in a class scope can be changed or extended without changing the class definition (monkey-patching).
While the first case, changing some base functionality, is inherently fraught with danger, the second, extending, seems desirable and
reasonable, and the rest of this discussion will focus on extension.

So, Fred wants to add `bar()` to X, and uses the monkey-patching facilities of the language to do so. He calls `x.bar()` and it works.
Ethel, working independently, also wants to add a bar method to X, with her own semantics. She can and does, and it works. Ricky uses
both Fred's and Ethel's libraries, creates an X, and calls `x.bar()` - what happens? Nothing good. Could this have been avoided? Perhaps Fred and Ethel could have written independent functions, without injecting them into X, e.g. `fredlib.bar(X)` and `ethellib.bar(X)`? Presumably they didn't because they wanted bar to be polymorphic, i.e. maybe they added bar to classes X, Y, and Z, so they could call `xyorz.bar()` and have the right thing happen depending on the type of `xyorz`. So, the problem with monkey-patching is that it is non-composable, because it forces all extensions to live in a single (class) namespace.

Is there another way? Yes, the designers of CLOS, in their great wisdom, and with a desire to support multiple dispatch, realized the
limitations of having polymorphic functions be in or of a class. They invented generic functions - stand-alone functions that allowed for
extensible polymorphic dispatch through the definition of one or more generic methods of the function, such methods being selected based
upon the type or value of one or more arguments to the function. And they did it in a language, Common Lisp, that had packages to separate
namespaces. Setting aside the very powerful multiple-dispatch capability, this scheme has the advantage of separating namespaces,
class definitions and polymorphism. The result is strictly more powerful and composable. Clojure follows CLOS in having namespaces
(for packages) and multimethods (for generic functions).

So, using Clojure, Fred, who wants a function bar that is polymorphic on the type of its argument, working in his own namespace, defines a
multimethod:

```clojure
(in-ns 'fred)
(clojure/refer 'clojure)
(defmulti bar class)
(defmethod bar String [s] :fred-bar-string)
(defmethod bar Integer [i] :fred-bar-int)
(bar "foo")
-> :fred-bar-string
(bar 2)
-> :fred-bar-int
```

and Ethel does similarly:

```clojure
(in-ns 'ethel)
(clojure/refer 'clojure)
(defmulti bar class)
(defmethod bar String [s] :ethel-bar-string)
(defmethod bar Symbol [s] :ethel-bar-sym)
(bar "foo")
-> :ethel-bar-string
(bar 'foo)
-> :ethel-bar-sym
```

Ricky, wanting to use the libraries of both Fred and Ethel, has many choices re: bar. He may only care about Fred's bar, in which case he
will :exclude bar when he refers to ethel, or, he may use both equally, and not refer to either, preferring to fully qualify each
call as fred/bar and ethel/bar, or he may find those names tedious and :rename them barf and bare. The important thing is that Ricky can
be aware of the semantic differences between fred/bar and ethel/bar, can make choices about which is in use when, and is never precluded by the existence of one from accessing the other. And all of these decisions are completely independent of the polymorphism (or lack
thereof) of bar.

So, with generic functions/multimethods, you don't need to modify someone else's scope in order to provide polymorphic extensions. Thus
'monkey-patching' is not needed to provide the kind of extension in CL or Clojure for which there is no alternative to monkey-patching in
some other dynamic OO languages (short of building extra-lingual CLOS-like dispatching). Generic functions/multimethods in packages/
namespaces are composable, because they allow for independent non-conflicting extension.

#### [How can I achieve programming mastery?](https://disqus.com/home/discussion/jasonrudolph/jasonrudolph_blog_programming_achievements_how_to_level_up_as_a_developer/#comment-287120251)

Mastery comes from a combination of at least several of the following:

- Knowledge
- Focus
- Relentless considered practice over a long period of time
- Detected, recovered-from failures
- Mentorship by an expert
- Always working _slightly_ beyond your comfort/ability zone, pushing it ever forward

Musicians get better by practice and tackling harder and harder pieces, not by switching instruments or genres, nor by learning more and varied easy pieces. Ditto almost every other specialty inhabited by experts or masters. A wide variety of experiences might lead to well-roundedness, but not to greatness, nor even goodness. By constantly switching from one thing to another you are always reaching above your comfort zone, yes, but doing so by resetting your skill and knowledge level to zero.

One can become a great developer in any general purpose language, in any domain, on any platform. And, most notably for the purposes of this discussion, such a developer can carry that greatness across a change in any of them. What skills then are so universally useful and transportable in software development? Two are: the ability to acquire knowledge, and the ability to solve problems.

How does one get better at acquiring knowledge and solving problems? Not by acquiring a lot of superficial knowledge nor solving a lot of trivial problems (a la your 'achievements'), but by acquiring ever deeper knowledge and solving ever harder problems.

You should take heed your phrase 'leveling up'. You don't level up by switching games all the time, but by sticking with one long enough to gain advanced skills. And, you need to be careful to recognize the actual game involved. Programming mastery has little to do with languages, paradigms, platforms, building blocks, open source, conferences etc. These things change all the time and are not fundamental. Knowledge acquisition skills allow you to grok them as needed. I'd take a developer (or even non-developer!) with deep knowledge acquisition and problem solving skills over a programmer with a smorgasbord of shallow experiences any day.

Will this lead to an as-easily-realized improvement strategy based upon boolean achievements? Probably not.
