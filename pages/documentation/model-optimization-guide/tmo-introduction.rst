.. index:: pair: page; Optimizing Models at Training Time
.. _optim_perf__tmo_introduction:

.. meta::
   :description: 
                 
   :keywords: 
              


Optimizing Models at Training Time
==================================

:target:`optim_perf__tmo_introduction_1md_openvino_docs_optimization_guide_optim_perf__tmo_introduction`

.. toctree::
   :maxdepth: 1
   :hidden:

   ./tmo-introduction/qat-introduction
   ./tmo-introduction/filter-pruning


Introduction
~~~~~~~~~~~~

Training-time model optimization is a way to get a more efficient and HW-friendly model when applying optimization 
methods with fine-tuning. It can help when the :ref:`post-training optimization <optim_perf__pot_intro>` does not provide the 
desired accuracy or performance results. OpenVINO does not have training capabilities but it provides a Neural 
Network Compression Framework (NNCF) tool that can be used to integrate training-time optimizations supported by OpenVINO 
in the training scripts created using source frameworks, such as PyTorch or TensorFlow 2.

To apply training-time optimization methods you need typical artefacts used to train model:

* A floating-point model in the framework representation.

* Training script written with framework API.

* Training and validation datasets.

Figure below shows a common workflow of applying training-time optimizations with NNCF.

.. image:: ./_assets/nncf_workflow.png


Optimization methods
~~~~~~~~~~~~~~~~~~~~

There are two methods available to improve model performance with OpenVINO:

* **8-bit uniform quantization** (or simply quantization) is a technique that allows moving from floating-point precision 
  to 8-bit integer precision for weights and activations during the inference time. It helps to reduce the model size, memory 
  footprint and latency as well as improve the computational efficiency using integer arithmetic. During the quantization 
  process the model undergoes the transformation process when additional operations, that contain quantization information, 
  are inserted into the model. However, the model continues to be the floating-point and can be fine-tuned to restore the 
  accuracy degradation introduced by the quantization the same way as the original model. This procedure is called 
  Quantization-aware Training (QAT). The actual transition to integer arithmetic happens at model inference.

* **Structured pruning** is used to remove unnecessary or redundant groups of weights from Deep Neural Networks. In the case 
  of Convolutional Neural Networks it usually results in **Filter Pruning** where the whole convolutional filters are being 
  removed from the model reducing the model size and footprint as well as overall computational complexity. This process 
  consists of two steps: 1. search and zero out redundant filters along with model fine-tuning 2. remove zero filters after 
  the fine-tuning. Since this method changes the model structure it usually requires a long fine-tuning or even retraining 
  depending on the pruning ratio.


Recommended workflow:
~~~~~~~~~~~~~~~~~~~~~

Based on the complexity and ease of use of the optimization methods, we recommend the following workflow to accelerate 
the inference with the fine-tuning.

* **Quantization-aware Training (QAT)** to get fast and accurate results with significant improvement in the inference 
  performance. Currently, a HW-compatible (CPU, GPU, VPU) QAT for 8-bit inference is available. For details, see 
  :ref:`Quantization-aware Training <optim_perf__qat_introduction>` documentation.

* **Filter Pruning**, used to get additional speedup on top of quantization. For details, see 
  :ref:`Filter Pruning <optim_perf__filter_pruning>` documentation.


Installation
~~~~~~~~~~~~

NNCF is open-sourced on `GitHub <https://github.com/openvinotoolkit/nncf>`__ and distributed as a separate package. It 
is also available on PyPI. We recommend installing it to the Python environment where the framework is installed.


Install from PyPI
-----------------

To install the latest released version via pip manager run the following command:

.. ref-code-block:: cpp

	pip install nncf



.. note:: 
   To install with specific frameworks, use the `pip install nncf[extras]` command, where extras is a list of possible extras, 
   for example, `torch`, `tf`, `onnx`.

To install the latest NNCF version from source follow the instruction on `GitHub <https://github.com/openvinotoolkit/nncf#installation>`__.

.. note:: 
   NNCF does not have OpenVINO as an installation requirement. To deploy optimized models you should install 
   OpenVINO separately.

See also
~~~~~~~~

* :ref:`Post-training Optimization <optim_perf__pot_intro>`

