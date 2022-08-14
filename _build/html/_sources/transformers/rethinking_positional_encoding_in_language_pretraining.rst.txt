Rethinking Positional Encoding in Language Pre-training
=======================================================

* Authors: Guolin Ke, Di He, Tie-Yan Liu
* Affiliations: Microsoft Research

Key Ideas
---------

Usually in transformers (e.g., BERT), we add the positional encoding to the word embeddings, which brings mixed correlations between two heterogeneous information resources.

1. The attention includes two unwanted terms involving the dot product between positional embeddings and word embeddings.
2. The class embedding :code:`[CLS]` will be biased towards the word tokens nearby.
