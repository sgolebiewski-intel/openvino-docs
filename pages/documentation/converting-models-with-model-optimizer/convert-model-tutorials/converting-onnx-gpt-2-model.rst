.. index:: pair: page; Converting an ONNX GPT-2 Model
.. _conv_prep__onnx_gpt2:

.. meta::
   :description: This tutorial demonstrates how to convert a pre-trained GPT-2 
                 model from ONNX to the OpenVINO Intermediate Representation.
   :keywords: Model Optimizer, tutorial, convert a model, model conversion, 
              --input_model, --input_model parameter, command-line parameter, 
              OpenVINO™ toolkit, deep learning inference, OpenVINO Intermediate 
              Representation, ONNX, GPT-2, GPT-2 model, pre-trained model, 
              convert a model to OpenVINO IR

Convert an ONNX GPT-2 Model
===========================

:target:`conv_prep__onnx_gpt2_1md_openvino_docs_mo_dg_prepare_model_convert_model_onnx_specific_convert_gpt2` 

`Public pre-trained GPT-2 model <https://github.com/onnx/models/tree/master/text/machine_comprehension/gpt-2>`__ is a large transformer-based language model with a simple objective: predict the next word, given all of the previous words within some text.

Download the Pre-Trained Base GPT-2 Model
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To download the model, go to `this model <https://github.com/onnx/models/blob/master/text/machine_comprehension/gpt-2/model/gpt2-10.onnx>`__, and press **Download**.

To download the model and sample test data, go to `this model <https://github.com/onnx/models/blob/master/text/machine_comprehension/gpt-2/model/gpt2-10.tar.gz>`__, and press **Download**.

Convert an ONNX GPT-2 Model to IR
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Generate the Intermediate Representation of the model GPT-2 by running Model Optimizer with the following parameters:

.. ref-code-block:: cpp

	mo --input_model gpt2-10.onnx --input_shape [X,Y,Z] --output_dir <OUTPUT_MODEL_DIR>

