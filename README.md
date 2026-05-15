# The Torchbearer

**Student Name:** Ryan Racela
**Student ID:** 133032559
**Course:** CS 460 – Algorithms | Spring 2026

> This README is your project documentation. Write it the way a developer would document
> their design decisions , bullet points, brief justifications, and concrete examples where
> required. You are not writing an essay. You are explaining what you built and why you built
> it that way. Delete all blockquotes like this one before submitting.

---

## Part 1: Problem Analysis

> Document why this problem is not just a shortest-path problem. Three bullet points, one
> per question. Each bullet should be 1-2 sentences max.

- **Why a single shortest-path run from S is not enough:**
  A shortest-path run isn't enough because you also have to account for what relics you've collected and which ones you haven't, what order to visit each relic location, and that you might have to make a decision that doesn't seem optimal right away in order to reach the ultimate solution.

- **What decision remains after all inter-location costs are known:**
  You need to decide what order to visit each relic chamber to achieve the lowest cost at the end.

- **Why this requires a search over orders (one sentence):**
  It requires a search over orders because the total torch fuel cost depends on what order you visit the relics, not just the individual distance costs.

---

## Part 2: Precomputation Design

### Part 2a: Source Selection

> List the source node types as a bullet list. For each, one-line reason.

| Source Node Type | Why it is a source |
|---|---|
| Entrance node | The walk begins here, so we need to know the cheapest way from this node to each relic |
| Relic chamber node | After collecting the relic at this node, your next move has to start from this node |

### Part 2b: Distance Storage

> Fill in the table. No prose required.

| Property | Your answer |
|---|---|
| Data structure name | Nested dictionary / distance table |
| What the keys represent | Outer keys are the source nodes and inner keys are the destination nodes |
| What the values represent | Shortest path cost from the source node to destination node |
| Lookup time complexity | Average case O(1) |
| Why O(1) lookup is possible | Dictionaries in Python use hash table lookup by key |

### Part 2c: Precomputation Complexity

> State the total complexity and show the arithmetic. Two to three lines max.

- **Number of Dijkstra runs:** 
    number of relics + 1
- **Cost per run:** 
    O((V + E) log V)
- **Total complexity:** 
    (R + 1) * O((V + E) log V)
    = O((R + 1)(V + E) log V)
- **Justification (one line):** 
    Dijkstra is run once from the spawn node and then once from each relic node R

---

## Part 3: Algorithm Correctness

> Document your understanding of why Dijkstra produces correct distances.
> Bullet points and short sentences throughout. No paragraphs.

### Part 3a: What the Invariant Means

> Two bullets: one for finalized nodes, one for non-finalized nodes.
> Do not copy the invariant text from the spec.

- **For nodes already finalized (in S):**
  Their stored values are the true lowest torch fuel cost from the source node.

- **For nodes not yet finalized (not in S):**
  Their stored values are the lowest torch fuel cost for now, but they might improve later on.

### Part 3b: Why Each Phase Holds

> One to two bullets per phase. Maintenance must mention nonnegative edge weights.

- **Initialization : why the invariant holds before iteration 1:**
  The source node starts with a torch fuel cost of 0 because it costs nothing to reach itself. All of the other nodes start at infinity because no paths to them have been discovered yet.

- **Maintenance : why finalizing the min-dist node is always correct:**
  The algorithm always chooses the unfinished node with the lowest known torch fuel cost. The edge weights are nonnegative, so every other path afterwards would either have an equal or greater cost.

- **Termination : what the invariant guarantees when the algorithm ends:**
  When the algorithm ends, every reachable node's fuel cost is correct. Any node still at infinity isn't reachable from the source node.

### Part 3c: Why This Matters for the Route Planner

> One sentence connecting correct distances to correct routing decisions.

Correct distances would ensure that the route planner compares the different orders of visiting the relic chambers using accurate costs of torch fuel.

---

## Part 4: Search Design

### Why Greedy Fails

> State the failure mode. Then give a concrete counter-example using specific node names
> or costs (you may use the illustration example from the spec). Three to five bullets.

- **The failure mode:** 
  Greedy can choose the next closest relic, but it might not give the lowest torch fuel cost in the end.
- **Counter-example setup:**
  Similar to the illustration example from the spec, suppose the entrance node is S, the relic chambers are B, C, and D, the exit node is T, and the inter-location travel costs are as follows:
  | From \ To | B   | C   | D   | T   |
  |-----------|-----|-----|-----|-----|
  | S         | 1   | 2   | 2   | --  |
  | B         | --  | 100 | 100 | 1   |
  | C         | 1   | --  | 1   | 100 |
  | D         | 1   | 1   | --  | 1   |
- **What greedy picks:**
  Greedy would pick B first because going from S to B would cost 1, which is the lowest first cost.
- **What optimal picks:**
  The optimal method would pick C, because the optimal order would be S -> C -> D -> B -> T.
- **Why greedy loses:**
  Greedy loses because it would choose S -> B -> C -> D -> T with a total torch fuel cost of 103, while the optimal method would choose S -> C -> D -> B -> T with a total fuel cost of 5.

### What the Algorithm Must Explore

> One bullet. Must use the word "order."

- The algorithm must explore different orders of visiting the relic chambers instead of just choosing the closest relic.

---

## Part 5: State and Search Space

### Part 5a: State Representation

> Document the three components of your search state as a table.
> Variable names here must match exactly what you use in torchbearer.py.

| Component | Variable name in code | Data type | Description |
|---|---|---|---|
| Current location | current_loc | node | The current node that the search is on |
| Relics already collected | relics_visted_order | list[node] | Relics collected so far in order they were visited|
| Fuel cost so far | cost_so_far | float | Total torch fuel cost so far |

### Part 5b: Data Structure for Visited Relics

> Fill in the table.

| Property | Your answer |
|---|---|
| Data structure chosen | set |
| Operation: check if relic already collected | Time complexity: O(1) average case |
| Operation: mark a relic as collected | Time complexity: O(1) average case |
| Operation: unmark a relic (backtrack) | Time complexity: O(1) average case |
| Why this structure fits | A set is able to remove and re-add relics efficiently during recursive backtracking |

### Part 5c: Worst-Case Search Space

> Two bullets.

- **Worst-case number of orders considered:** 
  Worst-case number of orders is k!
- **Why:** 
  If there are k relics, the worst case is that the search will have to try every possible order.

---

## Part 6: Pruning

### Part 6a: Best-So-Far Tracking

> Three bullets.

- **What is tracked:**
  The best complete route so far with its torch fuel cost and the order it visited each relic.
- **When it is used:**
  It's used whenever the search completes a route or checks whether a partial route can give a lower torch fuel cost.
- **What it allows the algorithm to skip:**
  It allows the algorithm to skip partial routes that are already more expensive than the best route found so far.

### Part 6b: Lower Bound Estimation

> Three bullets.

- **What information is available at the current state:**
  The current location, the torch fuel cost so far, the relics remaining, and the shortest-path costs.
- **What the lower bound accounts for:**
  It accounts for the torch fuel cost so far and an optimistic esimate of the best-case remaining torch-fuel cost to complete the route.
- **Why it never overestimates:**
  It never overestimates because to make its estimates, it only uses the shortest-path costs, which are already the minimum costs to travel between nodes.

### Part 6c: Pruning Correctness

> One to two bullets. Explain why pruning is safe.

- It's safe because if the current route's cost is already as expensive as the best route found so far, then it can't generate a better cost.

---

## References

> Bullet list. If none beyond lecture notes, write that.

- lecture notes
- Abdul Bari Algorithms playlist on Youtube
