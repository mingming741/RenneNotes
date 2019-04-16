# pip
pip是python的packet manager

##### Check PIP version:
    pip --version
##### 使用对应python的version管理文件
    python3.6 -m pip install numpy
##### 查看安装的所有包
    pip list
##### 激活虚拟环境，activate表示对应虚拟环境的激活器
    source [path]/activate


# 虚拟环境
python自己的有自己运行的虚拟环境，因为不同的library要求python的版本不同，虚拟环境可以避免一些兼容性的问题，安装虚拟环境lib
```
sudo pip3 install virtualenv 
```
创建虚拟环境，或者指定python的版本创建虚拟环境
```
virtualenv venv 
virtualenv -p /usr/bin/python2.7 venv
```
激活虚拟环境，需要先找到虚拟环境下面的activate
```
source venv/bin/activate
```

# Pytest
pytest是用来做python系统测试的python自带的库，通过检测assert的结果来生产输出报告，这里有一些可以使用的cmd：<br/>
pytest默认会block一些print，-s可以解除这个限制
```
pytest testfile.py -s
```
