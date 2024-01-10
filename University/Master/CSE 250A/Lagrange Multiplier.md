https://en.wikipedia.org/wiki/Lagrange_multiplier

A strategy for finding the local maxima and minima of a function subject to equation constraints. 

We want to covert the constrained problem into a form such that the derivative test of an unconstrained problem can still be applied. The Lagrangian is defined as

$$
\mathcal{L}(x, \lambda) \equiv f(x) + \langle \lambda, g(x) \rangle.
$$

By finding the stationary points of $\mathcal L$ (let all the partial derivative to be 0), we find a maximum or minimum for $f(x)$.