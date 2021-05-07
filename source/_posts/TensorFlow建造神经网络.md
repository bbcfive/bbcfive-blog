---
title: TensorFlow建造神经网络
date: 2018-06-23 10:55:12
tags: 
  - python
  - TensorFlow
categories: 机器学习  
---
## 一、激励函数
在神经网络中，隐层和输出层节点的输入和输出之间具有函数关系，这个函数称为激励函数。
常见的激励函数有：relu、sigmoid和tanh
少量层时可以随便选择；多量层时必须慎重考虑，否则可能会出现梯度爆炸或梯度消失现象。
卷积神经网络时一般选择relu，循环神经网络一般选择relu 或者 tanh。
![avatar](https://morvanzhou.github.io/static/results/torch/2-3-1.png)

## 二、添加层
``` py 
import tensorflow as tf

def add_layer(inputs, in_size, out_size, activation_function=None):
  Weights = tf.Variable(tf.random_normal([in_size, out_size]))
  biases = tf.Variable(tf.zeros([1, out_size]) + 0.1)
  Wx_plus_b = tf.matmul(inputs, Weights) + biases
  if activation_function is None:
    outputs = Wx_plus_b
  else:
    outputs = activation_function(Wx_plus_b)
  return outputs
```

## 三、定义输入层、隐藏层并打印输出层
``` py
import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt

# Make up some real data
x_data = np.linspace(-1, 1, 300, dtype=np.float32)[:, np.newaxis]
noise = np.random.normal(0, 0.05, x_data.shape).astype(np.float32)
y_data = np.square(x_data) - 0.5 + noise

##plt.scatter(x_data, y_data)
##plt.show()

# define placeholder for inputs to network
xs = tf.placeholder(tf.float32, [None, 1])
ys = tf.placeholder(tf.float32, [None, 1])
# add hidden layer
l1 = add_layer(xs, 1, 10, activation_function=tf.nn.relu)
# add output layer
prediction = add_layer(l1, 10, 1, activation_function=None)

# the error between prediction and real data
loss = tf.reduce_mean(tf.reduce_sum(tf.square(ys-prediction), reduction_indices=[1]))
train_step = tf.train.GradientDescentOptimizer(0.1).minimize(loss)
# important step
sess = tf.Session()
# tf.initialize_all_variables() no long valid from
# 2017-03-02 if using tensorflow >= 0.12
if int((tf.__version__).split('.')[1]) < 12 and int((tf.__version__).split('.')[0]) < 1:
  init = tf.initialize_all_variables()
else:
  init = tf.global_variables_initializer()
sess.run(init)

for i in range(1000):
  # training
  sess.run(train_step, feed_dict={xs: x_data, ys: y_data})
  if i % 50 == 0:
    # to see the step improvement
    print(sess.run(loss, feed_dict={xs: x_data, ys: y_data}))
```

## 四、数据动态化展示拟合过程
``` py
import tensorflow as tf
import numpy as np
import matplotlib.pyplot as plt

# plot the real data
fig = plt.figure()
ax = fig.add_subplot(1,1,1)
ax.scatter(x_data, y_data)
plt.ion()
plt.show()


for i in range(1000):
  # training
  sess.run(train_step, feed_dict={xs: x_data, ys: y_data})
  if i % 50 == 0:
    # to visualize the result and improvement
    try:
      ax.lines.remove(lines[0])
    except Exception:
      pass
    prediction_value = sess.run(prediction, feed_dict={xs: x_data})
    # plot the prediction
    lines = ax.plot(x_data, prediction_value, 'r-', lw=5)
    plt.pause(1)
```
        

本文是对[周沫凡](https://morvanzhou.github.io/tutorials/)同学tf课程的学习笔记记录。