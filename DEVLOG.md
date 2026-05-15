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

## Entry 3 – [5/15/26]: [Recursive Search Fix]

When I was implementing def _explore, I realized that after each recursive call, I need to undo the changes made to the list of visited relics and the relics remaining so that all the other possible orders can be tried correctly. I also figured out that I needed to use the copy operation when saving the best route because when I didn't, best[1] would reference the same list object, so the changes made in backtracking would modify the solution that was saved.

---

## Entry 4 – [5/15/26]: Post-Implementation Reflection

> Required. Written after your implementation is complete. Describe what you would
> change or improve given more time.

If I had more time, I think I could strengthen my pruning condition. What I have is really simple, so with more time, I think I could figure out a way to make it stronger. I could also come up with more specific tests for edge cases. I know we're only supposed to return the relic visit order, but I think it would be nice to make it where it returns the whole node-by-node walk instead.

---

## Final Entry – [5/15/26]: Time Estimate

> Required. Estimate minutes spent per part. Honesty is expected; accuracy is not graded.

| Part | Estimated Hours |
|---|---|
| Part 1: Problem Analysis | 30 |
| Part 2: Precomputation Design | 75 |
| Part 3: Algorithm Correctness | 20 |
| Part 4: Search Design | 30 |
| Part 5: State and Search Space | 60 |
| Part 6: Pruning | 60 |
| Part 7: Implementation | 60 |
| README and DEVLOG writing | 90 |
| **Total** | 425 |
