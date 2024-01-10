## L22 Backtracking, Branch and Bound Algorithms

### Backtracking

Systematically consider all possible outcomes of each decision, but prune searches that are not satisfy constraints. Used for problems that require **any** solution.

**General Form**

```algorithm
checknode(node v)
	if (solution(v)) // check if constraint is satisfied
		write solution
	else
		for each node u adjacent to v
			if (promising(u))
				checknode(u)
```

#### n Queens

### Branch and Bound Algorithms

Used for pruning in Optimization problems. Commonly used for NP-hard problems such as TSP.

```algorithm
checknode(node v, currBest)
	if (solution(v)) then
		update(currBest)
	else
		for each child u of v
			checknode(u, currBest)
	return currBest
```
