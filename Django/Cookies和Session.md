### 会话

+   从打开浏览器访问一个网站,到关闭浏览器结束这次访问,称之为一次会话
+   HTTP协议是无状态的,导致会话状态难以保持
+   Cookies和Session就是为了保持会话状态而存在的两个存储技术



### Cookies

+   cookies是保存在客户端浏览器上的存储空间

#### 特点

+   cookies 在浏览器上是以键-值对的形式存储的,键和值都是以ASCII字符串的形式存储(不能是中文字符串)
+   存储的数据带有生命周期
+   cookies的数据是按域存储隔离的,不同的域之间无法访问
+   cookies的内部的数据会在每次访问此网站时,都会携带到服务器端,如果cookies过大会降低响应速度

#### cookies的使用 - 存储

```python
HttpResponse.set_cookie(key,value='',max_age=None,expires=None)
```

+   key: cookie的名字
+   value: cookie的值
+   max_age: cookie存活时间,秒为单位
+   expires: 具体过期时间
+   当不指定max_age和expires时,关闭浏览器时此数据失效

存储示例:

```python
# 为浏览器添加键为my_var1,值为123,过期时间为1个小时的cookie
def cookie_view(request):
    respond = HttpResponse('已添加')
	respond.set_cookie('my_var1',123,3600)
	return respond

# 修改cookie
respond.set_cookie('my_var1',456,3600) # 覆盖
```



#### cookies的使用 - 删除&获取

删除cookie

```python
HttpResponse.delete_cookie(key)
# 删除指定key的cookie,如果不存在则什么也不发生
```

获取cookie

```python
cookies = request.COOKIES # 所有cookie组成的字典
```



### Session

+   session是在服务器上开辟一段空间用于保存浏览器和服务器交互时的重要数据
+   实现方式:
    +   使用session需要在浏览器客户端启动cookie,且在cookie中存储sessionid
    +   每个客户端都可以在服务器端有一个独立的session
    +   注意:不同的请求者之间不会共享这个数据,与请求者一一对应

#### session初始配置

+   `settings.py`中配置session

    1.   向`INSTALLED_APPS`列表中添加:

         ```python
         INSTALLED_APPS = [
             ...
             # 启用session应用
             'django.contrib.sessions',
             ...
         ]
         ```

    2.   向`MIDDLEWARE`列表中添加

         ```python
         MIDDLEWARE = [
             ...
             # 启用session中间件
             'django.contrib.sessions.middleware.SessionMiddleware',
             ...
         ]
         ```

#### session的使用

+   session是一个类似于字典的`SessionStore`类型的对象,可以用类似于字典的方式进行操作

+   session能够存储字符串,整型,字典,列表等

+   用法:

    1.   保存session的值到服务器

         ```python
         request.session['key']=value
         ```

    2.   获取session的值

         ```python
         value = request.session['key']
         value = request.session.get('key',默认值)
         ```

    3.   删除session

         ```python
         del request.session['key']
         ```

#### settings.py中相关配置项

1.   `SESSION_COOKIE_AGE`

     作用: 指定sessionid在cookies 中保存的时长(默认两周)

2.   `SESSION_EXPIRE_AT_BROWSER_CLOSE`

     作用: 值为`True`时,只要浏览器关闭,session就失效(默认为False)

注意: django的session数据存储在数据库中,所以使用session前需确保已经执行过`migrate`

#### 一些问题

1.   django_session表是单表设计,且该表数据量持续增加(浏览器故意删掉sessionid或过期数据未删除)
2.   可以每晚执行`python manage.py clearsessions` [该命令可删除已过期的session数据]



### Cookies和Session

| 种类    | 存储   | 安全性     |
| ------- | ------ | ---------- |
| Cookies | 浏览器 | 相对不安全 |
| Session | 服务器 | 相对安全   |

