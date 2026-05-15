# Development Log – The Torchbearer

**Student Name:** Ryan Racela
**Student ID:** 133032559

> Instructions: Write at least four dated entries. Required entry types are marked below.
> Two to five sentences per entry is sufficient. Write entries as you go, not all in one
> sitting. Graders check that entries reflect genuine work across multiple sessions.
> Delete all blockquotes before submitting.

---

## Entry 1 – [5/14/26]: Initial Plan

> Required. Write this before writing any code. Describe your plan: what you will
> implement first, what parts you expect to be difficult, and how you plan to test.

First, I need to understand why this isn't just a normal shortest-path problem. I think my first implementation step would be to calculate the shortest distances between the relic locations, since I'm going to need those distances later on. The most difficult part would probably be tracking the state of the nodes and figuring out how to get the best relic collection order. I plan to test each part with small custom graphs before trying larger ones, along with using the provided tests.

---

## Entry 2 – [5/14/26]: [Change in Precomputation Design]

> Required. At least one entry must describe a bug, wrong assumption, or design change
> you encountered. Describe what went wrong and how you resolved it.

In my precomputation design, I first thought that the exit node was a source, but I realized that it shouldn't because we don't need the distance to the exit, and not from it. So, I decided that only the spawn node and the relic nodes should be the source nodes. This affected the complexity because before, the number of Dijkstra runs would be the number of relics + 2, but now it's the number of relics + 1.

---

## Entry 3 – [5/14/26]: [Short description]

_Your entry here._

---

## Entry 4 – [5/14/26]: Post-Implementation Reflection

> Required. Written after your implementation is complete. Describe what you would
> change or improve given more time.

_Your entry here._

---

## Final Entry – [5/14/26]: Time Estimate

> Required. Estimate minutes spent per part. Honesty is expected; accuracy is not graded.

| Part | Estimated Hours |
|---|---|
| Part 1: Problem Analysis | 30 |
| Part 2: Precomputation Design | 75 |
| Part 3: Algorithm Correctness | |
| Part 4: Search Design | |
| Part 5: State and Search Space | |
| Part 6: Pruning | |
| Part 7: Implementation | |
| README and DEVLOG writing | |
| **Total** | |
