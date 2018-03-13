---
title: Continuous control with deep reinforcement learning
date: 2017-11-15
category: Deep Reinforcement Learning
tags: [RL, Atari]
---
*Timothy P. Lillicrap, Jonathan J. Hunt, Alexander Pritzel, Nicolas Heess, Tom Erez, Yuval Tassa, David Silver, Daan Wierstra*

[Link to paper](https://arxiv.org/abs/1509.02971)

## Summary

- Model-free, off-policy actor-critic algorithm 
- Contribution of the paper: applies deterministic Policy Gradients(DPG) to
  continuous control problems using Deep Neural function approximators, and
  apply the tricks from DQN to prevent the NN from diverging. Introduce soft
  target net update as another trick.
- Off policy
    - stochastic behavior policy (good for exploration)
    - deterministic target policy (what the agent actually follows) 
- Actor Critic
    - Actor: target policy network. State->single real value representing
      action (continuous!)
        - DDPG theorem gives update rule for modifiying weights of this network
    - Critic: Estimates Q(s,a). Used for computing the TD error
        - These weights can be updated by minimizing the loss between TD target
          and estimated Q.
- Remember all this machinery though is used to finally get the actor network
  (that generates deterministic policy) $\mu

## Things I learned

- Deterministic Policy Gradients
    - TL;DR: if you use a deterministic policy, you can't use the usual
      machinery for estimating policy gradients. [Silver](http://proceedings.mlr.press/v32/silver14.pdf)
      showed that the gradients can indeed be computed using the DPG theorem
        - Why would one want to use a deterministic policy at all? In high
          dimensional continuous action spaces, the stochastic policy gradient
          can be expensive to compute. 
        - But then how can one ensure sufficient exploration?...off policy
          actor-critic with added noise in action selection.
    - First read this [blog post](http://pemami4911.github.io/blog/2016/08/21/ddpg-rl.html)
- I think this paper is more similar to Q Learning than Policy Gradients methods
    like REINFORCE? I don't think so anymore...Maybe it has that flavor bc it's
    actor critic and the critic used the usual Q learning. Also, the tricks
    used for NN convergence from DQN were used here too.

## Questions

- Why use the critic at all. What are we gaining? 
- Why soft updates to target network? Is this still a common trick to prevent
  divergence of the NN function approximator?

## Answers

- When the critic used as the baseline (needed to compute Advantage) is itself the Q
  function/value function, it becomes actor critic. The reason to do this is to
  reduce variance. It also enables online TD updates instead of similar to Montecarlo
  estimates that require whole sample trajectories




