## ParDo (is short of parallel do)
- Apache beam ParDo class
- When? Want to take a trasformatin in your data processing pipeline and let dagta flow run it at scale with automatic distribution across many nodes in a cluster

? acts on all items at once


## Apache Beam and Cloud dataflow
- same?
- 

## Cloud Dataflow preprocessing
- data source, transformation steps, and data sink

## To run a pipeline you need something called a 
- runner? (o)
- executor? (x)
- pipeline? (x)

## Example
We are compiling our Cloud Dataflow pipeline written in Java and are submitting it to the cloud for execution
~~~bash
mvn compile -e exec:java \
  -Dexec.mainClass=$Main \
  -Dexec.args="--project=$PROJECT" \
  --stagingLocation=gs:$BUCKET/staging/ \
  --tempLocation=gs://$BUCKET/staging/ \
  --runner=DataflowRunner
~~~

## Quiz
wrap features in training/evaluation input function and wrap features in serving input function

~~~python
def add_engineered(features):
	lat1 = features['pickuplat']
	...
	dist = tf.sqrt(latdiff*latdiff + londiff*londiff)
	fature['euclidean'] = dataset
	return features

def serving_input_fn():
	feature_placeholders = ...
	features = ...
	return tf.estimator.export.ServingInputReceiver(add_engineered(features), feature_placeholders)
~~~