---
title: "Some observations from using Javascript and Typescript day-to-day"
categories: ['javascript', 'typescript', 'software architecture']
---

[Javascript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) has become my everyday programming language at [Shopify](https://shopify.com) and in my open-source projects.
I find it flexible and powerful, thanks partly to its diverse and dynamic ecosystem. Still, I noticed shortcomings that might impact a project's maintainability and scalability. What follows is a set of observations I've made in no particular order. Note that I'm talking about building a raw Node project with no framework.

### Lack of code conventions

The lack of conventions around how the code is organized and architected leads to inconsistencies that make the code hard to navigate.
When few people are contributing to a project, 
it's easy to come up with a set of conventions and make sure they are met through code reviews.
Still, when the team grows,
code reviews no longer work, 
and you need to resort to static analysis tools like [ESLint](https://eslint.org/) or build tools like [Rollup](https://rollupjs.org/).

[Rust](https://www.rust-lang.org/) and its build system, [Cargo](https://doc.rust-lang.org/cargo/), are excellent examples of building conventions into the day-to-day tools.

### Tooling indirection

I've written about this one a few times. Even with ESM, you can't escape tooling when working on a Javascript project. That leads to indirection that makes debugging code a bit more convoluted. It also leads to tooling setups that are not easy to reason about because you end up with implicit dependencies between tools like [Lerna](https://github.com/lerna/lerna), [Yarn](https://yarnpkg.com/), [Nx](https://nx.dev/), Rollup, and Typescript. It's the classic thing that once it's set up by one team member, others depend on them to improve or fix things.

Suppose you come from a compiled language like Swift or Rust. In that case, the additional tooling indirection is something you might be used to. Still, coming from Ruby like it was my case, I find a lot of beauty in throwing the Ruby code at the interpreter as it was written.

### Object-all-the-things

This worked fine. We use Typescript interfaces and types to declare the interface of our objects, and implemented functions that derived state from the objects:

I was recommended to avoid doing [Object Oriented Programming](https://en.wikipedia.org/wiki/Object-oriented_programming) unless strictly necessary, 
so we use Typescript interfaces and types to declare the interface of our objects and implement functions that derive state from the objects:

```ts
function deriveState(project: Project): string {}
```

The problem comes when we keep adding functions [without conventions](#lack-of-code-conventions).
You end up with utility functions scattered throughout the codebase, 
making it hard to look them up and potentially leading to duplications.
A solution to this could be declaring those utility functions in the interface:

```js
type Project = {
  name: string;
  deriveState: () => string
}
```

But that forces you to set a value to `deriveState` every time you create an instance of the object,
which can be very cumbersome when writing unit tests.
Another solution to this could be using union types to bring trait-based polyphormism to Javascript:

```ts
type Project = { name: string; }
type DerivableState = { dervieState: () => string; }
type Buildable = { build: () => Promise<void>; }
```

But that leads to more verbosity in method signatures that expect a project instance to conform to all the interfaces, `Project & DerivableState & Buildable`,
or `BuildableProjectWithDerivableState`.
I've seen many projects solving it with Typescript [utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html), 
but I also find the syntax very verbose.

### Closing thoughts

**Javascript and flexibility go hand in hand**, 
and it's something we can't change. 
However, we can leverage the flexibility of the language, 
runtimes like [Node](https://nodejs.org/en/) and [Deno](https://deno.land/), 
and the community tooling to build more conventional development experiences that help projects thrive.