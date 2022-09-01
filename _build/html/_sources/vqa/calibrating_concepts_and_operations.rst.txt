Calibrating Concepts and Operations: Towards Symbolic Reasoning on Real Images
========================================================

* Authors: Zhuowan Li, Elias Stengel-Eskin, Yixiao Zhang, Cihang Xie, Quan Tran, Benjamin Van Durme, Alan Yuille
* Affiliations: Johns Hopkins University
* ICCV, 2021
* Links: `arXiv <https://arxiv.org/abs/2110.00519>`_

Summary
-------

In this work, the authors identified that the long-tail distribution of visual concepts are unequal importance of reasoning steps in real data are two key obstacles that limit neural symbolic methods to generalize to real-world datasets. The authors proposed a new paradigm, **Calibrating Concepts and Operations (CCO)**, which enables neural symbolic models to capture underlying data characteristics and to reason with hierarchical importance. Experiments showed CCO substantially boosts the performance of neural symbolic methods on real images from GQA.

Key Ideas
---------

**Neural symbolic methods perform poorly on real-world datasets.** For instance, NSCL achieves 98.9% accuracy on CLEVR, but only 47.0% on GQA. There are two major differences bewteen synthetic datasets and real-world datasets:

1. Visual concepts are well balanced in synthetic datasets but follow a long-tail distribution in real-world datasets.
2. Unlike in synthetic data, the reasoning steps on real data have varying importance due to redundancy/over-specification in question description.

.. figure::
    figures/calibrating_concepts_and_operations-1.png
    :height: 220px

    Figure 1: Two key differences between synthetic datasets and real-world datasets.

**Calibrating concepts and operations (CCO).** CCO includes a concept calibration module and an operation calibration module.

* **Calibrating concepts.** Noticing that the magnitude of each concept embedding is informative, the authors proposed to explicitly learn different embedding magnitudes for each module type. The learned norm sizes can encode the concept distribution:
  
    .. math::
        c_\text{concept} = w_\text{concept}^\text{type} c_\text{concept}

* **Calibrating operations.** It is important for th model to reason with different operation importance. A bi-directional LSTM weight predictor is used to predict operation weights based on the whole program.

.. figure::
    figures/calibrating_concepts_and_operations-2.png
    :height: 340px

    Figure 2: Overview of CCO.

Technical Details
-----------------

**Normalized concept embedding.** Removing the normalization of concept embeddings leads to a performance improvemnet of :math:`+3.4%`. The learned magnitude for each concept embedding is strongly correlated with concept frequency in real-world datasets.

**Accuracy comparison on the balanced GQA test-dev split.**

.. figure::
    figures/calibrating_concepts_and_operations-3.png
    :height: 120px

    Figure 3: Accuracy comparison on the balanced GQA test-dev split.

**A positive correlation between learned embedding magnitude and concept frequency.**

.. figure::
    figures/calibrating_concepts_and_operations-4.png
    :height: 240px

    Figure 4: A positive correlation between learned embedding magnitude and concept frequency.

Notes
-----

References
----------

[1] Z. Li, E. Stengel-Eskin, Y. Zhang, C. Xie, Q. Tran, B. Van Durme, A. Yuille. `"Calibrating concepts and operations: Towards symbolic reasoning on real images." <https://arxiv.org/abs/2110.00519>`_. In *ICCV*, 2021.
