# django

## 博客搭建

**配置虚拟环境**

&emsp;&emsp;虚拟环境（virtualenv，或venv ）是 管理Python项目的利器 ，它将每个独立开来，保持环境的干净，解决包冲突问题。可以将虚拟环境理解为一个隔绝的小系统。

从Python3.3版本开始就自带了虚拟环境，不需要安装，配置一下就可以用了。

新建一个文件夹，如：myblog。进入此文件夹：

安装包

```py
sudo apt-get install python3-blogEnv
```

创建虚拟环境命令
```py
python3 -m venv blogEnv(虚拟环境名)
```
这个命令执行完毕后，目录中会出现一个名为venv的子目录，这里就是一个全新的虚拟环境，包含这个项目专用的Python解释器。

激活环境
```py
source ./blogEnv/bin/activate
```
当盘符前有(blogEnv)标识说明进入venv成功。

退出虚拟环境
虚拟环境中的工作结束后，在命令提示符中输入deactivate，与python2的虚拟环境退出方法相似，还原当前终端会话的PATH环境变量，把命令提示符重置为最初的状态。

**安装Django**

在虚拟环境下，输入命令`pip install django==2.2.3`：

**创建项目**

&emsp;&emsp;执行`django-admin startproject 项目名` 创建对应项目文件夹

![image](https://user-images.githubusercontent.com/81791654/167246401-31ff3d06-163c-449e-bdf0-76364762c949.png)

查看项目文件夹下的内容，如果和下图一致就证明成功了：

![image](https://user-images.githubusercontent.com/81791654/167246495-39e1c01e-d53f-4f8e-8aec-107edd49cc78.png)

运行Django服务器
```py
python3 manage.py runserver
```
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

输入`python manage.py startapp blogapp`，创建名为blogapp的app，再次查看有下列目录和文件。

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
```py
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
```        
        
**数据迁移（Migrations）**

编写好了Model后，接下来就需要进行数据迁移。迁移是Django对模型所做的更改传递到数据库中的方式。

注意，每当对数据库进行了更改（添加、修改、删除等）操作，都需要进行数据迁移。

Django的迁移代码是由模型文件自动生成的，它本质上只是个历史记录，Django可以用它来进行数据库的滚动更新，通过这种方式使其能够和当前的模型匹配。

在虚拟环境中进入my_blog文件夹，输入python3 manage.py makemigrations，对模型的更改创建新的迁移表：

![image](https://user-images.githubusercontent.com/81791654/167298895-88ac0081-0992-445d-b9eb-e698db3020f3.png)

![image](https://user-images.githubusercontent.com/81791654/167298910-5a900fd9-94b0-419a-9899-827c0e72f4a4.png)


## View视图初探

数据库虽然已经有了，但是用户通常只需要这个庞大数据库中的很小一部分进行查看、修改等操作。为此还需要代码来恰当的取出并展示数据，这一部分代码就被称为视图。

Django 中视图的概念是「一类具有相同功能和模板的网页的集合」。比如，在一个博客应用中，你可能会创建如下几个视图：

博客首页：展示最近的几项内容。
内容“详情”页：详细展示某项内容。
评论处理器：用于响应为一项内容添加评论的操作

这些需求都靠视图（View）来完成。

首先写一个最简单的视图函数，在浏览器中打印出Hello World!字符串。

打开blogs/views.py，写出视图函数：

![image](https://user-images.githubusercontent.com/81791654/167299158-965cfd9e-96f5-40f0-8d4e-90718a049de0.png)

网页都是从视图派生而来。每一个视图表现为一个简单的Python函数，它必须要做的只有两件事：返回一个包含被请求页面内容的 HttpResponse对象，或者抛出一个异常，比如 Http404 。至于你还想干些什么，随便你。

视图函数中的request与网页发来的请求有关，里面包含get或post的内容、用户浏览器、系统等信息。Django调用article_list函数时会返回一个含字符串的 HttpResponse对象。

有了视图函数，还需要配置URLconfs，将用户请求的URL链接关联起来。换句话说，URLconfs的作用是将URL映射到视图中。

在前面的文章中已经将URL分发给了article应用，因此这里只需要修改之前添加的article/urls.py就可以。添加以下代码

![image](https://user-images.githubusercontent.com/81791654/167299138-3fc1912c-f413-4b81-b74c-74dee5c7ec20.png)

![image](https://user-images.githubusercontent.com/81791654/167299313-442f51b4-d1f8-4f45-9639-5b9c31226e59.png)

Django 将会根据用户请求的 URL 来选择使用哪个视图。本例中当用户请求article/article-list链接时，会调用views.py中的article_list函数，并返回渲染后的对象。参数name用于反查url地址，相当于给url起了个名字，以后会用到。

测试：运行python3 manage.py runserver

成功运行后，打开浏览器，输入url地址http://127.0.0.1:8000/blogs/article-list/，其中127.0.0.1:8000是调试服务器的本地地址，blogs是项目路由my_blog\urls.py分发的地址，article-list是刚才配置的blogs\urls.py应用分发的地址。

浏览器打开：http://127.0.0.1:8000/blogs/article-list/

**准备工作**

在章节编写Model模型中虽然定义了数据库表，但是这个表是空的，不方便展示View调取数据的效果。所以在写View之前，需要往数据表里记录一些数据。接下来就做这个工作。

网站后台概念
网站后台，有时也称为网站管理后台，是指用于管理网站的一系列操作，如：数据的增加、更新、删除等。在项目开发的初期，因为没有真实的用户数据和完整的测试环境，会频繁地使用后台修改测试数据。

Django内置了一个很好的后台管理工具，只需要些少量代码，就可以实现强大的功能。

创建管理员账号（Superuser）
管理员账号（Superuser）是可以进入网站后台，对数据进行维护的账号，具有很高的权限。这里我们需要创建一个管理员账号，以便添加后续的测试数据。

虚拟环境中输入python3 manage.py createsuperuser指令，创建管理员账号：

![image](https://user-images.githubusercontent.com/81791654/167299706-a1318629-209d-4043-a057-11de721eb8d8.png)

将ArticlePost注册到后台中
接下来我们需要“告诉”Django，后台中需要添加ArticlePost这个数据表供管理。

打开article/admin.py，写入以下代码：
```py
article/admin.py

from django.contrib import admin

# 别忘了导入ArticlerPost
from .models import ArticlePost

# 注册ArticlePost到admin中
admin.site.register(ArticlePost)
```
这样就简单的注册好了。

在后台中遨游
细心的同学可能已经发现，Django项目生成的时候就自动配置好了后台的settings和url，因此不需要我们再操心了。

启动server，在浏览器中输入http://127.0.0.1:8000/admin/，一切正常的话就看到下面的登录界面了：
![image](https://user-images.githubusercontent.com/81791654/167300123-a4556546-c977-499c-b37a-c67b03226ce5.png)

登录进去，有如下界面
![image](https://user-images.githubusercontent.com/81791654/167300140-878984bd-5b50-4161-a75d-9fa0f7eb1024.png)


## 改写View视图

为了让视图真正发挥作用，改写blogs/views.py中的article_list视图函数：
```py
blogs/views.py

from django.shortcuts import render
```
# 导入数据模型ArticlePost
```py
from .models import ArticlePost

def article_list(request):
    # 取出所有博客文章
    articles = ArticlePost.objects.all()
    # 需要传递给模板（templates）的对象
    context = { 'articles': articles }
    # render函数：载入模板，并返回context对象
    return render(request, 'article/list.html', context)
```

代码同样很直白，分析如下：

`from .models import ArticlePost`从models.py中导入ArticlePost数据类

`ArticlePost.objects.all()`是数据类的方法，可以获得所有的对象（即博客文章），并传递给articles变量

context定义了需要传递给模板的上下文，这里即articles

最后返回了render函数。它的作用是结合模板和上下文，并返回渲染后的HttpResponse对象。通俗的讲就是把context的内容，加载进模板，并通过浏览器呈现。
render的变量分解如下：

request是固定的request对象，照着写就可以
blogs/list.html定义了模板文件的位置、名称
context定义了需要传入模板文件的上下文
视图函数这样就写好了。

**编写模板（template）**

在前面的视图中我们定义了模板的位置在blogs/list.html，因此在根目录下新建templates文件夹，再新建article文件夹，再新建list.html文件，即：


HTML是一种用于创建网页的标记语言。它被用来结构化信息，标注哪些文字是标题、哪些文字是正文等（当然不仅仅这点功能）。也可以简单理解为“给数据排版”的文件，跟你写文档用的Office Word一样一样的 。

在list.html文件中写入：
```html
templates/article/list.html

{% for article in articles %}
    <p>{{ article.title }}</p>
{% endfor %}
```
Django通过模板来动态生成HTML，其中就包含描述动态内容的一些特殊语法：

{% for article in articles %}：articles为视图函数的context传递过来的上下文，即所有文章的集合。{% for %}循坏表示依次取出articles中的元素，命名为article，并分别执行接下来操作。末尾用{% endfor %}告诉Django循环结束的位置。

使用.符号来访问变量的属性。这里的article为模型中的某一条文章；我们在前面的ArticlePost中定义了文章的标题叫title，因此这里可以用article.title来访问文章的标题。

<p>...</p>即为html语言，中间包裹了一个段落的文字。

再重新启动服务器:
http://127.0.0.1:8000/blogs/article-list/
是空白的，因为我们啥都没写
成功！

虽然简陋，但是已经走通了MTV（model、template、view）环路。

不要激动，精彩的还在后面。

## 使用 Bootstrap 4 改写模板文件

Bootstrap是用于网站开发的开源前端框架（“前端”指的是展现给最终用户的界面），它提供字体排印、窗体、按钮、导航及其他各种组件，旨在使动态网页和Web应用的开发更加容易。

Bootstrap有几个版本都比较流行，我们选择最新版本的Bootstrap 4：下载地址，并解压。

然后在项目根目录下新建目录static/bootstrap/，用于存放Bootstrap静态文件。静态文件通常指那些不会改变的文件。Bootstrap中的css、js文件，就是静态文件。

把刚才解压出来的css和js两个文件夹复制进去。

因为bootstrap.js依赖 jquery.js 和 popper.js 才能正常运行，因此这两个文件我们也需要一并下载保存。附上官网下载链接（进入下载页面，复制粘贴代码到新文件即可）：


因为在Django中需要指定静态文件的存放位置，才能够在模板中正确引用它们。因此在settings.py的末尾加上：

![image](https://user-images.githubusercontent.com/81791654/168419107-655d4e27-6cd8-4885-b691-858d2bf887be.png)
```py
my_blog/settings.py

...

STATICFILES_DIRS = (
    os.path.join(BASE_DIR, "static"),
)
```
再确认一下settings.py中有没有STATIC_URL = '/static/'字段，如果没有把它也加在后面。


**编写模板**

在根目录下的templates/中，新建三个文件：

base.html是整个项目的模板基础，所有的网页都从它继承；

header.html是网页顶部的导航栏；

footer.html是网页底部的注脚。

这三个文件在每个页面中通常都是不变的，独立出来可以避免重复写同样的代码，提高维护性。

现在templates\的结构像下面这个样子：

![image](https://user-images.githubusercontent.com/81791654/168419455-c14b7baf-c45b-4783-96be-ca058072c924.png)

加上之前的list.html，接下来就要重新写这4个文件了。

因为前端知识非常博大精深，并且也不是Django学习的重点，本教程不会展开篇幅去讲。如果之前没接触过前端知识也没关系，这里可以先复制粘贴，不影响后面Django的学习。

你可以试着改写其中的某段代码，看看会对页面产生什么样的影响；遇到不懂的就在Bootstrap官方文档找答案。慢慢就会明白它的运行机制，毕竟Bootstrap真的是非常简单易用的工具。

Bootstrap是非常优秀的前端框架，上手简单，所以很流行。你可以在官方网站上进行系统的学习。通篇看Bootstrap文档比较枯燥，因此建议你可以像查字典一样，需要用哪个模块，就到官网上找相关的代码，修改一下拷贝到你的项目中。

这里会一次性写大量代码，不要着急慢慢看，理解了就很简单了。

**首先写base.html：**
```html
templates/base.html

<!-- 
    载入静态文件
    使用 Django 3 学习的读者改为 {% load static %}
-->
{% load staticfiles %}

<!DOCTYPE html>
<!-- 网站主语言 -->
<html lang="zh-cn">

<head>
    <!-- 网站采用的字符编码 -->
    <meta charset="utf-8">
    <!-- 预留网站标题的位置 -->
    <title>{% block title %}{% endblock %}</title>
    <!-- 引入bootstrap的css文件 -->
    <link rel="stylesheet" href="{% static 'bootstrap/css/bootstrap.min.css' %}">
</head>

<body>
    <!-- 引入导航栏 -->
    {% include 'header.html' %}
    <!-- 预留具体页面的位置 -->
    {% block content %}{% endblock content %}
    <!-- 引入注脚 -->
    {% include 'footer.html' %}
    <!-- bootstrap.js 依赖 jquery.js 和popper.js，因此在这里引入 -->
    <script src="{% static 'jquery/jquery-3.3.1.js' %}"></script>

    <!-- 
        popper.js 采用 cdn 远程引入，意思是你不需要把它下载到本地。
        在实际的开发中推荐静态文件尽量都使用 cdn 的形式。 
        教程采用本地引入是为了让读者了解静态文件本地部署的流程。
    -->
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1-lts/dist/umd/popper.min.js"></script>

    <!-- 引入bootstrap的js文件 -->
    <script src="{% static 'bootstrap/js/bootstrap.min.js' %}"></script>
</body>

</html>
```
