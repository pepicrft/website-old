

---
title: "Type regression testing"
categories: ['typescript', 'development']
---

Deep package graphs and breaking changes wrapped in minor version updates lead to the well-known delete node_modules.
Many developers have accepted it's inherent to the Javascript ecosystem.
I can't.
I care about the developers' experience and I don't think that's a great experience.
We can do better.

Most projects are already using a tool that outputs information about the public interface,
[Typescript](https://www.typescriptlang.org/),
so why not leveraging it to do regression testing against a baseline?
Imagine a PR failing telling that the code changes introduce breaking changes.

Thanks to the Internet,
I learned that there are already people doing some exploration in this domain.
Gianluca Mezzetti, Anders Møller, and Martin Toldam Torp wrote up a paper on the subject:
[Type Regression Testing to Detect Breaking Changes in Node.js Libraries](https://cs.au.dk/~amoeller/papers/noregrets/paper.pdf).

I might give their approach a shot at [Gestalt](https://github.com/gestaltjs/gestalt) to prevent its future users from having to delete node_modules.