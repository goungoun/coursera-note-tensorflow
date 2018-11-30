## CMLE (Cloud Machine Learning Engine) How to
- Train a model on Google Cloud
- Monitor model training
- Deploy a trained model as a microservice

## Gradient decend optimization
- Not easy like Map Reduce
- Parameter servers to assist a pool of training workers

## Training your model with Cloud Machine Learning Engine
- Use tensorflow to create computation graph and training application
- "Package" your trainer application
- Configure and start a Cloud ML Engine job

## How to send job to Machine Learning Engine (MLE)?
task.py 
- Entry point code 
- parse commandline parameter
~~~python
parser.add_argument(
	'--train_data_paths', required=True)

parser.add_argument(
	'--train_steps', ...)
~~~

model.py
- training and evaluation input function
- Feature columns
- Feature engineering
- Serving input function
- Train and evluate loop
~~~python
def train_and evaluage(args):
	estimator = tf.estimator.DNNRegressor(
					model_dir=args['output_dir'],
					feature_column=feature_cols,
					hidden_units=args['hidden_units']
					)
	train_spec=tf.estimator.TrainSpec(
					input_fn=read_dataset(args['train_data_path'])
					)
	...
~~~

## TIP
- Use single-region bucket for ML
- Multi-region by default is better suited for web serving than ML training

## Monitorig job
Gcloud cmd
~~~bash
gcloud ml-engine jobs describe job_name
gcloud ml-engine jobs stream-jobs job_name
gcloud ml-engine jobs list --filter='createTime>2017-01-15T19:00'
gcloud ml-engine jobs list --filter='jobId:census*' --limit=3
~~~
GCP console: CPU & Memory utilization, 


