# I'm Sorry for Python's tail-calling Interpreter's Results

This is my first blog post ever. I want to use it to say
I'm truly sorry for communicating inaccurate results for
Python's tail-calling interpreter. I take full personal 
responsibility for the oversight that led to it.

## What happened?

About a month ago, I [merged][tail-call-pr] a new 
tail-calling interpreter into Python. That interpreter
reported a 9-15% performance boost on Python 3.14's Whats New page.

These figures turned out to be inaccurate. Long story short,
the compiler we were using (Clang 19), had a bug that 
worsened our baseline performance. We (the CPython 
developers) were completely unaware of this bug.

The real performance uplift one can expect by upgrading
to the tail-calling interpreter is between the 3-5% range.
We are not too sure about this figure as well, because we
had to compare across different compilers.

Thanks to Nelson Elhage for their excellent investigation
into this issue and bringing it up. For more information, you can
read their blog post [here][nelson-blog-post].

[tail-call-pr]: https://github.com/python/cpython/pull/128718
[nelson-blog-post]: https://blog.nelhage.com/

## What I'm doing to fix the situation

Upon receiving news from Nelson confirming that the Clang 19 bug caused
a 10% performance regression on our baseline. I did the following:

* Immediately pushed a [PR][whatsnew-tail-call-pr] to [Python 3.14's What's New Page][whatsnew-314] to correct the record. I put a big attention markup in reStructuredText to signal to the reader that a correction has been made. This also gives credit to Nelson.
* Updated all my Reddit posts to add a disclaimer and link to the updated What's New.
* For Twitter/X: I don't have premium so I can't edit my post. I'm thinking of posting a link to this blog post to let people know.

If you feel there's more I could do, please let me know.

[whatsnew-tail-call-pr]: https://github.com/python/cpython/pull/130908
[whatsnew-314]: https://docs.python.org/3.14/whatsnew/3.14.html#whatsnew314-tail-call

## What I've learnt from this

This completely blindsided me and I've learnt to never trust
the compiler when the performance results are too good to be true.
That, and to carefully investigate our baselines.

At the time of writing, the Clang 19 [bug][tailduplicator]
I talked about is not yet fixed, and it exists in Clang 19, 20, maybe 21-beta. **I do not want to blame the LLVM developers for this.** Like me, they are probably volunteer contributors as well.
Sometimes we make mistakes.

[tailduplicator]: https://github.com/llvm/llvm-project/issues/106846

## Summary

In short, a compiler bug in Clang 19 that we were unaware of
resulted in worse baselines. I reported these figures believing 
they were true. I should have done more investigation into the 
compiler before reporting these figures. I'm deeply sorry for
mistakenly reporting inaccurate numbers.