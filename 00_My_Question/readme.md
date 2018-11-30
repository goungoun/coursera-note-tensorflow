## Session
Session allows Tensorflow to cache and distribute computation

## Share variable?
분산일 때는 공유하고 local일 때는 공유하지 않는다?

## sess.run(z) == z.eval() 

## Eager Execution 으로 바꾸는 방법?
한번만 실행
~~~python
tfe.enable_eager_execution()
~~~

## 
이름을 왜 주는 것이지? 변수명이 x라고 선언되어있는데 
Namme the tensors and the operations
~~~python
import tensorflow as tf

x = tf.constant([3, 5, 7], name="x")
y = tf.constant([1, 2, 3], anme="y")
z1 = tf.add(x, y, name="z1")
z2 = x * y
z3 = z2 - z1
~~~

## FileWriter 
그래프를 write하는 기능. 쓴 다음에는 도대체 뭘 하지? 텐서보드에서 시각화 하려고  
Write the session graph to summary 디렉토리 
~~~python

with tf.Session as sess():
  with tf.summary.FileWriter('summaries', sess.graph) as writer:
  	a1, a3 = sess.run([z1, z3])
~~~

~~~
! ls summaries
~~~

## Tensorboard를 실행하는 법
그래프가 시각화되서 보여짐
~~~python
from google.datalab.ml import TensorBoard
Tensorboard().start('./summaries')
~~~

## google storage에 써놓고 Cloud shell에서 읽기
~~~python
tensorboard --port 8080 --logdir gs://${BUCKET}/${SUMMARY_DIR}
~~~

## Tensor의 shape 이해하기

`scala => vector => matrix => 3D Tensor => nD Tensor`
tf.constant 또는 stack으로 표기 

0 차원 - scala  ()
~~~python
x = tf.constant(3)
~~~

1 차원 - vector (3,)
~~~python
x = tf.constant([1, 2, 3])
~~~


2 차원 - matrix (2, 3)
~~~python
x = tf.constant([[1, 2, 3], [4, 5, 6]])
	             
~~~

3 차원 - 3D Tensor (2, 2, 3)
~~~python
x = tf.constant([[[1, 2, 3], [4, 5, 6]],
	            [[1, 2, 3], [4, 5, 6]]])

~~~

n 차원 - nD Tensor
~~~python
x1 = tf.constant([2, 3, 4]) # (3, )
x2 = tf.stack([x1, x1]) # (2, 3)
x3 = tf.stack([x1, x2, x2, x2]) # (4, 2, 3)
x4 = tf.stack([x3, x3]) # (2, 4, 2, 3)
~~~
