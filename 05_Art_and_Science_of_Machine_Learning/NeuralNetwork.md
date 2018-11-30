# Neural Network

## Quiz
- Why is it import adding non-linear activtion functions to neural networks?
=> Stops the layers from collapsing back into just a linear model

## LeLU
- f(x) = max(0, x)
- 10 times the speed of training than networks with sigmoid hidden activations

## Multi-class neural nets
- multi-class classification problems
- many binary classification problems
- Pit/Stalls/Circle/Suite => One versus all or one versus rest approach
- Model 1 - Pit(positive)/Other(negative)
- Model 2 - Stalls(positive)/Other(negative)
- Model 3 - Circle(positive)/Other(negative)
- Model 4 - Suite(positive)/Other(negative)

## Softmax loss
~~~python
loss = tf.reduce_mean(
	tf.nn.softmax_cross_entropy_with_logits_v2(logits, labels) -> shape = [batch_size]
	)
~~~

## Noise-contrasive loss function
