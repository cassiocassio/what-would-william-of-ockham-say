# what-would-william-of-ockham-say

A small parsimony-review subagent for [Claude Code](https://docs.claude.com/en/docs/claude-code/sub-agents). Asks: do we actually need this complexity?

> Entities should not be multiplied beyond necessity.
> — attributed to William of Ockham, ~1320

## Who was William of Ockham

A 14th-century English Franciscan friar (~1287–1347), born in the Surrey village of Ockham, who taught at Oxford. He argued that universal categories — "redness," "humanity" — exist only as names in the mind. This position, *nominalism*, is the philosophical seed of the principle that bears his name. The "razor" itself — *entia non sunt multiplicanda praeter necessitatem* — is a later formulation, not a literal quotation, but it's a fair summary of the posture: of every claim that the world contained one more kind of thing, William asked whether we actually needed it.

He was summoned to Avignon over his theology, fell out with the Pope over apostolic poverty, fled to Munich, and almost certainly died of the Black Death.

## What it does

**Mode A — adjudicator.** Runs after a fan-out review workflow has merged findings into a single list. Annotates each as `real`, `edge`, or `speculative`; picks the parsimonious fix where several have been proposed for the same problem; clusters duplicates; flags bikeshed crowding.

**Mode B — standalone.** Given a session, branch, or diff, finds the original task, computes a task-vs-work ratio, names tells from a fixed taxonomy, and proposes the smallest commit split.

It's a signal, not a gate. Complexity that earns its keep is fine.

## Install

```sh
curl -L https://raw.githubusercontent.com/cassiocassio/what-would-william-of-ockham-say/main/what-would-william-of-ockham-say.md \
  -o ~/.claude/agents/what-would-william-of-ockham-say.md
```

Or per-project: `<repo>/.claude/agents/what-would-william-of-ockham-say.md`.

## Heuristics, with sources

The agent doesn't invent its rules. It borrows from people who've thought about parsimony longer than I have:

- **Rule of Three** — Fowler & Roberts, *Refactoring* (1999). Don't abstract until the third occurrence.
- **Simple vs. easy** — [Rich Hickey, *Simple Made Easy*](https://www.infoq.com/presentations/Simple-Made-Easy/) (2011). A feature flag is *easy* and makes the system *complex*.
- **Hoare's test** — [C.A.R. Hoare, Turing Award lecture](https://dl.acm.org/doi/10.1145/358549.358561) (1980). Make it so simple there are obviously no deficiencies.
- **The wrong abstraction** — [Sandi Metz](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction) (2016). Duplication is far cheaper than the wrong abstraction.
- **Parkinson's Law of Triviality** — C.N. Parkinson (1957). Disproportionate weight on trivial issues because they're easy to have a view on.
- **One logical change per patch** — [Linux kernel](https://www.kernel.org/doc/html/latest/process/submitting-patches.html); [Google](https://google.github.io/eng-practices/review/).
- **Knuth, in full** — Donald Knuth (1974). "We should forget about small efficiencies, say about 97% of the time." Performance work wants a measurement, not a hunch.
- **Chesterton's Fence** — G.K. Chesterton (1929). Don't remove what you don't understand. Knight Capital is the modern, expensive version.
- **ETTO / boundary exception** — [Erik Hollnagel](https://erikhollnagel.com/ideas/etto-principle/) (2009). At irreversible system boundaries, parsimony defers to thoroughness.
- **Boy Scout in spirit, separate-commit in practice** — Bob Martin's "leave it cleaner" fuels the *while I'm here* pattern. Do the cleanup; don't bundle it.
- **Wirth's prior** — [Niklaus Wirth, *A Plea for Lean Software*](https://cr.yp.to/bib/1995/wirth.pdf) (1995). Software expands to fill the available memory.

The longer reading list — Parnas on information hiding, Brooks on conceptual integrity, Gabriel on *Worse is Better*, Leveson on the Therac-25 and Ariane-5 cases — sits behind these. If the tradition is new to you, Hickey's talk is the best hour-long introduction.

## Inspired by

The *what-would-X-say* naming follows the genre that John Gruber's [Daring Fireball](https://daringfireball.net/) tends to inspire: agents that take their voice from a named point of view rather than a job description.

## Licence

MIT.
