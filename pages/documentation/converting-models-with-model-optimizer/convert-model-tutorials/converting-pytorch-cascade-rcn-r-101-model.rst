.. index:: pair: page; Convert a PyTorch Cascade RCNN R-101 Model
.. _conv_prep__pytorch_cascade_rcnn:

.. meta::
   :description: This tutorial demonstrates how to convert a Cascade RCNN R-101 
                 model from Pytorch to the OpenVINO Intermediate Representation.
   :keywords: Model Optimizer, tutorial, convert a model, model conversion, 
              --input_model, --input_model parameter, command-line parameter, 
              OpenVINO™ toolkit, deep learning inference, OpenVINO Intermediate 
              Representation, Pytorch, Cascade RCNN R-101, Cascade RCNN R-101 
              model, pre-trained model, convert a model to OpenVINO IR

Convert a PyTorch Cascade RCNN R-101 Model
==========================================

:target:`conv_prep__pytorch_cascade_rcnn_1md_openvino_docs_mo_dg_prepare_model_convert_model_pytorch_specific_convert_cascade_rcnn_res101` 

The goal of this article is to present a step-by-step guide on how to convert a PyTorch Cascade RCNN R-101 model to OpenVINO IR. First, you need to download the model and convert it to ONNX.

Download and Convert Model to ONNX
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

* Clone the `repository <https://github.com/open-mmlab/mmdetection>`__ :

.. ref-code-block:: cpp

   git clone https://github.com/open-mmlab/mmdetection
   cd mmdetection

.. note:: To set up an environment, refer to the 
   `instructions <https://github.com/open-mmlab/mmdetection/blob/master/docs/en/get_started.md#installation>`__.

* Download the pre-trained `model <https://download.openmmlab.com/mmdetection/v2.0/cascade_rcnn/cascade_rcnn_r101_fpn_1x_coco/cascade_rcnn_r101_fpn_1x_coco_20200317-0b6a2fbf.pth>`__. The model is also available `here <https://github.com/open-mmlab/mmdetection/blob/master/configs/cascade_rcnn/README.md>`__.

* To convert the model to ONNX format, use this `script <https://github.com/open-mmlab/mmdetection/blob/master/tools/deployment/pytorch2onnx.py>`__.

.. ref-code-block:: cpp

   python3 tools/deployment/pytorch2onnx.py configs/cascade_rcnn/cascade_rcnn_r101_fpn_1x_coco.py cascade_rcnn_r101_fpn_1x_coco_20200317-0b6a2fbf.pth --output-file cascade_rcnn_r101_fpn_1x_coco.onnx

The script generates ONNX model file ``cascade_rcnn_r101_fpn_1x_coco.onnx`` in the directory ``tools/deployment/``. If required, specify the model name or output directory, using ``--output-file <path-to-dir>/<model-name>.onnx``.

Convert an ONNX Cascade RCNN R-101 Model to OpenVINO IR
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. ref-code-block:: cpp

   mo --input_model cascade_rcnn_r101_fpn_1x_coco.onnx --mean_values [123.675,116.28,103.53] --scale_values [58.395,57.12,57.375]

