# Course2. Launching into Machine Learning
~~~
Tensorflow + Data Lab + Big Query
~~~

## ML History
- 1800s: Linear Regression to predict plants and pea growth
- 1940s: Perceptron that precursor to neural networks
- 1960s: Neural Networks
- 1980s: Decision Trees
- 1990s: Kernel Method Support Vector Machines(SVMS)
- 2000s: Random Forests, Boosted Trees
- 2010s: Neural Networks again

## y = b + x * m
- labels and features can be extended to arbitraily high dimensionality 
>단순한 모델 같아도 고차원으로 확장 가능 (input과 output 모두)
>m 이 n차원으로 늘어날 수 있고 n차원으로 늘어날 때 hyper plain으로 시각화
- b는 scalar or a vector which is reffered to as the bias term

## class
- Just pretend that each value is its own independent class
- Represent as binary integers
 
## Decision Boundary
- Why do you call decision boundary?
- Because it reflects our decision about where the class begin and end.

## Generalization
- Critically the decision boundary is intended not just to be descriptive of the current data. 
- It's intended to be predictive of unseen data. 
- This property to extending to unseen example is called generalization. And it's essential to ML model.

## Loss function 
- Goal: Quantify model performance to figure out better model
- SUM: The sum of singned error cancles each other out!
- MSE(Mean Squared Error) : Hard to interprete. the bigger the MSE the worse the quality of the predictions
- RMSE(Root mean square) : Use Euclidian norm -> L2
- Cross Entropy (or Log Loss) : for classification. penalize bad predictions very strongly

> Hands-On Machine Learning with Scikit-Learn & Tensorflow <br>
> - MAE: Use Manhatan norm -> L1
> - The higher the norm index, the more it focuses on large values and neglect small one
> - When outliers are exponentially rare (like a bell-shaped curve), the RMSE performs very well <br>

## Gradient Descent - Direction, step size
Use loss functions as the basis for an algorithm called gradient descent
~~~python
while loss is > Epsilon:
  direction = computeDirection()
  for i range(self.params):
    self.params[i] = //
    self.params[i] =+ stepSize * direction[i]
  loss = ComputeLoss()
~~~
* Epsilon = A tiny constant

## Repeatable Samples in BigQuery
To make it repeatable : rand() => modulo + hash
~~~sql
#StandaradSQL
SELECT -- date, -- we cannot use date for training because we split dataet using date 
       ABS(FARM_FINGERPRINT(date)) AS date_hash,
       MOD(ABS(FARM_FINGERPRINT(date)),10) AS reminder_divide_by_10,
       airline, departure_airport, departure_schedule, arrival_airport, arrival_delay
  FROM `bigquery-sample.airline_ontime_data.flights`
 -- WHERE RAND() < 0.8
 WHERE MOD(ABS(FARM_FINGERPRINT(date)),10) < 8 -- Hash + Modulo operator
~~~
> what if date is not evenly distributed? 

## ML Examples
- Decision Tree: Titanic
- How do we predict a baby's health before they are born?

## LAB

Creating Repeatable Dataset splits in BigQuery
~~~bash
gcloud compute zones list
datalab create mydatalabvm --zone asia-south1-c
~~~

when datalab is closed
~~~bash
datalab connect mydatalabvm
~~~

~~~bash
%bash
git clone https://github.com/GoogleCloudPlatform/training-data-analyst
rm -rf training-data-analyst/.git
~~~