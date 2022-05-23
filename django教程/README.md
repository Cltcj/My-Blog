## 初识 Django 

Django 最初被设计用于具有快速开发需求的新闻类站点，目的是要实现简单快捷的网站开发。以下内容简要介绍了如何使用 Django 实现一个数据库驱动的 Web 应用。

Django框架的特点

相对于 Python 的其他 Web 框架，Django 的功能是最完整的，Django 定义了服务发布、路由映射、模板编程、数据处理的一整套功能。这也意味着 Django 模块之间紧密耦合。

Django 的主要特点如下：

* 完善的文档：经过 10 余年的发展和完善，Django 官方提供了完善的在线文档，为开发者解决问题提供支持。
* 集成 ORM 组件：Django 的 Model 层自带数据库 ORM 组件，为操作不同类型的数据库提供了统一的方式。
* URL 映射技术：Django 使用正则表达式管理URL映射，因此给开发者带来了极高的灵活性。
* 后台管理系统：开发者只需通过简单的几行配置和代码就可以实现完整的后台数据管理Web控制台。
* 错误信息提示：在开发调试过程中如果出现运行异常，Django 可以提供非常完整的错误信息帮助开发者定位问题。

##  Django设计模式-MTV

我们先对 MVC 设计模式进行介绍，它是 Web 设计模式的经典之作，MTV 模式也是在它的基础上衍生而来。

MVC 是 Model-View-Controller 的缩写，其中每个单词都有其不同的含义：

* Modle 代表数据存储层，是对数据表的定义和数据的增删改查；编写程序应有的功能，负责业务对象与数据库的映射(ORM)。
* View 代表视图层，是系统前端显示部分，它负责显示什么和如何进行显示；图形界面，负责与用户的交互(页面)。
* Controller 代表控制层，负责根据从 View 层输入的指令来检索 Model 层的数据，并在该层编写代码产生结果并输出。负责转发请求，对请求进行处理。

简易图
![image](https://user-images.githubusercontent.com/81791654/169685275-a9b96754-7bb7-46e1-a78d-cc2eec4cf22c.png)

用户操作流程图：
![image](https://user-images.githubusercontent.com/81791654/169685300-6851d24a-8fcd-42fa-8858-65e58ef5176e.png)



**MVC 优势：**

低耦合
开发快捷
部署方便
可重用性高
维护成本低
...

## 第一个django项目

使用 django-admin 来创建 cBlog 项目：
```py
django-admin startproject cBlog
```

创建完成后我们可以查看下项目的目录结构：
![image](https://user-images.githubusercontent.com/81791654/169727711-fac64c53-1ae8-4f79-b496-f3617cca24df.png)

目录说明：

* cBlog: 项目的容器。

* manage.py: 一个实用的命令行工具，可让你以各种方式与该 Django 项目进行交互。

* cBlog/__init__.py: 一个空文件，告诉 Python 该目录是一个 Python 包。

* cBlog/asgi.py: 一个 ASGI 兼容的 Web 服务器的入口，以便运行你的项目。

* cBlog/settings.py: 该 Django 项目的设置/配置。

* cBlog/urls.py: 该 Django 项目的 URL 声明; 一份由 Django 驱动的网站"目录"。

* cBlog/wsgi.py: 一个 WSGI 兼容的 Web 服务器的入口，以便运行你的项目。

接下来我们进入 cBlog 目录输入以下命令，启动服务器：
```py
python3 manage.py runserver
```

如果不说明，那么端口号默认为 8000。

在浏览器输入你服务器的 ip（这里我们输入本机 IP 地址： 127.0.0.1:8000） 及端口号，如果正常启动，输出结果如下：
![image](https://user-images.githubusercontent.com/81791654/169727975-d7fbe94d-1d0e-42df-9b7c-a11de07f077a.png)

## 视图和 URL 配置
在先前创建的 cBlog 目录下的 cBlog 目录新建一个 views.py 文件，并输入代码：

```py
from django.http import HttpResponse
def welcome(request):
    return HttpResponse("欢迎访问我的博客！")
```

接着，绑定 URL 与视图函数。打开 urls.py 文件，删除原来代码，将以下代码复制粘贴到 urls.py 文件中：
```py
from django.conf.urls import url
 
from . import views
 
urlpatterns = [
    url(r'^$', views.welcome),
]
```

![image](https://user-images.githubusercontent.com/81791654/169729208-bdf0ee93-4e59-410b-a737-eebfd8ed65bc.png)

完成后，启动 Django 开发服务器，并在浏览器访问打开浏览器并访问：

我们也可以修改以下规则：
![image](https://user-images.githubusercontent.com/81791654/169729559-5c844774-e97b-4846-847c-48f02ad86374.png)

注意：项目中如果代码有改动，服务器会自动监测代码的改动并自动重新载入，所以如果你已经启动了服务器则不需手动重启。

如果是 Django >= 2.0 的版本，urls.py 的 django.conf.urls 已经被 django.urls 取代。

如果是 Django >= 2.0 的版本，path() 函数无法匹配正则表达式，需要使用 re_path() 即可匹配正则表达式：

django.urls 的用法参考如下：
```py
from django.urls import path
from . import view

urlpatterns = [
    path('', view.welcome),
    path('welcome/', view.welcome)
]
```

## urls.py中path() 函数详解
Django path() 可以接收四个参数，分别是两个必选参数：route、view 和两个可选参数：kwargs、name。

语法格式：
```py
path(route, view, kwargs=None, name=None)
```

* route: 字符串，表示 URL 规则，与之匹配的 URL 会执行对应的第二个参数 view。

* view: 用于执行与正则表达式匹配的 URL 请求。

* kwargs: 视图使用的字典类型的参数。

* name: 用来反向获取 URL。

* Django2.0中可以使用 re_path() 方法来兼容 1.x 版本中的 url() 方法，一些正则表达式的规则也可以通过 re_path() 来实现 。

```py
from django.urls import include, re_path

urlpatterns = [
    re_path(r'^index/$', views.index, name='index'),
    re_path(r'^bio/(?P<username>\w+)/$', views.bio, name='bio'),
    re_path(r'^weblog/', include('blog.urls')),
    ...
]
```

## Django 模板
前面我们使用 django.http.HttpResponse() 来输出 "欢迎访问我的博客"。该方式将数据与视图混合在一起，不符合 Django 的 MVC 思想。

这里我们详细介绍 Django 模板的应用，模板是一个文本，用于分离文档的表现形式和内容。

**模板应用实例**

我们将在 cBlog 目录底下创建 templates 目录并建立 test.html文件，整个目录结构如下：

![image](https://user-images.githubusercontent.com/81791654/169734753-4a490720-e346-42da-8e80-db76de4f9e85.png)

接下来我们需要向Django说明模板文件的路径，修改cBlog/settings.py，修改 TEMPLATES 中的 DIRS 为 [os.path.join(BASE_DIR, 'templates')]，如下所示:

```py
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, 'templates')],       # 修改位置
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

我们现在修改 views.py，增加一个新的对象，用于向模板提交数据：
```py
from django.shortcuts import render
 
def test(request):
    context = {}
    context['welcome'] = '欢迎访问我的博客！'
    return render(request, 'test.html', context)
```

修改urls.py 文件代码
```py
from django.urls import path
 
from . import views
 
urlpatterns = [
    path('test/', views.test),
]
```
再运行一下manage.py
![image](https://user-images.githubusercontent.com/81791654/169738205-e89035f4-dcee-4edc-a6ef-f29b61f68b5f.png)
打开浏览器输入http://127.0.0.1:8000/
![image](https://user-images.githubusercontent.com/81791654/169738206-1524f596-2086-437c-b50c-131a37936af1.png)














