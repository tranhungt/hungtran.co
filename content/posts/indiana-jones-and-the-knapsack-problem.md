---
title: "Indiana Jones and the Knapsack Problem"
date: 2013-06-27T18:56:41-05:00
draft: false
---

If Indiana Jones was escaping a collapsing treasure dungeon with a limited capacity knapsack, how would he decide what to take with him?

Rationally, you would want to get the most value for what you can carry, much like eating the most expensive food at a buffet to get your money’s worth. But how would you decide, since each item has weight and value, and your bag/stomach only has so much capacity?

That’s a classic NP-Complete Problem. One way to solve it is to make permutations of all the possible ways to combine the items to see what your optimal value would be. But that would be way too long. It is found that if 1 calculation takes 1millisecond, and if you have an algorithm that takes 2^50 calculations, it would take 1 billion centuries to run. So if the buffet line had 50 items to choose from, you may be dead before you even start eating… maybe. But maybe we can narrow that down, by eliminating all the permutations that exceed your capacity. Even then, the algorithm still runs in polynomial time, and will take an infeasible amount of time.

There are other ways of looking at this. One is a top down approach, much like a fibonacci algorithm, that for the nth term, calculates the n-1 + n-2 terms in order to get the nth term. That means that n-1 also requires a calculation of n-2 + n-3. This branching would mean that there are about n^n calculations. But why would we want to calculate n-2 twice? And other fibonacci numbers lower down on the branch are also being calculated multiple times as well.

Instead of working from the top down, where must continually re-calculate the values beneath, we could work from the bottom up. That is, we could build a table that, once we have calculated a value, store it, for reference.

Then comes the introduction of dynamic programming. The idea is that once we have figured out the best way of doing something at a lower level, we can apply that knowledge when a problem arises that seeks to build on top of it. An example is the Indiana Jone’s knapsack problem. Once we figure out that, if there is only one item to take, then he should take that item if it fits his bag. That becomes the optimal solution at one item. If a second item exists, the he could look at his one-item optimization to see whether he could do better. This could be continued on to item 3, 4, etc, as each additional item is only one more set of calculations compared to the previous. This algorithm is a pseudo-polynominal problem, and has a time complexity of O(K n) where K is the capacity and n is the number of items.

Branch and Bound is another way of approaching this problem. It works by branching out possible estimations of the optimal value. Branching does the branching of whether to take the item, or not take the item. Bounding determines that we should only go down a path if there is a possibility that the resulting value could be higher than our previous encounters. To start our algorithm, there are two constraints we can relax, that is, what if we have no constraints, and what if we could take a fraction of a value at a fraction of the cost; that is, instead of either taking or not taking a big chunk of chocolate, we could take a fraction of it, and only pay a fraction of the price. By relaxing our constraints, we are able to make an estimate of what our algorithm is capable of in the best possible scenario and we can work from there. The algorithm here is not specific, but quite fast, because it prunes non-sensical or non-feasible solutions rather quickly.

Thought of the day - Is there a way to combine dynamic programming with branch and bound to make a more efficient algorithm?

P.S. It’s man vs machine, and you want to win! Arg!