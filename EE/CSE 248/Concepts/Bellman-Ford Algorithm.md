* Computes shortest path from a single source vertex to all of the other vertices in a weighted graph.
* Graph with no negative cycle.
* In iteration $k$, we get the shortest distance by using at most $k$ edges from the source, we we keep updating `dist` in each iteration.