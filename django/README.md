# django

## 博客搭建

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

python3 manage.py runserver

![image](https://user-images.githubusercontent.com/81791654/167246548-223cadcb-85bb-414c-b589-4fdfcf0d8587.png)

系统打印出这些信息，说明服务器启动成功了，打开chrome浏览器，输入http://127.0.0.1:8000/ ，即倒数第2排信息提示我们的服务器地址。看到下面的界面：
![image](https://user-images.githubusercontent.com/81791654/167246685-85d39223-a6f7-4605-af78-23ce0ff4a064.png)

django 默认的语言是英语，所以显示给我们的欢迎页面是英文的。我们在 django 的配置文件里稍作修改，让它支持中文。用任何一个文本编辑器打开 settings.py 文件，找到如下的两行代码：
![image](https://user-images.githubusercontent.com/81791654/167247055-3eab3d87-4da9-457a-b5cb-aed51fcadf3c.png)

修改完之后，我们再执行python3 manage.py runserver，然后在本地浏览器输入127.0.0.1:8000得到中文的界面：
![image](https://user-images.githubusercontent.com/81791654/167247085-e04888bd-a2aa-4ac6-a358-f29f221e8bba.png)


## 创建并配置APP功能模块

**创建APP**

在Django中的一个app代表一个功能模块。开发者可以将不同功能的模块放在不同的app中, 方便代码的复用。app就是项目的基石，因此开发博客的第一步就是创建新的app，用来实现跟文章相关的功能模块。

打开命令行，进入项目所在的目录：（注意Django的操作必须在虚拟环境下进行）

输入python manage.py startapp blogapp，创建名为blogapp的app，再次查看有下列目录和文件。

![image](https://user-images.githubusercontent.com/81791654/167247362-31db6fc4-f15e-4b76-9442-a130321484bd.png)


