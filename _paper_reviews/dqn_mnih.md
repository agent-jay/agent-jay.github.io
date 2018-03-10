---
title: Playing Atari with Deep Reinforcement Learning
date: 2017-11-14
authors: Volodymyr Mnih, Koray Kavukcuoglu, David Silver, Alex Graves, Ioannis Antonoglou, Daan Wierstra, Martin Riedmiller

---

## Summary and Notes

This paper's major contribution is applying what we've learnt about neural nets
since 2010, to solve Reinforcement Learning. large convolutional neural nets to
work effectively as function approximators in Q Learning. The strong guarantees
on convergence for linear function approximators don't apply to non-linear FAs
such as neural nets, and a host of tricks are needed to keep them from
catastrophically diverging.  Specifically, they manage to do this with the
following:

- An Experience Replay memory buffer to store <s,a,r,s'> tuples. These are what
  are actually sampled from when making Q Learning updates. 
    - This is done to make the targets stationary. If the target distribution
      itself were changing, the NN would have a harder time learning. This is
      also the reason the buffer is huge- order of mag. 10^6. 
    - Additionally, sampling from this buffer decorrelate data presented to the
      NN (and returns some semblance of iid data that most ML techniques
      assume). 
    - Data efficiency- better to reuse data that was seen that to throw it away
- Separate Target Network 
    - The NN generalizes to states nearby with parameter updates, so even if
      you're updating for one state the Q of an as yet unseen next state is
      being modified (unlike in tabular case where updates are made only for
      single observed state). Creates a loop where you keep increasing
      estimates of Q as training goes on. 
    - Instead, a separate Target network is used as the off policy Q Function
      approximator. This network is updated periodically with the other
      network's weights. 
    - Using larger nets surprisingly better- more forgiving to above problem-
      each update less affects nearby states
- Atari was a problem for which you can get away with non-markov states since
  the current frame gives you most of the informaiton you need to make a
  decision
- Double Q learning and Prioritized Experience Replay are easy ways to speed up
  convergence, make DQN more sample efficient and increase max score.

## Things I learned (the hard way):

- The Convolutional neural net is fed the last 4 game frames as 4 different channels
- Reducing actions from the action space of the agent is one way to speed up
  convergence (or at least debug) since you don't need the agent to play 1000s
  of games to figure out that not doing anything is a bad strategy.
- Huber loss instead of mean squared error is used in practice- doesn't
  penalize outlier values as much.
- Gradient clipping during optimiziation(? not sure if done by authors)
 
## Questions

- With this above setup is there an easy way to parallelize training, perhaps
  using multiple agents. One way I can think of is to share the experience
  buffer alone between several agents acting simultaneously. Not sure how
  Networks should update in a way that makes sense theoretically.
- How would punishments for existence and dropping a ball (in Breakout) in
  addition to positive rewards help (or hinder) learning?

## Answers

- [This](https://arxiv.org/abs/1507.04296) paper literally answers that
  question. Although how popular parallel implementations of DQN are, I'm not
  sure. Asynchronous methods for Policy Gradients seem more the norm when
  talking about parallel RL. Need to confirm intuition though
    - Their algo, called Gorila, has a few variants but here's a representative
      example- multiple actors each with it's own local replay memory and
      instances of the QNetwork. The QNetworks learn from the local memory and
      sends gradient updates to a parameter server which maintains
      the gold standard QNetwork. Periodically, the param server synchronizes
      with all the local networks.





