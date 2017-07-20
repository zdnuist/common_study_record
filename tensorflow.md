##### tensorflow
github地址 https://github.com/tensorflow/tensorflow
下载相应的whl文件
以下以Unbuntu为例 tensorflow-1.2.1-cp27-none-linux_x86_64.whl
python版本2.7
```
//安装pip
apt install python-pip
//更新pip
pip install --upgrade pip
//安装tensorflow
pip install tensorflow-1.2.1-cp27-none-linux_x86_64.whl
```

编写用例[tf1.py]
```
import tensorflow as tf
hello = tf.constant('hello tensorflow')
session = tf.Session()
print session.run(hello)
```
