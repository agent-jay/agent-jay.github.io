---
title: Recurrent Models of Visual Attention
date: 2017-11-15
authors: Volodymyr Mnih, Nicolas Heess, Alex Graves, Koray Kavukcuogluaper

---

## Summary

- This paper is important not necessarily for it's result but for it's
  formulation of "hard attention." 
- Where to focus in an image is decided by a stochastic policy (in
  reinforcement learning parlance) that outputs a single location (agent's
  action) given the hidden state of the RNN. This sampling process is
  non-differentiable and hence cannot be backpropagated through (More detailed
  notes can be found [here](./glimpse_yeung.md), also check Andrej Karpathy's
  post on policy gradients [here](http://karpathy.github.io/2016/05/31/rl/). 
- By sampling rollouts of the game under the policy, we can compute the
  gradient update of the RNN using the REINFORCE rule.

## Questions

- How does this attention differ from Bahdanau attention for MT?
- In terms of ops/speed/O(n) calculations is this really more efficent than a CNN? My
  intuition suggests not.

## Answers

- Bahdanau attention captures and compresses the information already seen
  before by the encoder, and is produced by asking the neural net "what do I
  expect to produce next?" This is done by computing similarity
  (cosine/MLP/..) between the "expected input vector"(function of prev. hidden
  state and last word produced) and the encoder hidden states.
    - In this paper, the attention is being used for exploration- "what should
      I see next?"

