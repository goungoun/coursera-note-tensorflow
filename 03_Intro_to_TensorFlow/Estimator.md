## Estimator API
- train the model
- evaluate the model
- predict the model
- repeat with a deep neural network model in Tensorflow

## Goal
- Create production-ready machine learning model using an API
- Train on large datasets that do not fit in memory
- Quickly monitor your training metrics in Tensorboard
- All of the above

## tf.estimator
~~~python
import tensorflow as tf

featcols = [
  tf.feature_conlumn.numeric_column("sq_footage")
  tf.feature_column.categorical_column_with_vocabulary_list("type", ["house", "apt"])
]

model = tf.estimator.LinearRegressor(featcols)
~~~

more column types
~~~python
tf.feature_column.bucktized_column
tf.feature_column.embedding_column
tf.feature_column.crossed_column
tf.feature_column.categorical_column_with_hash_bucket
~~~

## Estimator

~~~python
featcols = [
  tf.feature_column.numeric_column("sq_footage")
  tf.feature_column.categorical_column_with_vocabulary_list("type", ["house", "apt"])
]

model = tf.estimator.LinearRegressor(fetcols)
model.train(train_input_fn, steps=100)
prediction = model.predict(predict_input_fn)
~~~

~~~python
model = tf.estimator.DNNRegressor(featcols, hidden_units=[3,2])

~~~

## Chcekpoints 
We need a way to save our train model.

~~~python
model = tf.estimator.LinearRegressor(featcols, './model_trained') # where to put the checkpoint
model.train(train_input_fn, steps=100)

predictions = trained_model.predict(predict_input_fn)
~~~

## Step
~~~python
model.train(pandas_train_input_fn(df))
model.train(pandas_train_input_fn(df, steps=1000)) # 1000 additional steps from checkpoint
model.train(pandas_train_input_fn(df), max_steps=1000) # 1000 steps - might be nothing if checkpoint already there
~~~

## Data
taxi-train.csv - 7770 rows
taxi-valid.csv - 1665 rows
taxi-test.csv  - 1666 rows

~~~python
df_train = pd.read_csv('./taxi-train.csv', header = None, names = CSV_COLUMNS)
df_valid = pd.read_csv('./taxi-valid.csv', header = None, names = CSV_COLUMNS)
df_test = pd.read_csv('./taxi-test.csv', header = None, names = CSV_COLUMNS)
~~~

## Distributed training using "data parallism"
use tf.estimator.train_and_evaluate(estimator, ...)
~~~python
estimator = tf.estimator.LinearRegressor (...)
tf.estimator.train_and_evaluate(estimator, ...)
~~~