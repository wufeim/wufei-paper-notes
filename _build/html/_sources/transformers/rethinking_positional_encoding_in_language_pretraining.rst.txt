Rethinking Positional Encoding in Language Pre-training
=======================================================

* Authors: Guolin Ke, Di He, Tie-Yan Liu
* Affiliations: Microsoft Research
* ICLR, 2021
* Links: `arXiv <https://arxiv.org/abs/2006.15595>`_, `notes <https://livejohnshopkins-my.sharepoint.com/:b:/g/personal/wma27_jh_edu/EQdQ1YCVjdBEmaaIWnoydt4BOjjCAh0Bsvxx2EuV9EqZ7w?e=y7ccg3>`_

Summary
-------

Usually in transformers (e.g., BERT), we add the positional encoding to the word embeddings, which brings mixed correlations between two heterogeneous information resources.

1. The attention includes two unwanted terms involving the dot product between positional embeddings and word embeddings.
2. The class embedding :code:`[CLS]` will be biased towards the word tokens nearby.

The authors propose a new positional encoding method called **Transformer with Untied Positional Encoding** (TUPE). Experiments and ablation studies on GLUE benchmark demonstrate the effectiveness of this method.

.. figure:: figures/rethinking_positional_encoding_in_language_pretraining-1.png
    :height: 360px

    Figure 1: The architecture of TUPE.

Key Ideas
---------

**Untie correlations between positions and words**. Consider the absolute positional encoding in the transformer. A learnable real-valued vector :math:`p_i \in \mathbb{R}^d` is added to the word embedding :math:`w_i` at position :math:`i`. The self-attention is calculated as follows:

.. math::
    \alpha_{ij}^\text{Abs} = & \frac{1}{\sqrt{d}} ((w_i + p_i)W^{Q, 1}) ((w_j + p_j)W^{K, 1})^\top \\
    = & \frac{1}{\sqrt{d}} [ (w_iW^{Q, 1}(w_jW^{K, 1})^\top) + (w_iW^{Q, 1}(p_jW^{K, 1})^\top) \\
    & + (p_iW^{Q, 1}(w_jW^{K, 1})^\top) + (p_iW^{Q, 1}(p_jW^{K, 1})^\top) ]

which includes four terms: word-to-word, word-to-position, position-to-word, and position-to-position correlations. The authors argue that the second and the third terms are unwanted. Moreover, from the figure below, the word-to-position and position-to-word correlations seem uniform across positions.

.. figure:: figures/rethinking_positional_encoding_in_language_pretraining-2.png
    :height: 150px

    Figure 2: Visualizations of four correlations: word-to-word, word-to-position, position-to-word, and position-to-position.

Therefore, the authors propose to remove the second and third terms, and the positional encoding attention are added to each layer as an attention bias.

.. figure:: figures/rethinking_positional_encoding_in_language_pretraining-3.png
    :height: 240px

    Figure 3: Untie correlations bewteen positions and words.

**Untie the class embedding from positions.** In BERT, the class embedding :code:`[CLS]` is treated just like other tokens. However, regular words often have strong local dependencies and many visualizations [2, 3] show that the attention distributions of some heads concentrate locally. Therefore, treating :code:`[CLS]` like other tokens will make :code:`[CLS]` biased to focus on the first several words instead of the whole sentence.

Therefore, the authors propose to set the correlation scores related to the :code:`[CLS]` positional encoding to a fixed learnable parameter :math:`\theta`.

.. figure:: figures/rethinking_positional_encoding_in_language_pretraining-5.png
    :height: 150px

    Figure 4: Illustration of untying :code:`[CLS]`.

Technical Details
-----------------

**Implementations.** Two variants of TUPE are developed: TUPE-A with untied absolute positional encoding, and TUPE-R with an additional relative positional encoding.

**Efficiency.** The projection matrices :math:`U^Q` and :math:`U^K` for positional embeddings are shared across layers and only increase 1% of the 110M parameters.

**Absolute and relative positional encodings are not redundant to each other.**

**Experimental results.**

.. figure:: figures/rethinking_positional_encoding_in_language_pretraining-4.png
    :height: 180px

    Figure 5: GLUE scores on dev set.

**Visualizations of learned positional correlations by TUPE-A.**

.. figure:: figures/rethinking_positional_encoding_in_language_pretraining-6.png
    :height: 130px

    Figure 6: Visualizations of learned positional correlations by TUPE-A.

Notes
-----

1. The implementations of untying positional encodings and textual embeddings is not exactly the same as explained in the paper. In BERT, the positional encodings are fused with the features and subsequent layers may learn a very complex correlation. However, in this paper, the position correlations are fixed across layers. It works fine but all the explanations do not necessarily stand.

References
----------

[1] G. Ke, D. He, T. Liu. `"Rethinking positional encoding in language pre-training." <https://arxiv.org/abs/2006.15595>`_. In *ICLR*, 2021.

[2] K. Clark, U. Khandelwal, O. Levy, C. D. Manning. `"What does BERT look at? An analysis of BERT's attention" <https://arxiv.org/abs/1906.04341>`_. In *ACL*, 2019.

[3] L. Gong, D. He, Z. Li, T. Qin, L. Wang, T. Liu. `"Efficient training of BERT by progresssively stacking" <https://proceedings.mlr.press/v97/gong19a.html>`_. In *ICML*, 2019.
