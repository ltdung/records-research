# Graph Convolutional Transformer

This repository contains an implementation of Graph Convolutional Transformer, as described in “Graph Convolutional Transfomer: Learning the Graphical Structure of Electronic Health Records” (Choi et al. 2019). Code was written using Python 2.7 and Tensorflow 1.12.

The code sample provided here is not executable. We illustrate the core implementation of our model architecture, focusing on embedding the input features, transforming them with the model, and deriving the logits for binary prediction. The input data format is TensorFlow SequenceExample. We provide a Python script that generates trainable SequenceExamples from Philips eICU Collaborative Dataset. The current implementation only supports graph-level binary prediction (e.g. mortality prediction based on a single visit).

This is not an officially supported Google product.

## Overview and Usage Guidelines

Our model requires the following inputs:
- features: A dictionary mapping from a sequence feature name to a tf.SparseTensor containing all tokens of that feature type for a batch of inputs. This dictionary can be extracted from a SequenceExample using tf.io.parse_single_sequence_example.
- labels: A tf.Tensor with dimensions [batch size, 1], representing the binary labels for binary prediction.

We implement the following files to run the model:
- graph_convolutional_transformer.py: This file contains everything required to feed the feature dictionary, and obtain logits and the loss.
- process_eicu.py: This file preprocesses Philips eICU Collaborative Dataset in order to obtain SequenceExamples that can be used to test the model.

To train the model, simply call EHRTransformer.get_prediction on the inputs to generate logits. Subsequently, call EHRTransformer.get_loss to generate the loss for training. The loss can then be minimized using standard Tensorflow methods, e.g. calling optimizer.minimize(loss) with a tf.train.Optimizer of choice.

To preprocess the eICU data, request access to the dataset from the [eICU website](https://eicu-crd.mit.edu/gettingstarted/access/). Note that you are required to participate in the CITI training. Once you have gained access to eICU data, download the patient, admissionDx, diagnosis, treatment CSV files. Then place process_eicu.py to where the CSV files are, and execute `python process_eicu.py`. By default, it will generate 5 randomly sampled sets of train/validation/test samples.
