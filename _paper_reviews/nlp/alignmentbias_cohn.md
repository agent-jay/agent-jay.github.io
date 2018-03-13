---
title: Incorporating Structural Alignment Biases into an Attentional Neural Translation Model
date: 2017-11-15
category: Natural Language Processing
tag: [NLP, NMT, Attention, RNN, Seq2Seq]
---
*Trevor Cohn, Cong Duy Vu Hoang, Ekaterina Vymolova, Kaisheng Yao, Chris Dyer, Gholamreza Haffar*

[Link to paper](https://arxiv.org/abs/1601.01085)

## Summary

- Positional bias
    - add position of source and target word as additional input to attention
      model
- Markov condition
    - adds input that suggests monoticity between the input and output
- fertility (coverage?)
    - Local fertility: Sum of all prev. attention weights in a window
      surrounding source word
    - Global fertility: Fertility is incorporated into the loss function as a
      regularization term, that penalizes words for being being overly attended to.
      This would be implemented by producing an fi (fertility) for every word
      at the end of translation, to be added into the loss term.
- Bilingual symmetry
    - Train two models jointly - one for S->T other for T->S translation train
      them.  This is done with:
        - combining their loss terms (-ve log likelihoods)
        - A trace bonus term which tries to keep the two resulting alignment
          matrices as similar to each other as possible

All the above are inputs to the f_ji calculation that is the unnormalized
attention weight for a source word(i) for decoder step j

## Things I learned

- Fertility or coverage seems like a nice (and now common) way to exclude words
  that have already been generated or downweight them in future calculations. 
  Seems like a better way to incorporate prior attention weights when
  generating new ones, than Luong's input feeding method where attention
  weights are fed along with input variables into the RNN.
- Coming into the paper I expected an approach that would constrain the
  attention in a harsher sense. But the model tries to incorporate bias in a
  much softer manner, by just feeding the right inputs, which the authors
  believe will make the MT come up with better translations.
- For markov condition and fertility, use a window of fixed k words to make the
  weight matrix independent of size of source sentence(which is obviously
  variable)
- First time I've seen an NLP paper use a task based loss function to optimize
  (global fertility and bilingual symmetry)

## Questions

- How would positional bias (or any of the other biases) work in languages that
  don't have monotonic relations if the inputs are just positions of the source
  and target word (i,j)?
- Does the markov condition make sense for languages that don't have monotonic
  alignments?
- What are the window values at the beginning and end where there is no i-k or
  i+k respectively?
- Isn't the epsilon(alpha_j-1;i) calculation an indexing operation? How is that
  differentiable?
- In the global fertility model, how are mean and standard dev. decided?
- In the symmetry training objective, instead of the trace bonus, would it make
  sense to use cosine similarity bet attention matrix(s->t) and (t-s)
  transpose? As a measure of similarity...
- The ablation test could be more fine so we can see the relative contribution
  of each- position, markov and local fertility as well
    - Also why don't they use pretrained embeddings for everything, instead of
      just for global fertility? And aren't pretrained embeddings generally not
      useful for machine translation?

## Answers

- The non-linear function applied over this input will hopefully allow more
  complex interactions to be learned
- My guess is that it will only be useful for monotonic language pairs, and
  will be downweighted in other cases by the neural network
- My guess is that they'll be zeros. Similar to how convnet works at the
  boundaries of an image. This is an implementation decision.


