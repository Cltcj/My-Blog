## 初识 Django 

`Django` 最初被设计用于具有快速开发需求的新闻类站点，目的是要实现简单快捷的网站开发。以下内容简要介绍了如何使用 `Django` 实现一个数据库驱动的 `Web` 应用。

`Django` 框架的特点

相对于 `Python` 的其他 `Web` 框架，`Django` 的功能是最完整的，`Django` 定义了服务发布、路由映射、模板编程、数据处理的一整套功能。这也意味着 `Django` 模块之间紧密耦合。

`Django` 的主要特点如下：

* 完善的文档：经过 10 余年的发展和完善，`Django` 官方提供了完善的在线文档，为开发者解决问题提供支持。
* 集成 `ORM` 组件：`Django` 的 `Model` 层自带数据库 `ORM` 组件，为操作不同类型的数据库提供了统一的方式。
* `URL` 映射技术：`Django` 使用正则表达式管理 `URL` 映射，因此给开发者带来了极高的灵活性。
* 后台管理系统：开发者只需通过简单的几行配置和代码就可以实现完整的后台数据管理 `Web` 控制台。
* 错误信息提示：在开发调试过程中如果出现运行异常，`Django` 可以提供非常完整的错误信息帮助开发者定位问题。

##  Django设计模式-MTV

我们先对 `MVC` 设计模式进行介绍，它是 `Web` 设计模式的经典之作，`MTV` 模式也是在它的基础上衍生而来。

`MVC` 是 `Model-View-Controller` 的缩写，其中每个单词都有其不同的含义：

* `Modle` 代表数据存储层，是对数据表的定义和数据的增删改查；编写程序应有的功能，负责业务对象与数据库的映射(`ORM`)。
* `View` 代表视图层，是系统前端显示部分，它负责显示什么和如何进行显示；图形界面，负责与用户的交互(页面)。
* `Controller` 代表控制层，负责根据从 `View` 层输入的指令来检索 `Model` 层的数据，并在该层编写代码产生结果并输出。负责转发请求，对请求进行处理。

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

使用 `django-admin` 来创建 `cBlog` 项目：
```py
django-admin startproject cBlog
```

创建完成后我们可以查看下项目的目录结构：
![image](https://user-images.githubusercontent.com/81791654/169727711-fac64c53-1ae8-4f79-b496-f3617cca24df.png)

目录说明：

* `cBlog`: 项目的容器。

* `manage.py`: 一个实用的命令行工具，可让你以各种方式与该 Django 项目进行交互。

* `cBlog/__init__.py`: 一个空文件，告诉 `Python` 该目录是一个 `Python` 包。

* `cBlog/asgi.py`: 一个 `ASGI` 兼容的 `Web` 服务器的入口，以便运行你的项目。

* `cBlog/settings.py`: 该 `Django` 项目的设置/配置。

* `cBlog/urls.py`: 该 `Django` 项目的 `URL` 声明; 一份由 `Django` 驱动的网站"目录"。

* `cBlog/wsgi.py`: 一个 `WSGI` 兼容的 `Web` 服务器的入口，以便运行你的项目。

接下来我们进入 `cBlog` 目录输入以下命令，启动服务器：
```py
python3 manage.py runserver
```

如果不说明，那么端口号默认为 `8000`。

在浏览器输入你服务器的 `ip`（这里我们输入本机 `IP` 地址： 127.0.0.1:8000） 及端口号，如果正常启动，输出结果如下：
![image](https://user-images.githubusercontent.com/81791654/169727975-d7fbe94d-1d0e-42df-9b7c-a11de07f077a.png)

## 视图和 URL 配置
在先前创建的 `cBlog` 目录下的 `cBlog` 目录新建一个 `views.py` 文件，并输入代码：

```py
from django.http import HttpResponse
def welcome(request):
    return HttpResponse("欢迎访问我的博客！")
```

接着，绑定 `URL` 与视图函数。打开 `urls.py` 文件，删除原来代码，将以下代码复制粘贴到 `urls.py` 文件中：
```py
from django.conf.urls import url
 
from . import views
 
urlpatterns = [
    url(r'^$', views.welcome),
]
```

![image](https://user-images.githubusercontent.com/81791654/169729208-bdf0ee93-4e59-410b-a737-eebfd8ed65bc.png)

完成后，启动 `Django` 开发服务器，并在浏览器访问打开浏览器并访问：

我们也可以修改以下规则：
![image](https://user-images.githubusercontent.com/81791654/169729559-5c844774-e97b-4846-847c-48f02ad86374.png)

注意：项目中如果代码有改动，服务器会自动监测代码的改动并自动重新载入，所以如果你已经启动了服务器则不需手动重启。

如果是 `Django >= 2.0` 的版本，`urls.py` 的 `django.conf.urls` 已经被 `django.urls` 取代。

如果是 `Django >= 2.0` 的版本，`path()` 函数无法匹配正则表达式，需要使用 `re_path()` 即可匹配正则表达式：

`django.urls` 的用法参考如下：
```py
from django.urls import path
from . import view

urlpatterns = [
    path('', view.welcome),
    path('welcome/', view.welcome)
]
```

## `urls.py中path()` 函数详解
`Django path()` 可以接收四个参数，分别是两个必选参数：`route、view` 和两个可选参数：`kwargs、name`。

语法格式：
```py
path(route, view, kwargs=None, name=None)
```

* `route`: 字符串，表示 `URL` 规则，与之匹配的 `URL` 会执行对应的第二个参数 `view`。

* `view`: 用于执行与正则表达式匹配的 `URL` 请求。

* `kwargs`: 视图使用的字典类型的参数。

* `name`: 用来反向获取 `URL`。

* `Django2.0` 中可以使用 `re_path()` 方法来兼容 1.x 版本中的 `url()` 方法，一些正则表达式的规则也可以通过 `re_path()` 来实现 。

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
前面我们使用 `django.http.HttpResponse()` 来输出 "欢迎访问我的博客"。该方式将数据与视图混合在一起，不符合 `Django` 的 `MVC` 思想。

这里我们详细介绍 `Django` 模板的应用，模板是一个文本，用于分离文档的表现形式和内容。

**模板应用实例**

我们将在 `cBlog` 目录底下创建 `templates` 目录并建立 `test.html` 文件，整个目录结构如下：

![image](https://user-images.githubusercontent.com/81791654/169734753-4a490720-e346-42da-8e80-db76de4f9e85.png)

接下来我们需要向 `Django` 说明模板文件的路径，修改 `cBlog/settings.py`，修改 `TEMPLATES` 中的 `DIRS` 为 `[os.path.join(BASE_DIR, 'templates')]`，如下所示:

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

我们现在修改 `views.py`，增加一个新的对象，用于向模板提交数据：
```py
def hello(request):
    context = {}
    context['hello'] = 'Hello World!'
    return render(request, 'hello.html', context)                                                     
```

修改 `urls.py` 文件代码
```py
from django.urls import path
 
from . import views
 
urlpatterns = [
    path('hello/', hello.test),
]
```

再运行一下 `manage.py`

`python3 manage.py runserver`

打开浏览器输入http://127.0.0.1:8000/hello/
![image](https://user-images.githubusercontent.com/81791654/169740625-af751b48-3b56-445e-850e-1155ee56ddab.png)

可以看到，我们这里使用 `render` 来替代之前使用的 `HttpResponse`。`render` 还使用了一个字典 `context` 作为参数。

`context` 字典中元素的键值 `hello` 对应了模板中的变量 `{{ hello }}`。



这样我们就完成了使用模板来输出数据，从而实现数据与视图分离。

接下来我们将具体介绍模板中常用的语法规则。

## Django 模板标签

**变量**

**模板语法：**

```py
view：｛"HTML变量名" : "views变量名"｝
HTML：｛｛变量名｝｝
```

增加 `views.py` 的文件代码

```py
from django.shortcuts import render

def thanks(request):
  views_name = "谢谢你"
  return  render(request,"thanks.html", {"name":views_name})
```

再增加 'urls.py' 的文件代码

```html
path('thanks/', views.thanks),
```

在 `templates` 文件夹中增加的 `thanks.html` 内容为：

```html
<p>{{ name }}</p>
```

再次访问 http://127.0.0.1:8000/thanks，可以看到页面：

![image](https://user-images.githubusercontent.com/81791654/169764789-51f66151-ca4a-4783-aac3-e6786a59d29d.png)

**列表**

将 `views.py` 中的代码改写为：

```py
from django.shortcuts import render

def thanks(request):
    views_list = ["谢谢你","谢谢你","谢谢你"]
    return render(request, "thanks.html", {"views_list": views_list})
```

`templates` 中的 `thanks.html`中，可以用 . 索引下标取出对应的元素。

在`thanks.html`中改写：

```html
<p>{{ views_list }}</p>   # 取出整个列表
<p>{{ views_list.0 }}</p> # 取出列表的第一个元素
```

![image](https://user-images.githubusercontent.com/81791654/169767573-d07a4893-8693-45f3-96bb-6a6031d1507c.png)

**字典**

```py
from django.shortcuts import render

def thanks(request):
    views_dict = {"name":"谢谢你"}
    return render(request, "thanks.html", {"views_dict": views_dict})
```

`templates` 中的 `thanks.html` 中，可以用 .键 取出对应的值。

```html
<p>{{ views_dict }}</p>
<p>{{ views_dict.name }}</p>
```
再次访问 http://127.0.0.1:8000/thanks，可以看到页面：

![image](https://user-images.githubusercontent.com/81791654/169769051-c34e88e0-bd4b-41a8-a503-790bb1a865a1.png)

**过滤器**

模板语法：

`{{ 变量名 | 过滤器：可选参数 }}`

模板过滤器可以在变量被显示前修改它，过滤器使用管道字符，如下所示：

`{{ name|lower }}`

{{ name }} 变量被过滤器 lower 处理后，文档大写转换文本为小写。

过滤管道可以被* 套接* ，既是说，一个过滤器管道的输出又可以作为下一个管道的输入：

`{{ my_list|first|upper }}`

以上实例将第一个元素并将其转化为大写。

有些过滤器有参数。 过滤器的参数跟随冒号之后并且总是以双引号包含。 例如：

`{{ bio|truncatewords:"30" }}`

这个将显示变量 bio 的前30个词。

其他过滤器：

* addslashes : 添加反斜杠到任何反斜杠、单引号或者双引号前面。

* date : 按指定的格式字符串参数格式化 date 或者 datetime 对象，实例：`{{ pub_date|date:"F j, Y" }}`


**default**

default 为变量提供一个默认值。

如果 views 传的变量的布尔值是 false，则使用指定的默认值。

以下值为 false：

`0  0.0  False  0j  ""  []  ()  set()  {}  None`

```py
from django.shortcuts import render

def thanks(request):
    name = 0
    return render(request, "thanks.html", {"name": name})
```

修改thanks.html为：
```html
{{ name|default:"谢谢你" }}
```

**length**

返回对象的长度，适用于字符串和列表。

字典返回的是键值对的数量，集合返回的是去重后的长度。

修改views.py的文件代码
```py
from django.shortcuts import render

def thanks(request):
    name ="谢谢你"
    return render(request, "thanks.html", {"name": name})
```

修改thanks.html文件代码

```html
{{ name|length}}
```

**filesizeformat**

以更易读的方式显示文件的大小（即'13 KB', '4.1 MB', '102 bytes'等）。

字典返回的是键值对的数量，集合返回的是去重后的长度。

修改views.py的文件代码
```py
from django.shortcuts import render

def thanks(request):
    num=1024
    return render(request, "thanks.html", {"num": num})
```

修改thanks.html文件代码
```html
{{ num|filesizeformat}}
```
再次访问 http://127.0.0.1:8000/thanks，可以看到页面：

**date**

根据给定格式对一个日期变量进行格式化。

格式 `Y-m-d H:i:s` 返回 年-月-日 小时:分钟:秒 的格式时间。

修改views.py的文件代码
```py
from django.shortcuts import render

def thanks(request):
    import datetime
    now = datetime.datetime.now()
    return render(request, "thanks.html", {"time": now})
```

修改thanks.html文件代码
```html
{{ time|date:"Y-m-d" }}
```

再次访问 http://127.0.0.1:8000/thanks，可以看到页面

**truncatechars**

如果字符串包含的字符总个数多于指定的字符数量，那么会被截断掉后面的部分。

截断的字符串将以 ... 结尾。

修改views.py的文件代码
```py
from django.shortcuts import render

def thanks(request):
    views_str = "菜鸟教程"
    return render(request, "thanks.html", {"views_str": views_str})
```

修改thanks.html文件代码
```html
{{ views_str|truncatechars:2}}
```

**safe**

将字符串标记为安全，不需要转义。

要保证 views.py 传过来的数据绝对安全，才能用 safe。

和后端 views.py 的 mark_safe 效果相同。

Django 会自动对 views.py 传到HTML文件中的标签语法进行转义，令其语义失效。加 safe 过滤器是告诉 Django 该数据是安全的，不必对其进行转义，可以让该数据语义生效。

在增加中views.py的文件代码
```py
def mygithub(request):
    views_str = "<a href='https://github.com/Cltcj/'>点击跳转</a>"
    return render(request, "mygithub.html", {"views_str": views_str})
```

增加mygithub.html文件代码
```html
{{ views_str|safe }}
```

**if/else 标签**

基本语法格式如下：
```html
{% if condition %}
     ... display
{% endif %}
```
或者：
```html
{% if condition1 %}
   ... display 1
{% elif condition2 %}
   ... display 2
{% else %}
   ... display 3
{% endif %}
```

根据条件判断是否输出。`if/else`支持嵌套。
```html
`{% if %}` 标签接受 `and` ， `or` 或者 `not` 关键字来对多个变量做判断 ，或者对变量取反`（not）`，例如：

{% if athlete_list and coach_list %}
     athletes 和 coaches 变量都是可用的。
{% endif %}
```

修改`views.py` 文件代码：
```py
from django.shortcuts import render

def thanks(request):
    views_num = 88
    return render(request, "thanks.html", {"num": views_num})
```

修改thanks.html 文件代码：
```html
{%if num > 90 and num <= 100 %}
优秀
{% elif num > 60 and num <= 90 %}
合格
{% else %}
一边玩去～
{% endif %}
```

再访问访问 http://127.0.0.1:8000/thanks，可以看到页面：

**for 标签**
`{% for %}` 允许我们在一个序列上迭代。

与 Python 的 for 语句的情形类似，循环语法是 `for X in Y` ，Y 是要迭代的序列而 X 是在每一个特定的循环中使用的变量名称。

每一次循环中，模板系统会渲染在 `{% for %}` 和 `{% endfor %}` 之间的所有内容。

例如，给定一个运动员列表 athlete_list 变量，我们可以使用下面的代码来显示这个列表：
```html
<ul>
{% for athlete in athlete_list %}
    <li>{{ athlete.name }}</li>
{% endfor %}
</ul>
```

修改views.py 文件代码：
```py
from django.shortcuts import render

def thanks(request):
    views_list = ["谢谢你","谢谢你1","谢谢你2","谢谢你3",]
    return render(request, "thanks.html", {"views_list": views_list})
```

改变`thanks.html` 文件代码：
```html
{% for i in views_list %}
{{ i }}
{% endfor %}
```
[参考：](https://www.runoob.com/django/django-template.html)


### csrf_token

`csrf_token` 用于form表单中，作用是跨站请求伪造保护。

如果不用 `{% csrf_token %}` 标签，在用 form 表单时，要再次跳转页面会报 403 权限错误。

用了`{% csrf_token %}` 标签，在 form 表单提交数据时，才会成功。

解析：

首先，向服务器发送请求，获取登录页面，此时中间件 `csrf` 会自动生成一个隐藏input标签，该标签里的 value 属性的值是一个随机的字符串，用户获取到登录页面的同时也获取到了这个隐藏的input标签。

然后，等用户需要用到form表单提交数据的时候，会携带这个 input 标签一起提交给中间件 csrf，原因是 form 表单提交数据时，会包括所有的 input 标签，中间件 csrf 接收到数据时，会判断，这个随机字符串是不是第一次它发给用户的那个，如果是，则数据提交成功，如果不是，则返回403权限错误。

### 自定义标签和过滤器

1、在应用目录下创建 ariticle 目录(与 templates 目录同级，目录名只能是 ariticle)。

  










