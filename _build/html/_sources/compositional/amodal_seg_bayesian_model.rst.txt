Amodal Segmentation through Out-of-Task and Out-of-Distribution with a Bayesian Model
=====================================================================================

* Authors: Yihong Sun, Adam Kortylewski, Alan Yuille
* Affiliations: Johns Hopkins University
* CVPR, 2022
* Links: `project <https://arxiv.org/abs/2010.13175>`_

Summary
-------

Amodal segmentation aims to segment object boundaries which are occluded and hence invisible. The authors formulate amodal segmentation as an out-of-task and out-of-distribution generalization problem. The model is trained from non-occluded images using bounding box annotations and class labels only, but is applied to generalize out-of-task to object segmentation and to generalize out-of-distribution to segment occluded objects. The proposed approach outperfoms alternate methods with the same supervision by a wide margin, and even outperforms supervised methods when the occlusion is large.

Key Ideas
---------

The model taks as input a feature map :math:`\bar{F} = \psi(I, \eta)` where :math:`I` is the input image and :math:`\zeta` are the neural network weights. Let the features within a given bounding box :math:`\mathcal{D}` be :math:`F = \{ f_a: a \in \mathcal{D} \}`.

**A Bayesian model for amodal segmentation.** The authors introduce a latent variable :math:`w_a \in \{0, 1\}` to indicate foreground/background which are learnt without additional supervision.

.. math::
    P_a(f_a \mid y, m, w_a) = P_a(f_a \mid y, m)^{w_a} B_a(f_a \mid y, m)^{1-w_a} \times P_a(w_a \mid y, m)

The foreground and background model :math:`P_a(f_a \mid y, m), B_a(f_a \mid y, m)` are mixtures of von Mises Fisher distributions.

**Shape modeling.** The authors also introduce shape priors :math:`P(\overrightarrow{w} \mid y, m) = \prod_{a \in \mathcal{D} P_a(w_a \mid y, m)`, a learned 2D spatial map conditioned on the object category :math:`y` and the class mixture :math:`m`. Finally this gives a generative model of this data:

.. math::
    P(F \mid y) = \sum_{m, \overrightarrow{w}} P(F \mid y, m, \overrightarrow{w}) P(m) P(\overrightarrow{w} \mid y, m)

Technical Details
-----------------

Notes
-----

References
----------

[1] Y. Sun, A. Kortylewski, A. Yuille. `"Amodal segmentation through out-of-task and out-of-distribution with a bayesian model." <https://arxiv.org/abs/2012.02190>`_. In *CVPR*, 2021.
