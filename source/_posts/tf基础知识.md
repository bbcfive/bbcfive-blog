---
title: tensorflow基础知识
date: 2018-06-22 09:55:12
tags: 
  - python
  - tensorflow
categories: 机器学习  
---

## 一、使用session
tf.Session()是为了运行数据，所有定义的数据如果没有放进session里是不会被执行的

``` py
import tensorflow as tf

matrix1 = tf.constant([[3, 3]])
matrix2 = tf.constant([[2],[2]])
product = tf.matmul(matrix1, matrix2)  # matrix multiply np.dot(m1, m2)

# method 1
sess = tf.Session()
result = sess.run(product)
print(result)
sess.close()

# method 2
with tf.Session() as sess:
  result2 = sess.run(product)
  print(result2)
```

## 二、Variables
定义一个变量并使其做自加操作
``` py
import tensorflow as tf

state = tf.Variable(0, name='counter')

one = tf.constant(1)

new_value = tf.add(state, one)
update = tf.assign(state, new_value)

init = tf.global_variables_initializer()

with tf.Session() as sess:
  sess.run(init)
  for _ in range(3):
    sess.run(update)
    print(sess.run(state))
```

## 三、placeholder
变量占位符，使用placeholder定义，并不用给出确切值，而后feed_dict准确数据
``` py
import tensorflow as tf

input1 = tf.placeholder(tf.float32)
input2 = tf.placeholder(tf.float32)
output = tf.multiply(input1, input2)

with tf.Session() as sess:
  print(sess.run(output, feed_dict={input1: [7.], input2: [2.]}))
```

本文是对[周沫凡](https://morvanzhou.github.io/tutorials/)同学tf课程的学习笔记记录。