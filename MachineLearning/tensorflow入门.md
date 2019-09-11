# tensorflow超级无敌简明入门三天速成装逼教程

来自bilibili莫烦python教程https://www.bilibili.com/video/av16001891/?p=9

---

#### 什么是tensorflow？

中外翻译来就是==向量飞==？？？
首先建立一个结构，然后数据放入结构，tensorflow自己运行

---

#### 代码形式实现神经网络结构

```python
import tensorflow as tf
import numpy as np

# create data
x_data = np.random.rand(100).astype(np.float32)
y_data = x_data*0.1 + 0.3

### create tensorflow structure start ###
Weights = tf.Variable(tf.random_uniform([1], -1.0, 1.0))
biases = tf.Variable(tf.zeros([1]))

y = Weights*x_data + biases

loss = tf.reduce_mean(tf.square(y-y_data))
optimizer = tf.train.GradientDescentOptimizer(0.5)
train = optimizer.minimize(loss)

init = tf.initialize_all_vaiables()
### create tensorflow structure end ###

sess = tf.Session()
sess.run(init)    # Very important 必须激活变量

# 开始训练
for step in range(201):
    sess.run(train)
    if step % 20 == 0:
        print(setp, sess.run(Weights), sess.run(biases))
```















































