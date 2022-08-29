pixelNeRF: Neural Radiance Fields from One or Few Images
========================================================

* Authors: Alex Yu, Vickie Ye, Matthew Tancik, Angjoo Kanazawa
* Affiliations: UC Berkeley
* CVPR, 2021
* Links: `project <https://alexyu.net/pixelnerf/>`_

Summary
-------

Existing approaches invole optimizing the representation to every scene independently, requiring many calibrated view and significantly compute time. In this work, the authors proposed pixelNeRF, a learning framework that predicts a continuous neural scene representation conditioned on one or few input images. Experiments on ShapeNet and DTU datasets show that pixelNeRF outperforms current state-of-the-art baselines for novel view synthesis and single image 3D reconstruction.

Key Ideas
---------

**Standard NeRF.** In a standard NeRF :math:`f`, given a 3D point :math:`\mathbf{x} \in \mathbb{R}^3` and viewing direction :math:`\mathbf{d} \in \mathbb{R}^3`, :math:`f` returns a differential density :math:`\sigma` and RGB color :math:`\mathbf{c}`: :math:`f(\mathbf{x}, \mathbf{c}) = (\sigma, \mathbf{c})`.

The volumetric radiance field can then be rendered into a 2D image via:

.. math::
    \hat{\mathbf{C}} = \int_{t_n}^{t_f} T(t)\sigma(t)\mathbf{c}(t)dt

where :math:`T(t) = \exp(-\int_{t_n}^t \sigma(s) ds)` handles occlusion.

**Image-conditioned NeRF.** Given a input image :math:`I` of a scene, we extract a feature volume :math:`W = E(I)`. For a point on a camera ray :math:`x`, we retrieve the image feature by projecting :math:`x` onto thte image plane and obtain the feature vector :math:`W(\pi(x))` with bilinear interpolation. Then the image features are passed into the NeRF network as

.. math::
    f(\gamma(x), d; W(\pi(x))) = (\sigma, c)

The image features are incorporated as a residual at each layer. The pipeline is depicted below.

.. figure::
    figures/pixelnerf-1.png
    :height: 180px

    Figure 1: Overview of pixelNeRF.

Technical Details
-----------------

**Incorporating multiple view.** The model can be extended to allow for an arbitrary number of views at test time. Let the :math:`i` input image be :math:`I^{(i)}` and the associated camera transform from the world space to its view space be :math:`P^{(i)} = [R^{(i)}, t^{(i)}]`. Let the initial layers in the NeRF network be :math:`f_1` and the final layers be :math:`f_2`. We obtain intermediate vectors from the initial layers and then aggregate with average pooling operator :math:`\phi`:

.. math::
    \begin{align*}
    V^{(i)} & = f_1 \left( \gamma(x^{(i)}), d^{(i)}; W^{(i)} (\pi(x^{(i)})) \right) \\
    (\gamma, c) & = f_2 (\phi(V^{(i)}, \dots, V^{(n)}))
    \end{align*}

**Image encoder.** To capture both local and global information, a feature pyramid is extracted. Then the image features are added as a residual at the beginning of each ResNet block.

**Category-agnostic single-view reconstruction.**

.. figure::
    figures/pixelnerf-2.png
    :height: 180px

    Figure 2: Quantitative results on category-agnostic single-view reconstruction.

Notes
-----

References
----------

[1] A. Yu, V. Ye, M. Tancik, A. Kanazawa. `"pixelNeRF: Neural radiance fields from one or few images." <https://arxiv.org/abs/2012.02190>`_. In *CVPR*, 2021.
