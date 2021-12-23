### URL

+   **定义:**统一资源定位符(Uniform Resource Locator)
+   **作用:**用来表示互联网上某个资源的地址
+   URL的一般**语法格式**为：protocol :// hostname[:port] / path /[?query] [#fragment]
    +   **protocol**(协议)  ==http://==tts.tmooc.cn
        +   http通过HTTP访问该资源,格式http://
        +   https通过安全的HTTPS访问该资源,格式https://
        +   file资源是本地计算机上的文件,格式file://
    +   **hostname**(主机名) http:// ==tts.tmooc.cn==
        +   是指存放资源的服务器的域名系统(DNS)主机名、域名或ip地址
    +   **port**(端口) http://tts.tmooc.cn: ==80==
        +   整数,可选,省略时使用方案的默认端口
        +   各种传输协议都有默认的端口,如http的默认端口为80
    +   **path**(路由地址) http://tts.tmooc.cn/ ==video/showVideo==
        +   由零或多个“/”符号隔开的字符串，一般用来表示主机上的一个目录或文件地址。
    +   **query**(查询) /video/showVideo==?menuld=657421&version=AID999==
        +   可选，用于给动态网页（如使用CGI、ISAPI、PHP/JSP/ASP/ASP.NET等技术制作的网页）传递参数，可有多个参数，用“&”符号隔开，每个参数的名和值用“=”符号隔开。
    +   **fragment**(信息片断) version=AID999==#subject==
        +   字符串，用于指定网络资源中的片段。例如一个网页中有多个名词解释，可使用fragment直接定位到某一名词解释。



### django如何处理URL的请求

+   浏览器地址栏 --> http://127.0.0.1:8000/page/2003/
    1.   Django 从配置文件中根据ROOT_URLCONF 找到主路由文件;默认情况下，该文件在项目同名目录下的urls;例如mysite1/mysite1/urls.py
    2.   Django加载主路由文件中的urlpatterns变量[包含很多路由的数组]
    3.   依次匹配 urlpatterns 中的path，匹配到第一个合适的中断后续匹配
    4.   匹配成功–调用对应的视图函数处理请求，返回响应
    5.   匹配失败–返回404响应



### 视图函数

+   视图函数是用于接收一个浏览器请求(HttpRequest对象)并通过HttpResponse对象返回响应的函数.此函数可以接收浏览器请求并根据业务逻辑返回相应的响应内容给浏览器

+   语法

    ```python
    def xxx_view(request[,其他参数...]):
        return HttpResponse对象
    ```

+   样例

    ```python
    # file:<项目同名文件夹下>/views.py(自己创建)
    from django.http import HttpResponse
    
    def page1_view(request):
        html="<h1>这是第一个页面<h1>"
        return HttpResponse(html)
    ```

+   绑定视图函数

    ```python
    from django.contrib import admin
    from django.urls import path
    from . import view # 从当前目录import view
    
    urlpatterns = [
        path('admin/', admin.site.urls),
        path('page/2003/',view.page1_view)
        # path('路由地址',函数)
    ]
    ```

    
