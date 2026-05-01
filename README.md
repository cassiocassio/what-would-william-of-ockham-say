# what-would-william-of-ockham-say

A parsimony-review subagent for [Claude Code](https://docs.claude.com/en/docs/claude-code/sub-agents). Catches scope creep, premature abstraction, edge-case coverage that doesn't pay for itself, and bike-shedding. Adjudicates between competing fix proposals when other reviewers pile on.

> Entities should not be multiplied beyond necessity.
> — William of Ockham, ~1320

## Who was William of Ockham?

An English Franciscan friar, ~1287–1347, born in the Surrey village of Ockham. He studied and taught at Oxford, where he made his name in medieval logic by arguing that universal categories — "redness," "humanity," "table-ness" — exist only as names in the mind, not as real things in the world. This position, called **nominalism**, is the philosophical seed of the principle that bears his name: if you can explain something without inventing extra entities, don't invent them.

Ockham's life was not quiet. Summoned to the papal court at Avignon to answer for his theology, he ended up in a public fight with Pope John XXII over whether Jesus and the apostles had owned property. He lost; he fled north; he spent the rest of his life under the protection of the Holy Roman Emperor in Munich, writing political treatises against papal authority. He almost certainly died of the Black Death.

The "razor" — *entia non sunt multiplicanda praeter necessitatem*, "entities should not be multiplied beyond necessity" — is a later formulation of his method, not a literal Ockham quotation. But he wielded it relentlessly: in arguments about God, knowledge, causation, and being. The agent in this repo is named after the man, not the principle, because the *posture* matters as much as the rule. William asked, of every claim that the world contained one more kind of thing: *do we actually need this?*

## What it does

**Mode A — adjudicator (post-consolidation).** Runs after a fan-out review workflow has merged findings from other agents into a single list. William annotates each finding as `real` / `edge` / `speculative`, picks the parsimonious fix when several have been proposed for the same problem, clusters duplicates, and flags Parkinson's-Law-of-Triviality bikeshed crowding. The user reads one annotated report instead of fighting through three.

**Mode B — standalone (session / plan / diff).** Given a session, branch, or PR, William finds the original task, inventories what moved, computes the task-vs-work ratio, names tells from a fixed taxonomy, and proposes the smallest commit split.

It is a **signal, not a gate.** Complexity that earns its keep is fine — William names it as proportionate and moves on. The job is to catch complexity that doesn't earn it.

## Install

Drop the agent file into your Claude Code agents directory:

```sh
curl -L https://raw.githubusercontent.com/cassiocassio/what-would-william-of-ockham-say/main/what-would-william-of-ockham-say.md \
  -o ~/.claude/agents/what-would-william-of-ockham-say.md
```

Or per-project: `<repo>/.claude/agents/what-would-william-of-ockham-say.md`.

## Usage

```
Have what-would-william-of-ockham-say review the current diff.
```

Or, in a fan-out review pipeline, pass the consolidated finding list as input and ask for the Mode A annotated output.

## The inherited heuristics

The agent doesn't make up its rules. It cites people who thought about this longer than anyone in your repo will. Authority matters when pushing back on "excellence."

- **Rule of Three** — Fowler & Roberts, [*Refactoring*](https://refactoring.com/) (1999). Don't abstract until the third occurrence.
- **Simple vs. Easy** — Rich Hickey, [*Simple Made Easy*](https://www.infoq.com/presentations/Simple-Made-Easy/) (2011). Easy ≠ simple; a feature flag is easy and makes the system complex.
- **Hoare's test** — C.A.R. Hoare, [Turing Award lecture](https://dl.acm.org/doi/10.1145/358549.358561) (1980). "Make it so simple there are obviously no deficiencies."
- **The Wrong Abstraction** — Sandi Metz, [*The Wrong Abstraction*](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction) (2016). Duplication is far cheaper than the wrong abstraction.
- **Parkinson's Law of Triviality** — C.N. Parkinson (1957). The bike-shed effect: disproportionate weight on trivial issues because they're accessible.
- **One logical change per patch** — [Linux kernel](https://www.kernel.org/doc/html/latest/process/submitting-patches.html) and [Google CL guidelines](https://google.github.io/eng-practices/review/).
- **Knuth, in full** — Donald Knuth, [*Structured Programming with go to Statements*](https://dl.acm.org/doi/10.1145/356635.356640) (1974). "We should forget about small efficiencies, say about 97% of the time." Perf changes need a measurement, not a hunch.
- **Chesterton's Fence** — G.K. Chesterton, *The Thing* (1929). Don't remove what you don't understand. Knight Capital ($440M in 45 minutes from reactivated dead code) is the empirical version.
- **ETTO / boundary exception** — Erik Hollnagel, [The ETTO Principle](https://erikhollnagel.com/ideas/etto-principle/) (2009). At irreversible system boundaries, parsimony bows to thoroughness. Therac-25 and Ariane 5 are why.
- **Boy Scout in spirit, separate-commit in practice** — Robert Martin's "leave it cleaner" actively fuels the *while I'm here* anti-pattern. Do the cleanup; don't bundle it.
- **Wirth's prior** — Niklaus Wirth, [*A Plea for Lean Software*](https://cr.yp.to/bib/1995/wirth.pdf) (1995). Software expands to fill the available memory. Scope expands to fill the available session.

## The thinking behind it — a reading list

These are the texts the agent leans on, paraphrased in plain English. None of them are required reading — but if you want to argue with William, argue with these first.

- **C.A.R. Hoare, "The Emperor's Old Clothes"** ([Turing Award lecture, 1980](https://dl.acm.org/doi/10.1145/358549.358561)) — Hoare designed the first commercial language with built-in safe arrays and was horrified to watch the industry strip the safety out for speed. The lecture is his case that the discipline rewards complexity over correctness because complexity *looks* like seriousness. Famous line: *"There are two ways of constructing a software design: one way is to make it so simple that there are obviously no deficiencies, and the other way is to make it so complicated that there are no obvious deficiencies. The first method is far more difficult."*
- **Niklaus Wirth, "A Plea for Lean Software"** ([IEEE Computer, 1995](https://cr.yp.to/bib/1995/wirth.pdf)) — The designer of Pascal arguing that software bloat isn't a bug, it's the default. Hardware gets faster; software absorbs the gain by adding features nobody asked for. *Wirth's Law:* "Software is getting slower more rapidly than hardware is getting faster." If you have ever wondered why the modern app feels heavier than the one it replaced, Wirth told you why thirty years ago.
- **Edsger Dijkstra, EWD1036, "On the cruelty of really teaching computing science"** ([1988](https://www.cs.utexas.edu/~EWD/transcriptions/EWD10xx/EWD1036.html)) — A polemic about why programming education refuses to take simplicity seriously. Best one-liner: *"Simplicity is a great virtue but it requires hard work to achieve it and education to appreciate it. And to make matters worse: complexity sells better."* That last clause is why William exists.
- **Rich Hickey, "Simple Made Easy"** ([Strange Loop talk, 2011](https://www.infoq.com/presentations/Simple-Made-Easy/)) — The Clojure author drawing a sharp line between *simple* (an objective property — un-braided, one-fold) and *easy* (a subjective property — familiar, near-to-hand). Most of what we call "easy" tooling adds complexity in exchange for familiarity. Once you can hear the distinction, you can't un-hear it.
- **David Parnas, "On the Criteria To Be Used in Decomposing Systems into Modules"** ([1972](https://dl.acm.org/doi/10.1145/361598.361623)) — The legitimate counter-argument to YAGNI. Parnas says you should design modules around the changes you anticipate, not the data you happen to have. The honest reading is *both* are true: anticipate the changes the domain tells you about (Parnas), don't invent ones it doesn't (Ockham).
- **Frederick P. Brooks, *The Mythical Man-Month*** ([1975](https://en.wikipedia.org/wiki/The_Mythical_Man-Month)) — Especially the chapter on *conceptual integrity*: the argument that a system designed by one person with a coherent idea will beat a system designed by ten with all the right features. The takeaway William cares about: more features ≠ more value, and coherence is its own kind of simplicity.
- **John Gall, *Systemantics*** ([1975](https://en.wikipedia.org/wiki/John_Gall_(author))) — A satirical engineering classic with a serious thesis: *"A complex system that works is invariably found to have evolved from a simple system that worked."* Big-bang complex designs almost always fail. Start simple, let it grow under pressure, keep what survives.
- **Sandi Metz, "The Wrong Abstraction"** ([2016](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction)) — The clearest essay ever written on why premature DRY is worse than copy-paste. The cost of un-doing a wrong abstraction is much higher than the cost of duplication; experienced engineers underestimate this every time. William's *Metz rule* is from here.
- **Richard Gabriel, "Worse is Better"** ([1991](https://www.dreamsongs.com/RiseOfWorseIsBetter.html)) — A meditation on why the simpler, less-correct Unix/C aesthetic beat the more correct, more complete MIT/Lisp aesthetic in the marketplace. The uncomfortable conclusion: simplicity of *implementation* matters more for adoption than simplicity of *interface* or correctness of *behaviour*. Think about that whenever you choose between "the right thing" and "the thing that ships."
- **Erik Hollnagel & David Woods, *Resilience Engineering*** ([2006](https://www.taylorfrancis.com/books/edit/10.1201/9781315605685/resilience-engineering-erik-hollnagel-david-woods-nancy-leveson)) and Hollnagel's [ETTO Principle](https://erikhollnagel.com/ideas/etto-principle/) (2009) — The honest counterweight to Ockham. In safety-critical systems, redundancy and slack aren't waste — they're the whole point. Cutting "unnecessary" checks is exactly how Therac-25, Ariane 5 and most aviation accidents happened. William respects this as the *boundary exception*: at irreversible system edges, parsimony bows to thoroughness.
- **Nancy Leveson, *Engineering a Safer World*** ([2011](https://mitpress.mit.edu/9780262533690/engineering-a-safer-world/)) and her earlier paper on [Therac-25](https://web.stanford.edu/class/cs240/old/sp2014/readings/therac-25.pdf) (1993) — The empirical case-study record. Therac-25, Ariane 5, Knight Capital. Each is a story about a "small" simplification or reuse that killed people or burned $440M in 45 minutes. Required reading before any review where someone proposes deleting a check they don't fully understand.
- **Google SRE Book, ch. 15: "Postmortem Culture"** ([Beyer et al., 2016](https://sre.google/sre-book/postmortem-culture/)) — Blameless postmortems at scale consistently find that the proximate cause of outages is *added complexity* (config, retries, fallbacks, exception paths) far more than missing features. The data backs Hoare; complexity is where the bugs live.
- **The Linux kernel's [submitting-patches.rst](https://www.kernel.org/doc/html/latest/process/submitting-patches.html), Google's [Code Review Developer Guide](https://google.github.io/eng-practices/review/), and the [NASA JPL Power of Ten](https://spinroot.com/gerard/pdf/P10.pdf)** — The contemporary house rules. "One logical change per patch" (Linux), "a CL should be a minimal change" (Google), and JPL's ten rules for safety-critical C. Cite these when you need authority that isn't your own taste.

If you only read three of these, read **Hickey** for the vocabulary, **Hoare** for the discipline, and **Metz** for the practical bite. If you only read one, read Hickey.

## Inspired by

The naming follows John Gruber's *Daring Fireball* taste-authority frame — the kind of agent you build when you want to ask "what would [an opinionated authority] say?" rather than "what does the linter want?" Compare: a `what-would-gruber-say` agent for macOS/HIG taste, an `app-store-police` agent for App Store survival, etc. The genre's hook is naming a person, not an abstraction — the agent earns its voice from a specific point of view.

References:
- [Daring Fireball](https://daringfireball.net/) — John Gruber's blog, the prototype taste-authority lens
- [Claude Code subagents](https://docs.claude.com/en/docs/claude-code/sub-agents) — the platform this is built for

## Licence

MIT. Cite the inherited-heuristic authors as you would normally — those references are commentary, not relicenced material.
