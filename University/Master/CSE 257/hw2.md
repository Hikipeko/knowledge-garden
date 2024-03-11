### Question 5

#### 1 Roughly describe the MDP for blackjack

**States** are  (user_sum, user_has_Ace, dealer_first) tuples. (described in the code)

- user_sum: sum of user's cards' value, where A counts as 1. Possible values are 2 to 20
- user_A_active: whether the Ace in user's hand can be used as 11. Possible values are 0 and 1
- dealer_first: the first card's value of dealer, where A counts as 1. Possible values are 1 to 10
- Special states:
- WIN_STATE:    represented as (0,0,0)
- LOSE_STATE:   represented as (1,0,0)

**Actions** are hit or stand.

**Transitions** Is from one state to another. Roughly speaking, it's the probability of the next card draw from the deck.

**Reward** is 1 if win the game, -1 if lose, 0 if game continues.

Based on the above definition for Markov Decision Process, we can derive an optimal policy corresponds to the optimal way of solving 2048. The policy is an deterministic action (direction to move) for each state.

**Trajectory**: (12, 0, 10) -(hit)-> (14, 0, 10) -(hit)-> (17, 0, 10) -(hit)-> (0, 0, 0)

#### 2 Temporal-difference policy evaluation