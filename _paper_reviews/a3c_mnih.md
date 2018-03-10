---
title: Asynchronous Methods for Deep Reinforcement Learning
date: 2017-11-14
authors: Volodymyr Mnih, Adrià Puigdomènech Badia, Mehdi Mirza, Alex Graves, Timothy P. Lillicrap, Tim Harley, David Silver, Koray Kavukcuoglu 

---

## Summary

Introduces a new way to parallelize RL algorithms- 1-step DQN, n-step DQN,
1-step SARSA and Actor-Critic.

At the time of writing, the A3C algorithm introduced in this paper attained
SOTA results on the Atari Learning Environment.

Parallelizing actors using same/similar policies stabilizes learning and rids
the need for experience replay. This also allows the use of deep variants of RL
algorithms that were hitherto untouched as they were on policy (SARSA) or
required storing too much data (n-step Q learning). Reason for doing this as
quoted by the paper:
> running in parallel are likely to be exploring different parts of the environment. Moreover, one can explicitly use different exploration policies in each actor-learner to maximize this diversity. By running different exploration policies in different threads, the overall changes being made to the parameters by multiple actor-learners applying online updates in parallel are likely to be less correlated in time than a single agent applying online updates.  

Another quirk of parallel actors and no experience replay is the accumulation
of gradients across actors, across timesteps. This is similar to batching data
in standard Q learning. Accumulating gradients across actors probably acts as
some kind of smoothing/averaging to stabilize updates.

## Things I learned

- n-step Q learning is better than standard Q Learning (1-step Q Learning)
  becuase reward propagation is slower. More specifically, a reward directly
  only affects the Q value of the previous state,action pair and indirectly
  affects the nearby state/action pairs (by modifying the Q function,
  parametrized by the weights of a neural network).This can make the learning
  process slow since many updates are required the propagate a reward to the
  relevant preceding states and action  
  - Using n-step returns means that a reward affects state+actions made n-steps
    back in time
- Entropy regularization proposed by Williams and Peng back in 1995 is a way to
  penalize policies that are too sure (or atleast to ensure some amount of
  stochasticity and therefore exploration) via the loss function
- Performance scales linearly with number of threads for A3C but *superlinear*
  for 1-step methods!

## Questions

- In n-step learning, what is tmax used in the paper?
- When implementing this in Python, using multiprocessing how do you give each
  process an instance of a Q/Policy network and how to share parameters and
  accumulate gradient updates?
- Actor-critic is different from vanilla REINFORCE as it uses n-step returns
  instead of whole trajectories. How exactly does the formula for gradient
  updates differ (the log likelihood)? There won't be a summation over steps as
  the updates are done online for n-steps?

## Answers

- And the gradients are accumulated for 5 steps too.
- Async updates were shown to be an effective way to parallelize SGD in the
  Hogwild! paper. If my understanding is correct, it assumes that not all
  weights are written to by each actor, hence simultaneous updates shouldn't
  overwrite any key information. This
  [link](http://pytorch.org/docs/master/notes/multiprocessing.html) shows how
  Hogwild! style gradient updates can be done with PyTorch 
