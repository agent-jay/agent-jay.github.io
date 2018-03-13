---
title: End-to-end Learning of Action Detection from Frame Glimpses in Videos
date: 2017-11-15
category: Computer Vision
tags: [Video, CNN, RNN, RL]
---
*Serena Yeung, Olga Russakovsky, Greg Mori, Li Fei-Fe*

[Link to paper](https://arxiv.org/abs/1511.06984)

## Summary

Papers I've read so far only do video classification- classify video based on
activity, assume there is generally one activity in the video. 

This paper proposes a method to identify not only whether an action occurs, but
also where it does. Prior work involves using frame/segment level analysis at
multiple time scales. This is end-to-end.


Architecture:
Inspired by [RAM model]({{ site.base_url }}{% link _paper_reviews/computer_vision/ram_mnih.md %})
- observation network
    - recieves location (in-time) and video frame. Video frame is processed by
      CNN (VGG net features), location by another net-> combined and again
      passed through fully connected net.
    - output of this net is directly passed as input to the recurrent net
- recurrent network
    - recieves processed input from observation network, prev. hidden state
    - outputs:
        - candidate detection
            - network that outputs tuple(start time, end time, confidence)
        - indicator that says whether to emit candidate detection at that
          timestep
            - network that parameterizes Bernoulli dist. that is sampled from.
              During test time, MAP estimate is used
        - location of next frame to observe
            - network that parameterizes Gaussian distribution's mean (with
              fixed variance) that is sampled from. During test time, MAP
              estimate is used.

Training:
- Uses backprop
    - Candidate prediction
        - Loss function boosts confidence score for detection that is matched to ground truth label
        - Loss function (L2 norm) penalizes if start/end times of detection
          window is close to that of ground truth

- Uses REINFORCE
    - Indicator prediction
    - Location prediction
    - The reward function:
        - penalizes the agent for being conservative (not outputting predictions)
        - rewards positive detections, penalizes negative 
            - defn. of positive- overlap with ground truth above a certain
              threshold(hyperparam)

## Things I learned

- Training is done as 1-vs-all for each class. Seems like the way to go when it
comes to activity detection/video classification(sometimes), does seem to give
a performance bump.
- Why do you need REINFORCE if you could reparameterize any sampling steps?
    - Even if sampling was differentiable, the indexing isn't
    - Selecting an image(indexed from 1-T) that is discrete is not differentiable
    - A good way to think of why this is not differentiable is to think of the DRAW
      paper, there they make the attention differentiable by using a gaussian
      filter over the entire frame as opposed to an indexing operation. Hard
      attention vs soft attention
  
## Questions

- Why doesn't prediction indicator depend on candidate detection confidence
  score?
    - Their relation is incorporated into the loss function
- How many steps does it run for? Can this be made dynamic if it isn't already?
    - N is chosen beforehand=6
- Is a single location transformed into a 1024 dimensional vector? Does that
  make sense?
- How exactly is ground truth labeled? And if the RNN makes multiple
  predictions (say with indicator being +ve), and the candidate predictions are
  all active too, how .
- What makes the tuple(start,end,conf) bounded to the right values, say [0,1]
    - probably clipped. Doesn't say
- Doesn't the matching function's definition make sure that *every* candidate
  detection *will* be matched to some ground truth value, no matter if they are
  even close to each other?
- How are there multiple ground truth predictions- does it mean they aren't
  contiguous?
- Intuitively this model doesn't make sense, doesn't function like a human
  would. How is a glimpse enough to make predictions for a video it hasn't even
  seen yet.
    - this model is learning to find and leverage some bias in the training data, that
    each class has some very specific intervals and patterns. Might generalize
    poorly to real/out of training videos
    - assumption of start, end times that are being normalized
        - fixed length videos
        - no constraints that end has to be greater than start. Doesn't look like
        they do encode it in the model
- Is this actually actor-critic? Since they compute *approximate gradient* and
  also use the advantage function to reduce bias. So 2 function approximators..
- How efficient is this model both in training and testing? They say they use
  2% of total frames but in other ways is it efficient
- Does REINFORCE make sense for this problem(as opposed to some other Q
  learning)? As in, in order to learn a good policy, it needs to actually
  randomly stumble into a good one.
    - What is the use case for REINFORCE, does it suit itself to a certain
      class of problems where the state/action space is relatively constrained?
      I can imagine it taking forever in more complicated scenarios?
    - How to encourage efficient parameter exploration? It feels like most of
      the magic here is in the reward function





