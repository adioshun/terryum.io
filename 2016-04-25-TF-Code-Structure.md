---
layout:     post
title:      The Basic Workflow of Tensorflow Codes
date:       2016-04-25 08:00:00
summary:    The basic Wwrkflow of TensorFlow codes
categories: ml_practice
thumbnail: pencil
tags:
 - Machine learning applications
 - Tensorflow
---

# What is Tensorflow
&nbsp;
I believe you may have already heard about what the [TensorFlow][TF] is. **TensorFlow**(TF) is an open source machine learning library [developed by Google][TF_Google]. As the name it is, the library uses  **tensors**(multi-dimensional arrays) as its basic data type, and run the learning algorithm as if liquid(data) is **flowing** through the graph structure.

The **structure of TF codes** is quite intuitive. First, **define the graph structure** for learning, and second, **instil the data liquid** (data stream) into the graph structure. **That's it**. How simple it is...!

Before I start, I'd like to mention that the contents of this article is based on the following tutorials and example codes. Also, I'd like to note that the codes in this article is not for actual performing the TF, but **for understanding** of the basic workflow of the TF. Thus, please refer to the example codes listed below for completing your TF codes.

### Tutorials
(i) [B. Ramsundar's][L1], and (ii) [C-C. Chiu's slides][L2]

### Example Codes
(i)[S. Choi's][L3], (ii) [N. Lintz's][L4], (iii), [A. Damien's][L5], and (iv) [P. Mital's][L6] Github repositories

![TensorFlow][Img_TF]

# Backtracking of the flow of a TF code
&nbsp;
I'd like to **backtrack** the workflow of a TF codes from the end. What would be the *last* step of the TF code? It must be **feeding the data** into the pre-built graph structure (learning structure) to get the results.

### Feeding data into the graph structure
&nbsp;
The ```tf.Session()``` runs the learning structure by feeding the data ```feed_dict``` into the structure ```train_op```. Note that all variables should be initialized before used.

```python
with tf.Session() as sess:
    tf.initialize_all_variables().run()
    sess.run(train_op, feed_dict={X: trX, Y: trY})
```

We can think the learning structure as a **continuous data flow**. If you fetch a datum block from the end, then a input block will automatically be fed into the front because the flow should be continuous. Thus, what the ```tf.Session()``` need to acceess is not the whole structure of the graph, but just the the last output of the structure. Here, the last output that ```tf.Session()``` is accessing is ```train_op```.

### Optimization
&nbsp;
Our remaining question is where ```train_op``` comes from. It should be the learning result from the leaning structure, i.e., **neural network**. In fact, the process of learning neural network is nothing but **optimizing weights** of the neural netork to fit the model into given truths(labels). Thus, the last step of the learning structure is **optimization**, and ```train_op``` comes from this optimiztion process.

The optimization process is defined with the choice of **optimizer** and **cost function**.

```python
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(y_pred, y_true))
train_op = tf.train.GradientDescentOptimizer(0.05).minimize(cost)
```
Here, we use ```softmax_cross_entropy_with_logits``` as our cost function, and ```GradientDescentOptimizer``` as our optimizer. You can find list of available optimizers and cost functions (cross entropy functions) from [here][Opt] and [here][Loss], respectively. You can also define your own optimizer or cost function for your own purpose.

### Structure of the neural network
&nbsp;
[![Perceptron][Img_Neuron1]][Img_Neuron2]

Now, we need to define the **subject** of the optimization; what structure should we optimize? The answer is the **neural network** structure. Thus, we need to define the neural network structure to be optimized. The basic structure of neural network is as follows:

1. First, apply linear transform (affine function) to the input with the weight $$  W$$ and biases $$b$$, i.e., $$y = Wx + b$$
2. Apply an activation fuction to $$y$$ to introduce nonlinearilty to the learning structure. An ordinary choice of the activation function is [rectified linear unit][ReLU] (ReLU).
3. To make the final output in the range [0,1], apply [softmax funtion][SoftMax] to the output at the last (output) layer.


```python
# Step 1 : Affine function
W = tf.Variable(tf.random_normal([n_input, n_output]))
b = tf.Variable(tf.random_normal([n_output]))
y = tf.matmul(x, W) + b
# Step 2 : ReLU
y_pred = tf.nn.relu(y)
# Step 3 : Softmax (for the last layer)
softmax_out = tf.nn.softmax(y_pred)
```
The *Step 3* can be omitted if the cost function has already included softmax funtion as in the ```softmax_cross_entropy_with_logits``` function.

Finally, the nodes for receiving training data are defined as follows.

```python
    x = tf.placeholder("float", [None, n_input])
    y_true = tf.placeholder("float", [None, n_output])
```

If our goal is to classfy digits (0~9) from MNIST data of which dimension is 28 by 28 (=784), ```n_input``` is 784 while ```n_output``` is 10.

### Summary
&nbsp;
To recap, the following is our first neural network code by using TF.

```python
import tensorflow as tf
import numpy as np

X = tf.placeholder("float", [None, n_input])
Y = tf.placeholder("float", [None, n_output])

W = tf.Variable(tf.random_normal([n_input, n_output]))
b = tf.Variable(tf.random_normal([n_output]))
y = tf.matmul(x, W) + b
y_pred = tf.nn.relu(y)

cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits(y_pred, y_true))
train_op = tf.train.GradientDescentOptimizer(0.05).minimize(cost)

with tf.Session() as sess:
    tf.initialize_all_variables().run()
    sess.run(train_op, feed_dict={X: x_data, Y: y_data})
```

As mentioned earlier, this is *not* a complete code, thus, please refer to the example codes listed above. In real applications, you need to train the neural network with **minibatch** (subset) of data and iterate the learning process(session) for **several epochs** for getting better result. Also, [Convolutional neural network][ConvNet], which is the today's new standard for image recognition problems, requires to include additional layers such as convolution layers and max pooling layers.

But the basic workflow of the TF code is same; first **define the structure** of the neural network, second, **feed training data** into the structure to optimize the model. That's it. You can **evaluate the results** by comparing predicted values with true values.

I hope this article would be helpful for you to **understand the basic workflow of TF**. Feel free to leave you feedback on [facebook][facebook] or [google plus][google plus]!

[L1]: http://web.stanford.edu/class/cs224d/lectures/CS224d-Lecture7.pdf
[L2]: http://www.slideshare.net/tw_dsconf/tensorflow-tutorial
[L3]: https://github.com/sjchoi86/Tensorflow-101
[L4]: https://github.com/nlintz/TensorFlow-Tutorials
[L5]: https://github.com/aymericdamien/TensorFlow-Examples
[L6]: https://github.com/pkmital/tensorflow_tutorials
[TF]: https://www.tensorflow.org/
[TF_Google]: https://googleblog.blogspot.ca/2015/11/tensorflow-smarter-machine-learning-for.html
[Tut1]: http://web.stanford.edu/class/cs224d/lectures/CS224d-Lecture7.pdf
[Tut2]: http://www.slideshare.net/tw_dsconf/tensorflow-tutorial
[Codes]: https://github.com/nlintz/TensorFlow-Tutorials/blob/master/3_net.py
[Img_TF]: {{site.imgurl}}/TF.png
[Opt]: https://www.tensorflow.org/versions/r0.8/api_docs/python/train.html#training
[Loss]: https://www.tensorflow.org/versions/r0.8/api_docs/python/nn.html#losses
[Img_Neuron1]: {{site.imgurl}}/Neuron.jpeg
[Img_Neuron2]: http://cs231n.github.io/convolutional-networks/
[ReLU]: https://en.wikipedia.org/wiki/Rectifier_(neural_networks)
[SoftMax]: https://en.wikipedia.org/wiki/Softmax_function
[ConvNet]: http://cs231n.github.io/convolutional-networks/
[facebook]: {{site.facebook_link}}
[google plus]: {{site.google_plus_link}}
