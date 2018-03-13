---
title: Bidirection Attention Flow for Machine Comprehension
date: 2017-11-15
category: Natural Language Processing
tags: [NLP, Attention, RNN]
---
*Minjoon Seo, Aniruddha Kembhavi, Ali Farhadi, Hananneh Hajishirzi*

[Link to paper](https://arxiv.org/abs/1611.01603)

## Summary

BiDAF is a fairly popular machine comprehension methods for question answering,
and was for a time a state of the art on the SQuAD leaderboard.  The methods it
follows are reflective of a common approach used on SQuAD. Also popular as
it's one of the few methods that have their source code available. Other papers
added extensions or modified the methods introduced in this paper. eg. FastQA

Contribution of the paper is to use a matrix of attentions. Similar to
decomposable attention models for textual entailment(verify). This isn't used
directly however, a context2query and query2context layer use this as input to
create different representations. 


## Things I learned

- There's a mistake in the paper in the query2context layer. The max is
  performed over rows, not columns. The same error is present in the
  architecture diagram as well.
- Different approaches to attention or more accurately different definitions of
  content based similarity.
    - Dot product (Hill 16)
    - Linear 
    - BiLinear (Chen 16)
    - Linear after MLP (Hermann et al)
- Another *really* interesting thing I found out about, not on the paper but
  from AI2's podcast on BIDAF was about how the ensemble models on SQuAD work.
  Most models on the leaderboard have a *single* and *ensemble* model and
  apparently all of them use this strategy to create this ensemble- use the
  *same* architecture but with different random seeds, and then use some form
  of voting    
    - *We also train an ensemble model consisting of 12 training runs with the
      identical architecture and hyper-parameters.  At test time, we choose the
      answer (span) with the highest sum of confidence scores amongst the 12 runs for
      each question*
- The architecture is quite complex, with lots of layers. Seriously, read the
  paper. The highway layer, modeling layer and the bidirectional lstm before
  the second output unit felt like they were extra. 
- Another thing they mention on the AI2 podcast is the FastQA paper, on how to
  simplify the BiDAF architecture. Should take a look to see how much is
  redundant or how much can you get away with removing.

## Questions
- How *exactly* does a one dimensional CNN work? 
    - [Denny Britz's blog on CNNs for NLP](http://www.wildml.com/2015/11/understanding-convolutional-neural-networks-for-nlp/)
        - Read the code and paper becuase the blog is iffy on certain details
