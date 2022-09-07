.. index:: pair: page; Convert an ONNX Mask R-CNN Model
.. _conv_prep__onnx_mask_rcnn:

.. meta::
   :description: This tutorial demonstrates how to convert a pre-trained Mask 
                 R-CNN model from ONNX to the OpenVINO Intermediate Representation.
   :keywords: Model Optimizer, tutorial, convert a model, model conversion, 
              --input_model, --input_model parameter, command-line parameter, 
              OpenVINO™ toolkit, deep learning inference, OpenVINO Intermediate 
              Representation, ONNX, Mask R-CNN, Mask R-CNN model, pre-trained 
              model, convert a model to OpenVINO IR


Convert an ONNX Mask R-CNN Model
================================

:target:`conv_prep__onnx_mask_rcnn_1md_openvino_docs_mo_dg_prepare_model_convert_model_onnx_specific_convert_mask_rcnn` The instructions below are applicable **only** to the Mask R-CNN model converted to the ONNX file format from the `maskrcnn-benchmark model <https://github.com/facebookresearch/maskrcnn-benchmark>`__.

#. Download the pre-trained model file from `onnx/models <https://github.com/onnx/models/tree/master/vision/object_detection_segmentation/mask-rcnn>`__ :
   
   * commit-SHA: 8883e49e68de7b43e263d56b9ed156dfa1e03117.

#. Generate the Intermediate Representation of the model by changing your current working directory to the Model Optimizer installation directory and running Model Optimizer with the following parameters:
   
   .. ref-code-block:: cpp
   
       mo \
      --input_model mask_rcnn_R_50_FPN_1x.onnx \
      --input "0:2" \
      --input_shape [1,3,800,800] \
      --mean_values [102.9801,115.9465,122.7717] \
      --transformations_config front/onnx/mask_rcnn.json

Be aware that the height and width specified with the ``input_shape`` command line parameter could be different. For more information about supported input image dimensions and required pre- and post-processing steps, refer to the `documentation <https://github.com/onnx/models/tree/master/vision/object_detection_segmentation/mask-rcnn>`__.

#. Interpret the outputs of the generated IR file: masks, class indices, probabilities and box coordinates.
   
   * masks.
   
   * class indices.
   
   * probabilities.
   
   * box coordinates.

The first one is a layer with the name ``6849/sink_port_0``, and rest are outputs from the ``DetectionOutput`` layer.

