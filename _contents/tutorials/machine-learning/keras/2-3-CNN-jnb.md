---
youku_id: XMTc4MDEyMDk0MA
youtube_id: zHop6Oq757Y
title: CNN 卷积神经网络
description: CNN 一般用来处理图片. 他在图片识别上有很多优势.
author: Mark JingNB
publish-date: 2016-10-30
chapter: 2
thumbnail: "/static/thumbnail/keras/06 CNN.jpg"
---
* 学习资料:
  * [代码链接](https://github.com/MorvanZhou/tutorials/blob/master/kerasTUT/6-CNN_example.py)
  * 机器学习-简介系列 [CNN 简介]({% link _contents/tutorials/machine-learning/ML-intro/1-3-CNN.md %})
  * Tensorflow [CNN]({% link _contents/tutorials/machine-learning/tensorflow/5-04-CNN2.md %})
  
这次我们主要讲CNN（Convolutional Neural Networks）卷积神经网络在keras上的代码实现。
用到的数据集还是MNIST。不同的是这次用到的层比较多，导入的模块也相应增加了一些。

```python
from keras.layers import Dense, Activation, Convolution2D, MaxPooling2D, Flatten
```
首先是数据预处理和model的设置。
然后添加第一个卷积层，滤波器数量为32，大小是5*5，Padding方法是same即不改变数据的长度和宽带。
因为是第一层所以需要说明输入数据的shape，激励选择relu函数。代码如下

```python
model.add(Convolution2D(
    nb_filter=32,
    nb_row=5,
    nb_col=5,
    border_mode='same',     # Padding method
    dim_ordering='th',      
    input_shape=(1,         # channels
                 28, 28,)    # height & width
))
model.add(Activation('relu'))
```

第一层pooling（池化，下采样），分辨率长宽各降低一半，输出数据shape为（32，14，14）

```python
model.add(MaxPooling2D(
    pool_size=(2, 2),
    strides=(2, 2),
    border_mode='same',    # Padding method
))
```

再添加第二卷积层和池化层

```python
model.add(Convolution2D(64, 5, 5, border_mode='same'))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2), border_mode='same'))
```
经过以上处理之后数据shape为（64，7，7），需要将数据抹平成一维，再添加全连接层1

```python
model.add(Flatten())
model.add(Dense(1024))
model.add(Activation('relu'))
```

添加全连接层2（即输出层）

```python
model.add(Dense(10))
model.add(Activation('softmax'))
```

设置优化方法，loss函数和metrics方法

```python
model.compile(optimizer=adam,
              loss='categorical_crossentropy',
              metrics=['accuracy'])
```

开始训练模型

```python
model.fit(X_train, y_train, nb_epoch=1, batch_size=32,)             
```

输出test的loss和accuracy结果

<img class="course-image" src="/static/results/keras/2-3-1.png">
