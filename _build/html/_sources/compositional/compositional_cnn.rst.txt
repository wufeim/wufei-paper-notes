Compositional Convolutional Neural Networks: A Deep Architecture with Innate Robustness to Partial Occlusion
============================================================================================================

* Authors: Adam Kortylewski, Ju He, Qing Liu, Alan Yuille
* Affiliations: Johns Hopkins University
* CVPR, 2020
* Links: `arXiv <https://arxiv.org/abs/2003.04490>`_, `thecvf.com <https://openaccess.thecvf.com/content_CVPR_2020/html/Kortylewski_Compositional_Convolutional_Neural_Networks_A_Deep_Architecture_With_Innate_Robustness_CVPR_2020_paper.html>`_

Summary
-------

Recent findings show that deep convolutional neural networks (DCNNs) do not generalize well under partial occlusion. The authors propose to integrate compositional models and DCNNs into a unified deep model with innate robustness to partial occlusion, termed **Compositional CNN**.

Results show that DCNNs do not classify occluded objects robustly, even when trained with data that is strongly augmented with partial occlusions. The proposed model, CompositionalNets, outperforms standard DNNs by a wide margin at classifying partially occluded objects, even when it has not been exposed to occluded objects during training.

Key Ideas
---------

**Fully generative compositional models.** Let :math:`F^l \in \mathbb{R}^{H \times W \times D}` be the output of a layer :math:`l` in a DCNN. The authors proposed a differentiable generative compositional model of the feature activations :math:`p(F \mid y)` for an object class :math:`y`, which is modeled as a mixture of von-Mises-Fisher (vMF) distributions:

.. math::
    \begin{align*}
    p(F \mid \theta_y) & = \prod_p p(f_p \mid \mathcal{A}_{p, y}, \Lambda) \\
    p(f_p \mid \mathcal{A}_{p, y}, \Lambda) & = \sum_k \alpha_{p, k, y}p(f_p \mid \lambda_k)
    \end{align*}

where :math:`\theta_k = \{ \mathcal{A}_y, \Lambda\}` are the model parameters and :math:`\mathcal{A}_y = \{\mathcal{A}_{p, y}\}` are the parameters of the mixture models at every position :math:`p \in \mathcal{P}` on the 2D lattice. In particular, :math:`\mathcal{A}_{p, y} = \{\alpha_{p,0,y}, \dots, \alpha_{p, K, y} \mid \sum_{k=0}^K \alpha_{p, k, y} = 1\}` are the mixture coefficients and :math:`\Lambda = \{\lambda_k = \{\sigma_k, \mu_k\} \mid k = 1, \dots, K\}` are the parameters of the vMF distribution:

.. math::
    p(f_p \mid \lambda_k) = \frac{e^{\sigma_k \mu_k^\top f_p}}{Z(\sigma_k)}, \lVert f_p \rVert = 1, \lVert \mu_k \rVert = 1

where :math:`Z(\sigma_k)` is the normalization constant.

**Occlusion reasoning.** Compositional models can be augmented with an occlusion model at each position :math:`p`, either the object model :math:`p(f_p \mid \mathcal{A}_{p, y}^m, \Lambda)` or the occluder model :math:`p(f_p \mid \beta, \Lambda)` is active:

.. math::
    \begin{align*}
    p(F \mid \theta_y^m, \beta) & = \prod_p p(f_p, z_p^m = 0)^{1-z_p^m} p(f_p, z_p^m 1)^{z_p^m} \\
    p(f_p, z_p^m = 1) & = p(f_p \mid \beta, \Lambda) p(z_p^m = 1) \\
    p(f_p, z_p^m = 0) & = p(f_p \mid \mathcal{A}_{p, y}^m, \Lambda) p(z_p^m = 0)
    \end{align*}

where :math:`\mathcal{Z}^m = \{z_p^m \in \{0, 1\} \mid p \in \mathcal{P}\}`.

.. figure::
    figures/compositional_cnn-1.png
    :height: 220px

    Figure 1: Feed-forward inference with a CompositionalNet.

Technical Details
-----------------

**Inference as feed-forward neural network.** The computational graph of the fully generative compositional model is directed and acyclic, and can be inferenced with a single forward pass.

**End-to-end training of CompositionalNets.** The model is fully differentiable and can be trained end-to-end using backpropagation. The loss function is composed of four terms

.. math::
    \mathcal{L}(y, y', F, T) = \mathcal{L}_\text{class}(y, y') + \gamma_1 \mathcal{L}_\text{weight}(\omega) + \gamma_2 \mathcal{L}_\text{vwf}(F, \Lambda) + \gamma_3 \mathcal{L}_\text{mix}(F, \mathcal{A}_y)

where :math:`\mathcal{L}_\text{class}` is the cross-entropy loss between the network output :math:`y'` and the true class label :math:`y`, :math:`\mathcal{L}_\text{weight}` is the weight regularization on the DCNN parameters, :math:`\mathcal{L}_\text{vmf}` and :math:`\mathcal{L}_\text{mix}` regularize the parameters of the compositional model to have maximum likelihood for the features in :math:`F`.

**Classification results for vehicles of PASCAL3D+ with different levels of artificial occlusion.**

.. figure::
    figures/compositional_cnn-2.png
    :height: 200px

    Figure 2: Classification results for vehicles of PASCAL3D+ with different levels of artificial occlusion.

**Classification results for vehicles of MS-COCO with different levels of real occlusion.**

.. figure::
    figures/compositional_cnn-3.png
    :height: 180px

    Figure 3: Classification results for vehicles of MS-COCO with different levels of real occlusion.

**Occlusion localization results.**

.. figure::
    figures/compositional_cnn-4.png
    :height: 280px

    Figure 4: Occlusion localization results.

Notes
-----

References
----------

[1] A. Kortylewski, J. He, Q. Liu, A. Yuille. `"Compositional convolutional neural networks: A deep architecture with innate robustness to partial occlusion." <https://arxiv.org/abs/2003.04490>`_. In *CVPR*, 2020.
