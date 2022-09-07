.. index:: pair: page; Convert a PyTorch BERT-NER Model
.. _conv_prep__pytorch_bert_ner:

.. meta::
   :description: This tutorial demonstrates how to convert a BERT-NER model
                 from Pytorch to the OpenVINO Intermediate Representation.
   :keywords: Model Optimizer, tutorial, convert a model, model conversion, 
              --input_model, --input_model parameter, command-line parameter, 
              OpenVINO™ toolkit, deep learning inference, OpenVINO Intermediate 
              Representation, Pytorch, BERT-NER, BERT-NER model, pre-trained model, 
              convert a model to OpenVINO IR

Convert a PyTorch BERT-NER Model
================================

:target:`conv_prep__pytorch_bert_ner_1md_openvino_docs_mo_dg_prepare_model_convert_model_pytorch_specific_convert_bert_ner` 

The goal of this article is to present a step-by-step guide on how to convert PyTorch BERT-NER model to OpenVINO IR. First, you need to download the model and convert it to ONNX.

Download and Convert the Model to ONNX
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To download a pre-trained model or train the model yourself, refer to the `instructions <https://github.com/kamalkraj/BERT-NER/blob/dev/README.md>`__ in the BERT-NER model repository. The model with configuration files is stored in the ``out_base`` directory.

To convert the model to ONNX format, create and run the following script in the root directory of the model repository. If you download the pre-trained model, you need to download ` <https://github.com/kamalkraj/BERT-NER/blob/dev/bert.py>`__ to run the script. The instructions were tested with the commit-SHA: ``e5be564156f194f1becb0d82aeaf6e762d9eb9ed``.

.. ref-code-block:: cpp

	import torch
	
	from bert import Ner
	
	ner = Ner("out_base")
	
	input_ids, input_mask, segment_ids, valid_positions = ner.preprocess('Steve went to Paris')
	input_ids = torch.tensor([input_ids], dtype=torch.long, device=ner.device)
	input_mask = torch.tensor([input_mask], dtype=torch.long, device=ner.device)
	segment_ids = torch.tensor([segment_ids], dtype=torch.long, device=ner.device)
	valid_ids = torch.tensor([valid_positions], dtype=torch.long, device=ner.device)
	
	ner_model, tknizr, model_config = ner.load_model("out_base")
	
	with torch.no_grad():
	    logits = ner_model(input_ids, segment_ids, input_mask, valid_ids)
	torch.onnx.export(ner_model,
	                  (input_ids, segment_ids, input_mask, valid_ids),
	                  "bert-ner.onnx",
	                  input_names=['input_ids', 'segment_ids', 'input_mask', 'valid_ids'],
	                  output_names=['output'],
	                  dynamic_axes={
	                      "input_ids": {0: "batch_size"},
	                      "segment_ids": {0: "batch_size"},
	                      "input_mask": {0: "batch_size"},
	                      "valid_ids": {0: "batch_size"},
	                      "output": {0: "output"}
	                  },
	                  opset_version=11,
	                  )

The script generates ONNX model file ``bert-ner.onnx``.

Convert an ONNX BERT-NER model to IR
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. ref-code-block:: cpp

	mo --input_model bert-ner.onnx --input "input_mask[1 128],segment_ids[1 128],input_ids[1 128]"

where ``1`` is ``batch_size`` and ``128`` is ``sequence_length``.

