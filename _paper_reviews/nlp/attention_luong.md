---
title: Effective Approaches to Attention-based Neural Machine Translation
date: 2017-11-15
category: Natural Language Processing
tags: [Attention, NLP, NMT, Seq2Seq, RNN]
---
*Minh-Thang Luong, Hieu Pham, Christopher D. Manning*

[Link to paper](https://arxiv.org/abs/1508.04025)

## Summary

Two variants-
- global attention 
- local attention

Global attention is different from Bahdanau attention in a couple ways:
- Uses stacked forward RNNs instead of BiRNN
    - Attention is computed over the uppermost hidden states instead of
      concatenated backward/forward hidden states
- Hidden state of decoder is computed pre-attention
    - ht ->at ->ct ->h’t ->prediction, vs
    - ht-1 ->at ->ct ->ht ->prediction
- They try 3 different alignment functions - innerproduct (cosine similarity),
  bilinear and concat. Bahdanau uses only concat.

Input feeding- 
Feed the h_bar along with(concatenated) prev. predicted word embedding, as
input to the next step of the decoder This only seems like the sensible thing
to do given that the hidden state doesn't depend on attention.

Local attention- a mix between soft and hard attention, 
    - cheaper to compute than soft attention(bahdanau)
    - differentiable unlike hard attention(Xu et al)
Basically limit the attention to a subset [pt-D, pt+D]. Pt can be chosen to be
t (monotonicity assumption) or can be learned as part of the neural net.

## Things I learned

- Variants of attention. I've seen these before in a couple different places
- A simple way to add bias in the model

## Questions

- Why is the hidden state computed before attention? One would think using the
  attention for the hidden state calculation would be more useful. *If* input
  feeding isn't used, I think it's more efficient *during training*, since the
  hidden state of the decoder can be calculated independent of attention.

## Answers



