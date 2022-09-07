.. index:: pair: page; Convert a TensorFlow Attention OCR Model
.. _conv_prep__conv_tensorflow_attention_ocr:

.. meta::
   :description: This tutorial demonstrates how to convert the Attention OCR 
                 model from the TensorFlow Attention OCR repository to the 
                 OpenVINO Intermediate Representation.
   :keywords: Model Optimizer, tutorial, convert a model, model conversion, 
              --input_model, --input_model parameter, command-line parameter, 
              OpenVINO™ toolkit, deep learning inference, OpenVINO Intermediate 
              Representation, TensorFlow, AOCR, TensorFlow Attention OCR, 

Convert a TensorFlow Attention OCR Model
========================================

:target:`conv_prep__conv_tensorflow_attention_ocr_1md_openvino_docs_mo_dg_prepare_model_convert_model_tf_specific_convert_attentionocr_from_tensorflow` 

This tutorial explains how to convert the Attention OCR (AOCR) model from the `TensorFlow Attention OCR repository <https://github.com/emedvedev/attention-ocr>`__ to the Intermediate Representation (IR).

Extract a model from library
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To get an AOCR model, download ``aocr`` Python library:

.. ref-code-block:: cpp

	pip install git+https://github.com/emedvedev/attention-ocr.git@master#egg=aocr

This library contains a pre-trained model and allows training and running AOCR, using the command line. After installation of ``aocr``, extract the model:

.. ref-code-block:: cpp

	aocr export --format=frozengraph model/path/

Once extracted, the model can be found in ``model/path/`` folder.

Convert the TensorFlow AOCR model to IR
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The original AOCR model includes the preprocessing data, which contains:

* Decoding input data to binary format where input data is an image represented as a string.

* Resizing binary image to working resolution.

The resized image is sent to the convolution neural network (CNN). Because Model Optimizer does not support image decoding, the preprocessing part of the model should be cut off, using the ``--input`` command-line parameter.

.. ref-code-block:: cpp

	mo \
	--input_model=model/path/frozen_graph.pb \
	--input="map/TensorArrayStack/TensorArrayGatherV3:0[1 32 86 1]" \
	--output "transpose_1,transpose_2" \
	--output_dir path/to/ir/

Where:

* ``map/TensorArrayStack/TensorArrayGatherV3:0[1 32 86 1]`` - name of node producing tensor after preprocessing.

* ``transpose_1`` - name of the node producing tensor with predicted characters.

* ``transpose_2`` - name of the node producing tensor with predicted characters probabilities.

