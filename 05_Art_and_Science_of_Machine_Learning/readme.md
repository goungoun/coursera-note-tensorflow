### Art and Science of Machine Learning

## Overfitting

## Occam's lazor 
> When presented with completing hypothetical answers to a problem, 
> one should select the one that makes |the fewest assumptions.
> from. wikipedia

## Regularization is a major field of ML research
- Early Stopping
- Parameter Norm Penalties: L1, L2 regularization, Max-norm regularization
- Dataset Augmentation
- Noise Robustness
- Sparse Representations
- Regularization does not affect the model structure. It changes on ly the loss function

## L1 & L2 Regularization
- To penalize model complexity
- Magnitude of a vector is represented by the norm function
- L1 (=featue selection): Diamond shape. Sparsity. Some of the weights end up having the optimal value zero
- L2 (=weight decay): Circle shape in 2D space
- We use the square of the L2 norm to simplify calculation of derivatives
- lamda: simple scala value that allows us to control how much emphasis we want to put on model simplicity over minimizing training error

## Optimization is a major field of ML research
- GradientDecent: traditional. typically implented stochastically 
- Momentum: Reduces learning rate when gradient values are small
- AdaGrad: Give frequently occuring features low learning rates
- AdaDelta: Improves AdaGrad by avoiding reducing LR to zero
- Adam: AdaGrad with a bunch of fixes
- Ftrl: "Follow the regularized leader", works well on wide models

## Parameter vs Hyper-parameters
- Parameter: Changed during model training
- Hyper-parameters: Set before training

## Google Vizier
- Hyperpameter tuning in auto magical way!
- Paper: Google Visier - A Service for Black-Box Optimization

~~~python
parser.add_argument(
	'--nbuckets',
	help = 'Number of buckets into which to discretize lats and lons'
	default = 10,
	type = int
	)

parser.add_argument(
	'--hidden_units',
	help = 'List of hidden layer size to use for DNN feature columns'
	nargs = '+',
	default = [128, 32, 4]
	)
~~~

~~~bash
%writefile hyperparam.yaml
~~~

## Lab
-- model.py
-- task.py
-- gcloud ml-engine
~~~bash
rm -rf house_trained
export PYTHONPATH=${PYTHONPATH}:${PWD}/house_prediction_module
gcloud ml-engine local train \
    --module-name=trainer.task \
    --job-dir=house_trained \
    --package-path=$(pwd)/trainer \
    -- \
    --batch_size=30 \
    --learning_rate=0.02
~~~


~~~bash
trainingInput:
  hyperparameters:
    goal: MINIMIZE
    maxTrials: 5
    maxParallelTrials: 1
    hyperparameterMetricTag: rmse
    params:
    - parameterName: batch_size
      type: INTEGER
      minValue: 8
      maxValue: 64
      scaleType: UNIT_LINEAR_SCALE
    - parameterName: learning_rate
      type: DOUBLE
      minValue: 0.01
      maxValue: 0.1
      scaleType: UNIT_LOG_SCALE
~~~

--hyperparam.yaml 
~~~bash
%%bash
OUTDIR=gs://${BUCKET}/house_trained   # CHANGE bucket name appropriately
gsutil rm -rf $OUTDIR
export PYTHONPATH=${PYTHONPATH}:${PWD}/house_prediction_module
gcloud ml-engine jobs submit training house_$(date -u +%y%m%d_%H%M%S) \
   --config=hyperparam.yaml \
   --module-name=trainer.task \
   --package-path=$(pwd)/house_prediction_module/trainer \
   --job-dir=$OUTDIR \
   --runtime-version=$TFVERSION \
   -- \
   --output_dir=$OUTDIR
 ~~~

