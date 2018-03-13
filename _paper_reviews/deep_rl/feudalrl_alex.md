---
title: FeUdal Networks for Hierarchical Reinforcement Learning
date: 2017-12-9
category: Deep Reinforcement Learning
tags: [RL, Atari]
---
*Alexander Sasha Vezhnevets, Simon Osindero, Tom Schaul, Nicolas Heess, Max Jaderberg, David Silver, Koray Kavukcuoglu*

[Link to paper](https://arxiv.org/abs/1703.01161)

## Summary

Feudal Networks are one way of achieving Heirarchical Reinforcement Learning,
which is essentially planning actions *at multiple resolutions of time *. This
paper approaches it using end-to-end learning using a 2 stage architecture- a
manager(lower temporal resolution) and a worker(higher resolution). The
intuition is that this will help with the long-term credit assignment problem.

This paper differs from prior work that uses the options framework in that it
doesn't need explicitly designed sub-goals, they are learned automatically

The Manager sets goals in a latent space that is learnt. The Worker conditions
actions on these goals. The Manager recieves only feedback (rewards) from the
environment, not the worker. Training is done using something the paper
proposes- *transitional policy gradient*.

## Thoughts

- A lot of restrictions or un-intuitive ideas are due to the requirement that
  the whole model be differentiable. An example being how goals from the
  Manager are forced to affect the worker(Sec 3.1).
- The semantic meaning of the goal is literally a difference/transition that
  needs to be actuated, where $ cos(s_{t+c}- st, g_t(\theta)) is being
  minimized and c is some fixed number of steps

## Questions

- What are the big ideas and successes in Heirarchical Reinforcement Learning
- Are the subpolicies learnt human interpretable?
- In the Atari section, the authors compare the settings used to control
  credit assignment, discounts and BPTT length between the FuN and LSTM. Why
  are FuNs given longer BPTT unroll length(400) than LSTM(100)? That doesn't sound like a
  fair comparison

## Answers

- Read more about the Options framework. Both the regular and deep versions
- The authors perform a test where a goal is fixed for a whole episode and the
  subpolicy is played out. The goals/subgoals by themselves are uninterpretable

