### Cloud DataFlow (=Apache Beam)

## Dataflow
- Dataflow can run pipelines written in Python and Java programming languages
- Execute data pipelines at scale
- Developer don't need to care about the size of cluster
- Dataflow can change the amount of compute resources
- The API can be executed on Flink, Spark etc
- Parallel tasks (autoscaled by execution framework)
- Batch + Stream processing using the same code

## Pipeline - Batch
Input => Read => Transform => Group => Filter => Write => Output
~~~python
p = beam.Pipelines()
(p
	| beam.io.ReadfromText('gs://..')
	| beam.Map(Transform)
	| beam.GroupByKey()
	| beam.FlatMap(Filter)
	| beam.io.WriteToText('gs://...')
)
p.run()

def Transform(line):
	return (count_words(line), 1)

def Filter(key, values):
	return key > 10
~~~

## Pipeline - Stream
Batch + Stream using the same code
~~~python
p = beam.Pipelines()
(p
	| beam.io.ReadStringFromPubSub('project/topic')
	| beam.WindowInto(SlidingWindows(60))
	| ..
	| ..
	| ..
	| beam.io.WriteToBigQuery(table)
)
p.run()
~~~

## PCollection
Unlike the other data structures, PCollection does not store all of its data in memory

## Shard the Output
- There can be multiple servers trying to write results to the file system
- In order to avoid contention issues where multiple servers are trying to get a file lock to the same file concurrently, by default, the text I/O connector wil the output, writing the results acrfoss multiple files in the file system.
- Pipeline is writing the result to a file with the prefix output in the data connector
~~~python
beam.io.WriteToText(file_path_prefix='/data/output', file_name_suffix='.txt')
~~~

## Execute on the cloud
~~~python
gsutil cp ../javahelp/src/main/java/com/google/cloud/training/dataanalyst/javahelp/*.java gs://bkt1999/javahelp
~~~