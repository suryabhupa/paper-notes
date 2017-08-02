## Proximal Policy Optimization Algorithms
##### John Schulman, Filip Wolski, Prafulla Dhariwal, Alec Radford, Oleg Klimov (2017)

**Short Summary**: Schulman et. al. improve Trust Region Policy Optimization (TRPO) with a new variant of the recently introduced Proximal Policy Optimization by replacing the objective function with a simpler objective function with better sample complexity, at least empirically. 

Policy gradient algorithms are a class of algorithms that optimize the policy function directly by maximizing expected return. However, the two major subclasses of policy gradient methods are not without drawbacks; vanilla policy gradients have poor data efficiency and robustness, while trust region or natural gradient methods are quite complicated (getting good implementations of them is hard), and are not compatible with more exotic architectures, such as those that involve dropout or parameter sharing (such as sharing between policy and value function). 

TRPO stably achieves the SOTA for continuous control problems on the OpenAI Gym, MuJoCo, and Roboschool environments. A surrogate objective is maximized subject to a constraint on the step size. However, it involves computing a KL term and involves using conjugate gradient by making a linear approximation to the objective and a quadratic approxmation to the constraint.

PPO was a recently introduced (by DeepMind) that solves some of these problems by replacing the hard KL constraint with a KL pentalty as originally formulated by TRPO. They introduce a clipped surrogate loss based on the conservative policy iteration scheme, which discourages the ratio of \pi_new/\pi_old to deviate too far from 1 (i.e. take a step too far away from the previous policy parameterization). It's clever because when it moves too far and the probability ratio * advantage estimate is clipped, gradient is only passed when it moves too far in the wrong direction (clipping prevents gradient from flowing). 

Despite its simplicity, it does quite well on both continuous control tasks AND discrete games that normally DQNs reign supreme at. It's become the de facto policy gradient implementation at OpenAI and is the subject of some of my own upcoming research. Stay tuned for more!
