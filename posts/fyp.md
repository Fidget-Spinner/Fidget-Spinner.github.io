# My Final Year Project / Bachelors' Theses

Today I submitted my final year project.
Fingers crossed I get to graduate!

This was supervised by Manuel Rigger and Stefan Marr. I **have**
to give a shoutout to Michael Droettboom as well for being so generous
with benchmarking help. I have to also thank CF Bolz Tereick for
helping me to better understand PyPy in the process.

Every CPython contributor working with me also deserves thanks. However,
if I list them all here, the list will go on too long. So I'm sorry that
you're not specificially mentioned here!

Read it if you want to learn more about:
* Various JIT compiler architectures
  1. Method
  2. Tracing
  3. Meta-tracing (PyPy)
  4. Meta-interpreters (GraalVM)
  5. Lazy basic block versioning (YJIT)
  6. Tracelets (HHVM)
* Various experiments I tried out in CPython:
  1. A tail-calling interpreter.
  2. Partial evaluation of traces.
  3. Tracing from function entrypoints.
  4. A method JIT (CPython is currently tracing JIT).
  5. A baseline JIT.
  6. Superinstructions

Out of the above, the tail-calling interpreter has been upstreamed.
Tracing from function entrypoints is likely to be upstreamed but not in its
current form.

Read more [here](../resources/FYP%20Final%20Report%20H341100.pdf).

And when I say I hope I graduate ...I really mean it... Design and Analysis
of Algorithms is really tough!

