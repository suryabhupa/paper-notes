## [On Inductive Biases in Deep Reinforcement Learning](https://arxiv.org/abs/1907.02908)
#### Matteo Hessel, Hado van Hasselt, Joseph Modayil, David Silver (2019)

**Short Summary**: Hessel et. al. ablate oft-used "behind-the-scenes" meta-choices used by actor-critic style algorithms for Atari. These choices include: 
1. using discounted returns (as opposed to optimizing the undiscounted return)
2. using reward clipping (as opposed to using the raw returns, whose scale can vary wildly as training proceeds)
3. using specific pre-processing per state (using tricks like frame stacking, and specific pre-processing)
4. using action repeats (to encourage more exploration, use less compute, etc.)

They ablate them against more general, "learned" variants of each choice, using either related work or with simple modifications. Notably: 

1. learn the discount factor using meta-gradient approaches
2. use PopArt for reward scale
3. using a recurrent state function
4. learning a "commitment" value (like options) for how long to repeat an action

The results are largely what one might expect; learned variants are competitive but not always better on Atari. Notably, using undiscounted returns is actually quite bad, so in general discounting is quite important.

Next, they apply these tricks designed for Atari for DM-Control, a continuous control set of tasks. As somewhat expected, a lot of the atari-specific tricks don't transfer to continuous control tasks, but as also somewhat expected, the generic agent still does better on average! The big takeaway here is that when we pour inductive biases into our agents, we are potentially trading off performance for generality, and as is seen from results like AlphaGo --> AlphaZero, removing these priors might actually lead to more general and more performant agents.

Interestingly enough, a version of this paper was rejected from ICLR 2019, mainly on the basis that it didn't add anything new that we didn't know already know, which is a fair assesment. However, this was still reasonably useful to read; the authors tend to be quite clear writers, so it was good to see these tricks catalogued in one place for reference. 
