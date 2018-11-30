### Transform

## Analyze phase
- You analyze the training dataset
- Schema for training dataset
~~~python
 # Tensosrflow type for input column
raw_data_schema = {
	colname: dataset_schema.ColumnSchema(tf.string, ...)
	  for colname in 'dayofweek, key'.split(',')
}

# update the raw data schema by adding al the tf.float32 typed columns
raw_data_schema.update({
	colname: dataset_schema.ColumnSchema(tf.float32, ...)
	  for colname in 'fare_amount, pickuplon, ..., dropofflat'.split(',')
	})

# raw data schema for beam on dataflow (metadata template)
raw_data_schema = 
  dataset_metadata.DatasetMetadata(dataset_schema.Schema(raw_data_schema))
~~~

- run the analyze and transform PTransform on training dataset to get back preprocessed training data and the transform function
~~~python
raw_data = (p
	| beam.io.Read(beam.io.BigQuerySource(query=myquery, use_standard_sql=True))
	| beam.Filter(is_valid)
	)

transformed_dataset, transform_fn = ((raw_data, raw_data_metadata))
    | beam_impl.AnalyzeAndTransformDataset(preprocess))
~~~

## Transform phase


## Supporting serving



