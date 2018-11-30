### Lab
| Use feature cross to create a good classifier

http://goo.gl/2NUCAF
http://goo.gl/ivd4x4
http://goo.gl/ofiHCT

## Questions
- What's the best performance you can get?
- Which feature crosses help the most?
- Does the model output surface look like a linear model?

## Feature
- raw input: x1, x2
- created fature: x1*x1, x2*x2, x1*x2
- feature cross: x1*x2, x1*x1(self cross), x2*x2(self cross)
- Notice a relative thickness of the five lines running from input to output

## Create feature crosses using Tensorflow
- You can cross two or more categorical or bucketized columns
- Before doing so, bucktize
~~~python
day_hr = tf.feature_column.crossed_column(
	[dayofweek, hourofday],
	24 * 7) # number of hash buckets: feature-cross % hash_bucket_size
~~~

## With a dense layer (=Embedding)
- Pass it through a dense layer
- You could also choose to load the embeding from London when Frankfruit data is not collected

## Quiz
- feature cross works only with large dataset to train(?)