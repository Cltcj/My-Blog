# django

## 博客搭建第一天

**配置虚拟环境**

&emsp;&emsp;虚拟环境（virtualenv，或venv ）是 管理Python项目的利器 ，它将每个独立开来，保持环境的干净，解决包冲突问题。可以将虚拟环境理解为一个隔绝的小系统。

从Python3.3版本开始就自带了虚拟环境，不需要安装，配置一下就可以用了。

新建一个文件夹，如：myblog。进入此文件夹：

安装包
sudo apt-get install python3-blogEnv
创建虚拟环境命令
python3 -m venv blogEnv(虚拟环境名)
这个命令执行完毕后，目录中会出现一个名为venv的子目录，这里就是一个全新的虚拟环境，包含这个项目专用的Python解释器。

激活环境
source ./blogEnv/bin/activate
当盘符前有(blogEnv)标识说明进入venv成功。

退出虚拟环境
虚拟环境中的工作结束后，在命令提示符中输入deactivate，与python2的虚拟环境退出方法相似，还原当前终端会话的PATH环境变量，把命令提示符重置为最初的状态。

**安装Django**

在虚拟环境下，输入命令pip install django==2.2.3：

**创建项目**

&emsp;&emsp;执行django-admin startproject 项目名 创建对应项目文件夹

![image](https://user-images.githubusercontent.com/81791654/167246401-31ff3d06-163c-449e-bdf0-76364762c949.png)

查看项目文件夹下的内容，如果和下图一致就证明成功了：

![image](https://user-images.githubusercontent.com/81791654/167246495-39e1c01e-d53f-4f8e-8aec-107edd49cc78.png)

运行Django服务器

![image](https://user-images.githubusercontent.com/81791654/167246548-223cadcb-85bb-414c-b589-4fdfcf0d8587.png)


![image](https://user-images.githubusercontent.com/81791654/167246685-85d39223-a6f7-4605-af78-23ce0ff4a064.png)


