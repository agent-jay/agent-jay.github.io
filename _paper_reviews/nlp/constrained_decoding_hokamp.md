---
title: Lexically Constrained Decoding for Sequence Generation Using Grid Beam Search
date: 2017-11-15
category: Natural Language Processing
tags: [NLP, RNN]
---
*Chris Hokamp, Qun Liu*

[Link to paper](https://arxiv.org/abs/1704.07138)

## Summary

Introduces Grid Beam Search- a way to to generate sequences conditioned on some
input (for machine translation, but essentially any seq2seq model) as well as
*lexical constraints* ie. a set of tokens that have to be present in the target
sentence.

The paper uses it in the context of interactive machine translation, which is
an iterative, human-in-the loop type of MT.

## Things I learned

- OOV words are handled during test time using subword units? This is definitely a
  problem one can expect to face in Machine Comprehension. But this solution
  makes more sense for Machine Translation
    - "*Subword representations provide an elegant way to circumvent this
      problem, by breaking unknown or rare tokens into character n-grams which
      are part of the model’s vocabulary (Sennrich et al., 2016; Wu et al.,
      2016).*"

## Questions

- What happens if a sentence is required to repeat certain words which are part
  of the constraints. If I'm right, the algorithm doesn't allow repetition of
  constraint words Is this realistic for MT? How about for Question Generation?
    - My naive guess is that it isn't possible to generate such sentences. Need
      to reason this out with the algorithm
- What enforces a token in the constraint to be in the final result? Line 22 is
  where the assignment to the beam happens. Doesn't look like a chosen token
  *has* to be from the constraint list
