# Reflections on 2 years of CPython's JIT Compiler: The good, the bad, the ugly

5 July 2025

This blog post includes by honest opinions on the CPython JIT. What I think we did well,
what I think we could have done better. I'll also do some brief qualititative
analysis.

I've been working on CPython's JIT compiler since before the very start.
I don't know how long that is at this point ... 2.5, maybe almost 3 years?
Anyways, I'm primarly responsible for Python's JIT compiler's
[optimizer](https://docs.python.org/3.13/whatsnew/3.13.html#an-experimental-just-in-time-jit-compiler).


## Here's a short summary:

The good:
1. I think we're starting to build a community around the JIT, which is great.
2. The JIT is also **teachable**. We have newcomers coming in and contributing.

The bad (could use improvement):
1. Performance
2. Inaccurate coverage of the JIT


## Good: A community-driven JIT

CPython's JIT is community-driven at this point. You may have heard
of the layoffs at Microsoft affecting the Faster CPython team. However,
my underestanding is that from the very start, the JIT was meant to be
a community project.

This wasn't always the case. When the JIT started out, it was practically
only Brandt working on the machine code generator. I had help from Mark (Shannon)
and Guido in landing the initial optimizer, but after that it was mostly me.
Later I got busier with school work and Brandt became the sole contributor to
the optimizer for a a few months or so. Those were dark times.

I'm really happy to say that we have more contributors today though:
* Savannah works on the machine code generator, reproducible JIT stencils, and 
  sometimes the optimizer. 
* Tomáš works on the optimizer and is a codeowner of it!
* Diego works on the machine code generator to improve it on ARM, and sometimes
  the optimizer.
* We also have various non core-dev drive-by contributors. Zheaoli and Noam are names
  that I remember. Though I'm definitely missing a few names here.

This community building was somewhat organic, but also very intentional. We
actively tried to make the JIT easier to work on. If you dig up past discussions,
one of Mark's arguments for a tracing JIT was easier static analysis. This easiness
isn't just that it doesn't require a meet or join in general or that it requires
only a single pass, but more that static anlaysis of a single basic block is
easier to teach than a whole control-flow graph.

We also actively welcome people to work on the JIT with us. CPython doesn't have much
optimizing compiler expertise interested in working on the JIT. We have some compiler
people, but the subset of those interested in working on the JIT is even smaller. So
we aim to train up people even if they don't have any background in compilers.

## Good: A teachable JIT

As I mentioned earlier, tracing was one decision to make the JIT easier to teach.
There are a few other design decisions too, but those will be their own blog post.
So I'm not talking about them here.


## Bad: Performance

CPython 3.13's JIT ranges from slower to the interpreter
to roughly equivalent to the interpreter.
Calling a spade a spade: CPython 3.13's JIT is slow. It hurts me to say this considering
I work on it, but I don't want to sugarcoat my words here.

The argument at the time was that it was a new feature and we needed to lay the foundations
and test the waters. You might think that surely, CPython 3.14's JIT is a lot faster right? Nope.
The answer is again... complicated. When using a modern compiler like Clang 20
to build CPython 3.14, I often found the interpreter outperforms the JIT. The JIT only really starts reaching
parity or outperforming the interpreter if we use an old compiler like GCC 11 to build the interpreter.
However, IMO that's not entirely fair to the interpreter, as we're purposely limiting it by using a compiler
we _know_ is worse for it. You can see this effect very clearly on Thomas Wouter's analysis
[here](https://github.com/Yhg1s/python-benchmarking-public). In short, the JIT is almost always slower
than the interpreter if you use a modern compiler. This also assumes the interpreter doesn't get hit
by random performance bugs on the side (which have happened many times now).

You might ask: why is the 3.14 JIT not much faster? The real answer, which 
again hurts me to say is that the 3.14 JIT has almost no major performance 
features over 3.13. In 3.14, we were mostly expanding the existing types 
analysis pass to cover more bytecodes. We were also using that as a way to 
teach new contributors about the JIT and encourage contribution. In short, we 
were building up new talent. I personally think we were quite low on 
contributors at the start. I also had other commitments which made features 
that were supposed to go into the JIT not go in, which I'm sorry for. 
Personally, I think building up more talent over prioritizing immediate 
performance is the right choice for long-term sustainability of the JIT.

## Bad: Inaccurate coverage

The initial coverage of the JIT got the numbers wrong. There was this number 
of "2-9%" faster than the interpreter being spread around. I think the first 
major blog post that covered this was
[this one](https://tonybaloney.github.io/posts/python-gets-a-jit.html#is-it-faster). Note that I'm friends with the 
author of that post and I'm not trying to say that they did a bad job.
Conveying performance is a really hard job. One that I'm still struggling with 
[myself](./apology-tail-call.md). However, in good conscience, and as an 
aspiring scientist, I can't stand by and watch people say the JIT is "2-9%" 
faster than the intepreter. It's really more nuanced than that (see section 
above). Often times, the CPython 3.13 JIT is a lot slower than the interpreter.

I've seen other sources repeat this number too. It frustrates me a lot. The 
problem with saying the 3.13 JIT is faster is that it sets the wrong 
expectations. Again, users on the Python Discourse forum and privately have 
shared performance numbers where the JIT is a significant regression for them.
This goes against the grain of what's reported online. We do not have control over the numbers, but I still would like to clear the air on what the real expectation should be.

## Conclusion and looking forward

I'm still hopeful for the JIT. As I mentioned above, we've built a significant 
community around it. We're now starting to pick up momentum on issues and new optimizations that could bring single-digit percentage speedups to the JIT in 3.14 (note: this is the geometric mean of our benchmarks, so real speedups might be greater or lesser). Brandt has already merged some [optimizations](https://github.com/python/cpython/pull/135905) for the JIT's machine code. I don't want to bring unwanted attention to the other efforts for the moment. Just know this: there are mulitiple parallel efforts to improve the JIT now that we have a bigger community around it that can enable such work.





