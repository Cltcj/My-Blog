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


项目结构分解如下：

根目录my_blog下有两个文件：db.sqlite3是一个轻量级的数据库文件，用来存储项目产生的数据，比如博客文章；manage.py是项目执行命令的入口，比如runserver。

目录blogapp是刚创建出来的app，用来放置博客文章相关的代码：后台管理文件admin.py，数据模型文件models.py，视图文件views.py，存放数据迁移文件的目录migrations。

根目录下还有一个my_blog目录，其中的settings.py包含项目的配置参数，urls.py则是项目的根路由文件。


**注册APP（settings）**

接着我们需要修改项目配置文件，“告诉”Django现在有blogapp这么一个app了。

打开my_blog目录的settings.py，找到INSTALLED_APPS写入如下代码：

![image](https://user-images.githubusercontent.com/81791654/167247575-3b4b7825-14ac-4cda-8928-1918dd9d700d.png)

**配置访问路径（urls）**

然后再给app配置访问路径url。

url可以理解为访问网站时输入的网址链接，配置好url后Django才知道怎样定位app。

打开my_blog目录下的urls.py，增加以下代码：

![image](https://user-images.githubusercontent.com/81791654/167247708-04ce2854-0f0d-47c2-8c99-7f5e073c43c8.png)

path为Django的路由语法：

参数blogapp/分配了app的访问路径；
include将路径分发给下一步处理；
namespace可以保证反查到唯一的url，即使不同的app使用了相同的url（后面会用到）。

在开发环境下，blogapp的url为：http://127.0.0.1:8000/blogapp/

现在我们已经通过path将根路径为blogapp的访问都分发给blogapp这个app去处理。但是app通常有多个页面地址，因此还需要app自己也有一个路由分发，也就是blogapp.urls了。

>article可以有多个页面，如列表页面、详情页面等，那么就需要如下两个url：
http://127.0.0.1:8000/blogapp/list/
http://127.0.0.1:8000/blogapp/detail/
app 中的 urls.py 就是用来区分它们的。

![image](https://user-images.githubusercontent.com/81791654/167248074-ece15562-13bc-472a-b1ea-2ea69328e8ea.png)
\
urlpatterns中暂时是空的，没写入任何路径的映射，不着急以后会写。

此时我们的app就配置完成了。

注意此时app还没有写好，因此启动服务器可能会报错，是正常的。

Django2.0之后，app的urls.py必须配置app_name，否则会报错。

## 编写博客文章的Model模型

Django 框架主要关注的是模型（Model）、模板（Template）和视图（Views），称为MTV模式。

它们各自的职责如下：
层次	职责
模型（Model），即数据存取层	处理与数据相关的所有事务： 如何存取、如何验证有效性、包含哪些行为以及数据之间的关系等。
模板（Template），即业务逻辑层	处理与表现相关的决定： 如何在页面或其他类型文档中进行显示。
视图（View），即表现层	存取模型及调取恰当模板的相关逻辑。模型与模板的桥梁。

**简单来说就是Model存取数据，View决定需要调取哪些数据，而Template则负责将调取出的数据以合理的方式展现出来。**

**数据库与模型**

数据库是存储电子文件的场所，储存独立的数据集合。一个数据库由多个数据表构成

默认情况下，数据库就是db.sqlite3这个文件了。在网站上线后你可能想换别的数据库，不过目前我们还不需要讨论这个内容。

操作数据库使用的是复杂的SQL语句，它是完全不同于Python的另一种语言，这对新手来说无疑是困难的。

幸运的是，在 Django 里写Web应用并不需要你直接去操作数据库，而是定义好模型（用Python语法就可以了！），模型中包含了操作数据库所必要的命令。也就是说你只需要定义数据模型，其它的底层代码都不用关心，它们会自动从模型生成

>其实它有专门的术语，叫对象关系映射（Object Relational Mapping，简称ORM），用于实现面向对象编程语言里不同类型系统的数据之间的转换。

**编写Model.py**

如前面所讲，Django中通过模型（Model）映射到数据库，处理与数据相关的事务。

对博客网站来说，最重要的数据就是文章。所以首先来建立一个存放文章的数据模型。

打开blogapp/models.py文件，输入如下代码：


![image](https://user-images.githubusercontent.com/81791654/167255475-1430795e-6dd7-4081-b702-e49c7eb0620b.png)

代码非常直白。

每个模型都被表示为 django.db.models.Model 类的子类，从它继承了操作数据库需要的所有方法。

每个字段都是 Field 类的实例 。比如字符字段被表示为 CharField ，日期时间字段被表示为 DateTimeField。这将告诉Django要处理的数据类型。

定义某些 Field 类实例需要参数。例如 CharField 需要一个 max_length参数。这个参数的用处不止于用来定义数据库结构，也用于验证数据。

使用 ForeignKey定义一个关系。这将告诉 Django，每个（或多个） ArticlePost 对象都关联到一个 User 对象。

Django具有一个简单的账号系统（User），满足一般网站的用户相关的基本功能。

ArticlePost类定义了一篇文章所必须具备的要素：作者、标题、正文、创建时间以及更新时间。

我们还可以额外再定义一些内容，规范ArticlePost中数据的行为：

![image](https://user-images.githubusercontent.com/81791654/167255553-a494b02e-d3ac-4546-a5e3-b314aa5ac63b.png)

内部类Meta中的ordering定义了数据的排列方式。-created表示将以创建时间的倒序排列，保证了最新的文章总是在网页的最上方。注意ordering是元组，括号中只含一个元素时不要忘记末尾的逗号。

__str__方法定义了需要表示数据时应该显示的名称。给模型增加 __str__方法是很重要的，它最常见的就是在Django管理后台中做为对象的显示值。因此应该总是返回一个友好易读的字符串。

完整代码：

from django.db import models
from django.contrib.auth.models import User
from django.utils import timezone

class ArticlePost(models.Model):
    author = models.ForeignKey(User, on_delete=models.CASCADE)
    title = models.CharField(max_length=100)
    body = models.TextField()
    created = models.DateTimeField(default=timezone.now)
    updated = models.DateTimeField(auto_now=True)

    class Meta:
        ordering = ('-created',)

    def __str__(self):
        return self.title
        
        
**数据迁移（Migrations）**

编写好了Model后，接下来就需要进行数据迁移。迁移是Django对模型所做的更改传递到数据库中的方式。

注意，每当对数据库进行了更改（添加、修改、删除等）操作，都需要进行数据迁移。

Django的迁移代码是由模型文件自动生成的，它本质上只是个历史记录，Django可以用它来进行数据库的滚动更新，通过这种方式使其能够和当前的模型匹配。

在虚拟环境中进入my_blog文件夹，输入python3 manage.py makemigrations，对模型的更改创建新的迁移表：
