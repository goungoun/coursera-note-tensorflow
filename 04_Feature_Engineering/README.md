# Course4. Feature Engineering
~~~
Tensorflow + Apache Beam + Cloud Dataflow + Cloud Dataprep
~~~
## Feature Engineering
- Scale to large dataset
- Find good features
- Preprocess with Cloud MLE

## Feature vector
- Raw data must be mapped into numerical feature vectors
Raw data
~~~
0 : {
	house_info: {
		num_rooms : 6
		num_bedrooms : 3
		street_name : "Main Street"
		num_basement_rooms: -1
		...
	}
}
~~~
numerical feature vectors
~~~
[6.0, 1.0, 0.0, 0.0, 9.321, -2.20, 1.01 ... ,]
~~~

## One-hot encoding
~~~
{
	"transactionId": 42
	"item" : "Ice Cream"
	"servedBy" : {
		"employeeId": 72365,
		"waitTime": 1.4,
		"customerRating": 4
	}
}
~~~

Let's say we have five employees so use five columns
> One column is hot, and all other columns are cold => called one-hot encoding
~~~
72364  72365  72366  72367  72368
    0      1      0      0      0
~~~
- In TensorFlow, this is called a sparse column
- What if new employee??

## Encoding categorical data
String: EmployeeId
~~~python
tf.feature_colmn.categorical_column_with_vocabulary_list('employeeId',
	vocabulary_list = ['72364', '72365', '72366', '72367', '72368'])
~~~

Number: Indexed already
~~~python
tf.feature_column.categorical_column_with_identity('employeeId', num_buckets = 5)
~~~

Don't have vocabulary of all possible values
~~~python
tf.feature_column.categorical_columnwith_hash_bucket('employeeId', hash_bucket_size = 500)
~~~

## Missing Data
~~~
{
	"servedBy" : {
		..
		"customerRating": -1
	}
}
~~~

You need to have another column <br>
ex1)
~~~
[..., 4, 1, ...] # 4, 1:responded
[..., 0, 0, ...] # -1, 0: no response
~~~

ex2)
~~~
[..., 0, 0, 0, 1, 1, ...] # 4

[..., 0, 0, 0, 0, 0, ...] # -1
~~~

## Good vs Bad Features
- Be related to the objective
- Be known at prediction-time
- Be numeric with meaningful magnitude
- Have enough examples ex.(At least) Five sample rule
- Bring human insight to problem

## Outliers
- ML: Lots of data, keep outliers and build models for them
- Statistics : I've got all the data I'll ever get. Throw away outliers

## Fixed Bin Boundaries
- Predicting House value: latitudes & two peaks
- No linear relationship between latitude and the housing values
- But yet individual latitudes can be a pretty good indicator of housing value ex) Los Angeles and SanFrancisco
- Idea: One floating fature => 11 distinct boolean values.. Yes-No LatitudeBin1, LatitudeBin2, ...

## Lab
~~~
https://github.com/GoogleCloudPlatform/training-data-analyst/tree/master/courses/machine_learning/deepdive/04_features
~~~

## Preprocessing and Feature Creation
- When to use? Rescale or Normalize before machine learning
- What to use? BigQuery or Beam
- Performance for gradient decent
- You need to know Min, Max value : compute aggregate statistics for numeric columns
Normalize
~~~python
feature['scaled_price']=
  (feature['price']-min_price)/(max_price-minprice)
~~~
- Categorical
~~~python
tf.feature_column.categorical_column_with_vocabulary_list('city',
	keys=['San Diego', 'Los Angeles', 'San Francisco', 'Sacramento'])
~~~

## Tensorflow
- RealValue to discrete one
~~~
tf.feature_column.bucketizedcolumn(lat, boundaries=np.arange(32,42,1).tolist())
~~~

## BigQuery
~~~sql
select (tolls_amount + fare_amount) as fare_amount,
  DAYOFWEEK(pickup_datetime) as dayofweek,
  HOUR(pickup_datetime) as hourofday,
  ...
 from `nyc-tlc.yellow.trips`
where trip_distince > 0
~~~

## Beam
- Compute time-windowed statistics (e.g. number of products sold in previous hour) for use as input feature
<= Beam Only!


## feature cross
- To separate prediction per grid cell
- `x3 = x1*x2`
- one hot and cold the first set of values
  one hot and cold the second set of values
  x3 feature cross them: only one hot when x1 = 1 and x2 = 1 => Only one buckdet fires
  그림이 네모칸 수가 조금 이상한 것 같음 
~~~python
 x1
 -----------
 0  1  0  0
 -----------
 
 x2
 -----------
 0  0  1  0
 -----------
 
 x1*x2
 ----------------------
 0  0  1  0  ...
 ----------------------
 ~~~
 - Why feature cross is powerful?
 If you take these feature crossed values and feed them into a linear regression, the ratio of blue dots to yellow dots in the grid cell corresponding to x1 and x2

## Taxi color + City => Linear Model
- Feature crosses + massive data is an efficient way for learning hightly complex spaces
- Feature croesses allow a linear model to memorize large dataset
- Use feature cross. It is so powerful.
- NY*yello color, Rome*yello color, Rome*green color etc..
- Weighted sum : yello color, NY only. 

## Convex problem vs Non convex problem
- Convex problem is easier than non convex problem
- Optimizing linear model is convex problem