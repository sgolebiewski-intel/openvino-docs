.. index:: pair: page; Convert TensorFlow FaceNet Models
.. _conv_prep__conv_tensorflow_face_net:

.. meta::
   :description: This tutorial demonstrates how to convert a FaceNet model 
                 from TensorFlow to the OpenVINO Intermediate Representation.
   :keywords: Model Optimizer, tutorial, convert a model, model conversion, 
              --input_model, --input_model parameter, command-line parameter, 
              OpenVINO™ toolkit, deep learning inference, OpenVINO Intermediate 
              Representation, TensorFlow, FaceNet, FaceNet model, 
              pre-trained model, convert a model to OpenVINO IR

Convert TensorFlow FaceNet Models
=================================

:target:`conv_prep__conv_tensorflow_face_net_1md_openvino_docs_mo_dg_prepare_model_convert_model_tf_specific_convert_facenet_from_tensorflow` 

`Public pre-trained FaceNet models <https://github.com/davidsandberg/facenet#pre-trained-models>`__ contain both training and inference part of graph. Switch between this two states is manageable with placeholder value. Intermediate Representation (IR) models are intended for inference, which means that train part is redundant.

There are two inputs in this network: boolean ``phase_train`` which manages state of the graph (train/infer) and ``batch_size`` which is a part of batch joining pattern.

.. image:: ./_assets/FaceNet.png
	:alt: FaceNet model view

Convert a TensorFlow FaceNet Model to the IR
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To generate a FaceNet OpenVINO model, feed a TensorFlow FaceNet model to Model Optimizer with the following parameters:

.. ref-code-block:: cpp

	 mo
	--input_model path_to_model/model_name.pb       \
	--freeze_placeholder_with_value "phase_train->False"

The batch joining pattern transforms to a placeholder with the model default shape if ``--input_shape`` or ``--batch`` \*/\* ``-b`` are not provided. Otherwise, the placeholder shape has custom parameters.

* ``--freeze_placeholder_with_value "phase_train->False"`` to switch graph to inference mode

* ``--batch`` \*/\* ``-b`` is applicable to override original network batch

* ``--input_shape`` is applicable with or without ``--input``

* other options are applicable

