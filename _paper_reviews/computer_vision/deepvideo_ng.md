---
title: "Beyond Short Snippets: Deep Networks for Video Classification"
date: 2017-11-15
category: Computer Vision
tags: [Video, CNN, RNN]
---
*Joe Yue-Hei Ng, Matthew Hausknecht, Sudheendra Vijayanarasimhan, Oriol Vinyals, Rajat Monga, George Toderici*

[Link to paper](https://arxiv.org/abs/1503.08909)
## Summary

This paper tries to answer the question of how to process Variable length
sequences in a more principled way. The authors suggest three methods:
- temporal feature pooling with convnets 
- temporal feature processing with RNNs
- both of the above with optical flow

Pooling isn't really all that different from the method that came before it
(single frame features averaged over a timespan) except that here, the pooling
is part of the neural network and is followed by another neural network. This
means that temporal information feeds back into frame level features. 
It doesn't really work with any length sequence (they cap it
at 30 or 120, anything longer and they revert to averaging the outputs across
batches. 

On the plus side- each frame can be processed in parallel and the outputs are
nearly as good as the LSTM


## Things I learned

- Max pooling is better than average pooling because of sparse gradient
updates- only certain subsets of the network are updated at any given time (the
one that produced max), so the network learns faster



## Questions

- What do they mean by network expansion?
- If they make 30/120 copies of a  huge network, how do they store that many
copies in memory? Each of them has a different activation that needs to be
stored to compute gradients...
- How come RNNs which are built for sequential data aren't much better than
  CNNs?

## Answers

- Network expansion- it's like a pretraining using a single network that makes
  predictions per frame (with averaged scores across frames?). This network is
  then cloned *frame* number of times for the actual feature pooling models.
