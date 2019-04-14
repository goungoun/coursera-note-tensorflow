# Course3. Tensorflow

## Graph and Session
- graph: node + edges
- note: operation of i/o
- edges : array of data

## Lazy evaluation
- It minimize the context switch between python and c++
~~~python
...
c = tf.add(a, b)
~~~
- a, b, c => tensor
- add => operations
- You need to run it as a part of session
 "Hey session pleaes evaluate for me."
- You write a DAG and then you run the DAG in the context of a session to get results
- 1st Build Stage: create a graph
- 2nd Run State: run the graph 

## tf.eager
- Execute immediatly when developing or debugging code
~~~python
import tenorflow as tf
from tensorflow.contrib.eager.python import tfe # import tf.eager

tfe.enable_eager_execution() # call exactly once

x = tf.constant([3, 5, 7])
y = tf.constant([1, 2, 3])

print (x-y)

tf.Tensor([2 3 4], shape=(3,), dtype=int32)
~~~

## Numpy vs Tensorflow
- numpy
~~~python
a = np.array([5, 3, 8])
b = np.array([3, -1, 2])
c = np.add(a, b) # Evaluated immediatly
print (c)
~~~
- tensorflow
~~~python
a = tf.constant([5, 3, 8])
b = tf.constant([-3, -1, 2])
c = tf.add(a, b) 
print (c) # Tensor ("Add:0", shape=(3,), dtype=int32) <= Debug output of tensor

with tf.Session() as sess:
  result = sess.run(c)
  print result # [8 2 10]
~~~

## Tensor and Variable

Example code 
~~~python
import tensorflow as tf

x = tf.constant([3, 5, 7])
y = tf.constant([1, 2, 3])

z1 = tf.add(x, y)
z2 = x * y # Shortcuts to common arithmetic operators
z3 = z2 - z1

with tf.Session() as sess:
  a1, a3 = sess.run([z1, z3])
  print a1
  print a3
~~~
> constant에 소수점을 넣어서 타입을 바꿔보는 연습을 함

Heron's forumlar
Example 1 - `tf.constant`
~~~python
def compute_area(sides):
  # slice the input to get the sides
  a = sides[:,0]  # 5.0, 2.3
  b = sides[:,1]  # 3.0, 4.1
  c = sides[:,2]  # 7.1, 4.8
  
  # Heron's formula
  s = (a + b + c) * 0.5   # (a + b) is a short-cut to tf.add(a, b)
  areasq = s * (s - a) * (s - b) * (s - c) # (a * b) is a short-cut to tf.multiply(a, b), not tf.matmul(a, b)
  return tf.sqrt(areasq)

with tf.Session() as sess:
  # You don't want to calculate just one triangle area.
  # You are going to calculate area of a lot of triangles!!
  area = compute_area(tf.constant([
      [5.0, 3.0, 7.1],
      [2.3, 4.1, 4.8]
    ]))
  result = sess.run(area)
  print(result)

~~~
> tf.constant에 왜 두 줄을 적었을까? 
> 삼각형 빗변의 길이로 구성된 여러개의 데이터를 한번에 입력하고 각각의 삼각형의 면적을 한번에 구함

Example 2 - `placeholder` and `feed_dict`
~~~python
with tf.Session() as sess:
  sides = tf.placeholder(tf.float32, shape=(None, 3))  # batchsize number of triangles, 3 sides
  area = compute_area(sides)
  result = sess.run(area, feed_dict = {
      sides: [
        [5.0, 3.0, 7.1],
        [2.3, 4.1, 4.8]
      ]
    })
  print(result)
~~~
> 두번째의 예시에서 placeholder & feed_dict를 사용하는 것이 type을 알 수 있어서 훨씬 안심이 되고 코드도 깔끔해짐
> 코딩을 하면서 type을 알 수 없는 불안감을 placeholder가 보완해주고 있고 shape을 통해서 몇차원인지도 알 수 있음 

## How to debug
visualize graph
~~~python
import tensorflow tf

x = tf.constant([3, 5, 7], name="x")
x = tf.constant([1, 2, 3], name="y")
z1 = tf.add(x, y, name="z1")
z2 = x * y
z3 = z2 - z1

with tf.Session() as sess:
	with tf.summary.FileWriter('summaries', sess.graph) as writer:
		a1, a3 = sess.run([z1, z3])

! ls summaries

~~~

## tf_estimator


## Quiz
> 텐서플로우는 (exclusively) 머신러닝을 처리하기 위한 용도라고 체크했는데 수학적인 연산을 하기 위해 사용할 수 있다고 함

The iterative process where a TensorFlow model can crowdsource and combine model feedback from individual users is called what?

tf.summary.FileWriter
shape of tf.constant([2, 3, 5])

