---
title: 'A Hypothesis about Learning Curve of BERT'
date: 2021-07-19

permalink: /posts/2021/07/my-first-post/
tags:
  - cool posts
  - category1
  - category2
---

This post proposes a hypothesis about learning curve of BERT, based on [an analysis about BERT's attention](https://nlp.stanford.edu/pubs/clark2019what.pdf). As mentioned in the analysis, heads often focus on special tokens: early heads attend to \[CLS], middle heads attend to \[SEP], and deep heads attend to periods and commas. Intuitively, we hypothesize there are three rough phases of heads respectively learning three different properties: definition, dependency, and finer-grained Segment. We also suggest that through pre-training the special tokens are systematically defined as special part of speech: \[CLS] as hunter, \[SEP] as separator, periods and commas as loner.

Special Part of Speech
======
For better understanding, we first consider what makes the tokens (\[CLS], \[SEP], periods and sommas) special. Firstly, high word frequency makes the learning easier and earlier. Secondly, they are outsiders of the sentence, since it is hard to find relations between the special tokens and the others. While other frequent tokens are insiders with simple relations: for example, articles (a/an/the) are often related to immediately following word. In gereral, special tokens are frequent outsiders. When looking into them, we find they are like the frogs in the well with different destiny:

* \[CLS] is motivated by the Next Sentence Prediction (NSP) task to attend broadly to aggregate a representation, which makes it a global hunter: the frog is pushed to leave its small environment and explore the wide world.
* \[SEP] is fixed at the end of sentences like the frog in the fixed well.
* Typical, the "sentences" refered by BERT are much longer than single sentences. Periods and commas are separetors for traditional sentences inside the "sentences". We name them loner for differentiation. Compared to \[SEP], position of loner is more flexible like the frog in the moving well.

Phase One
======
In phase one, the early heads attend broadly to get information of other tokens, which helps define the usage or part of speech of current tokens. Attention to \[CLS] is obviously higher, probably because it is a hunter: the heads attend to it for imformation about other tokens instead of the hunter itself.

Phase Two
======
After clarifying the definition, middle heads focus on dependency parsing. According to the analysis, \[SEP] draws over half of attention to itself in layer 6-10, which is used as a no-op for attention heads. While no single attention head perform well at syntax "overall", attention heads sepcialize to specific dependency relation (e.g., pobj, det, and dobj), especially for heads from layer 4-9. Based on these findings, we suggest that \[SEP] is used as a token separator which separate specific tokens from the others: for example, when certain heads specialize in direct object (dobj), the verb and following object are most important while other tokens would better not be functioning (no-op). \[SEP] provides a suitable place for these no-op tokens to lie low, since \[SEP] is hardly related to other tokens like the frog in the well. With high potential, a question may be raised: why not attend to loner (periods and commas), since loner is also the frog in the well. Probably, the difference of their well is the main reason: it is easier to find the frog in the fixed well instead of the moving one. 

Phase Three
======