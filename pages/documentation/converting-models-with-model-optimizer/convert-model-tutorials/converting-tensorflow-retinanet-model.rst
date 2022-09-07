.. index:: pair: page; Convert a TensorFlow RetinaNet Model
.. _conv_prep__conv_tensorflow_retina_net:

.. meta::
   :description: This tutorial demonstrates how to convert a RetinaNet model 
                 from TensorFlow to the OpenVINO Intermediate Representation.
   :keywords: Model Optimizer, tutorial, convert a model, model conversion, 
              --input_model, --input_model parameter, command-line parameter, 
              OpenVINO™ toolkit, deep learning inference, OpenVINO Intermediate 
              Representation, TensorFlow, RetinaNet, RetinaNet model, convert a 
              model to OpenVINO IR, transformations_config

Convert a TensorFlow RetinaNet Model
====================================

:target:`conv_prep__conv_tensorflow_retina_net_1md_openvino_docs_mo_dg_prepare_model_convert_model_tf_specific_convert_retinanet_from_tensorflow` This tutorial explains how to convert a RetinaNet model to the Intermediate Representation (IR).

`Public RetinaNet model <https://github.com/fizyr/keras-retinanet>`__ does not contain pre-trained TensorFlow weights. To convert this model to the TensorFlow format, follow the Reproduce Keras to TensorFlow Conversion tutorial.

After converting the model to TensorFlow format, run the Model Optimizer command below:

.. ref-code-block:: cpp

   mo --input "input_1[1 1333 1333 3]" --input_model retinanet_resnet50_coco_best_v2.1.0.pb --data_type FP32 --transformations_config front/tf/retinanet.json

Where ``transformations_config`` command-line parameter specifies the configuration json file containing model conversion hints for the Model Optimizer. The json file contains some parameters that need to be changed if you train the model yourself. It also contains information on how to match endpoints to replace the subgraph nodes. After the model is converted to the OpenVINO IR format, the output nodes will be replaced with DetectionOutput layer.
