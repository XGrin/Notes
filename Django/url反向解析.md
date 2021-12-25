## URL反向解析

### 代码中url出现位置

+   模板(html)中

    1.   `<a href = 'url>超链接</a>'`

         点击后页面跳转至url

    2.   `<form action = 'url' method = 'post'>`

         form表单中的数据 用post 方法提交至url

+   视图函数中 - 302跳转 `HttpResponseRedirect('url')`

    将用户地址栏中地址跳转至url



### url反向解析

url反向解析是指在视图或模板中,用path定义的名称来**动态查找或计算出响应的路由**

path函数的语法

+   `path(route,views,name='别名')`
+   `path('page',views.page_view,name='page_url')`

根据path中的`name=`关键字传参给url确定了一个唯一确定的名字,在模板或视图中,可以通过这个名字反向推断出此url信息

模板中通过url标签实现地址的反向解析

```html
{% url '别名' %}
{% url '别名' '参数1' '参数2' %}

如:
<a href="{% url 'abc'}">url反向解析</a>
```

在视图函数中可调用django中的reverse方法进行反向解析

```python
from django.urls import reverse
reverse('别名',args=[],kwargs={})

# 例子
print(reverse('pagen',args=[300]))
print(reverse('person',kwargs={'name':'zhangsan','age':18}))
```

