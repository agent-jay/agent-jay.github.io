---
title: Good Question! Statistical Ranking for Question Generation 
date: 2017-11-15
category: Natural Language Processing
tags: NLP
---
*Michael Heilman Noah A. Smith*

[Link to paper](http://www.aclweb.org/anthology/N10-1086)

## Summary

The [Learning to Ask](https://arxiv.org/abs/1705.00106) paper uses this as a baseline. This paper came out in 2010
before the current wave of neural approaches- embeddings, rnns etc.

Basic idea is to successively transform using rules, a declarative source
sentence into a simpler sentence, and then into a question phrase. Does not
even attempt complex queries requiring multiple sentences. As there are many
rules, multiple questions can be generated from the same sentence. A logistic
regression model is trained to judge the quality of these questions and
eliminate bad ones. Essentially a feature based classifier. The authors refer
to this as the overgenerate and rank approach.

## Things I learned
- A rule based system is **hard**. This looks difficult, time consuming work.
  And difficult to scale and test as with most rule based systems. But I
  suppose it's good as a baseline.
- The transformation implicitly determines the type of question asked ie. the
  quality of the question asked relies on the strength of transformation
- Transformations are done using a language called Tregex, which works on
  phrase structure trees produced by the Stanford Parser.
- Every sentence is guaranteed to have multiple types of questions due to the
  rule based sentence-question transformation. Not so in the Learning To Ask
  paradigm.
- Question answer datasets were far smaller then. Neural approaches are
  possibly only feasible now.


## Questions
- Really need to understand what exactly a parser is. What are POS tags,
  category labels?
- What is a dependency tree? Is it the same as above?
- How do neural learning to rank models work? Is it just a name given to
  scoring a bunch of items using logistic function and then ranking them?

