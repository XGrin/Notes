### GET处理

+   GET请求动作,一般用于向服务器获取数据

+   能够产生GET请求的场景:

    +   浏览器地址栏中输入url,并回车
    +   <a href=''地址?参数=值....'
    +   form表单中的method为get

+   GET请求方式中,如果有数据需要传递给服务器,通常会用查询字符串的方式传递

    URL格式:xxx?参数名1=值1&参数名2=值2...

    如:http://127.0.0.1:8000/page1?a=100&b=200

    ```python
    def xxx_view(request):
        a = request.GET['a'] # 获取参数a
        a = request.GET.get('a','no a') # 获取参数a,如果参数a不存在则返回'no a'
        a = request.GET.getlist('a') # 获取列表
    ```




### POST处理

+   POST请求动作,一般用于向服务器提交大量隐私数据

+   客户端通过表单等POST请求将数据传递给服务器端，如：

    ```html
    <form method='post' action='/login/'>
        姓名:<input type="text" name="username">
        <input type='submit' value='登录'>
    </form>
    ```

+   服务端接收参数

    通过`request.method`来判断是否为post请求,如:

    ```python
    if request.method == 'POST':
        处理POST请求的数据并响应
    else:
        处理非POST请求的响应
    ```

    使用POST方式接收客户端数据

    ```python
    request.POST['参数名']
    request.POST.get('参数名','默认值')
    request.POST.getlist('参数名')
    ```

    ***注意:***取消csfr验证,否则Django将会拒绝客户端发来的POST请求,报403

    取消csfr验证: 禁止掉setting.py中MIDDLEWARE中的CsrfViewMiddleware的中间件

    ```python
    MIDDLEWARE = [
        ...
        # 'django.middleware.csrf.CsrfViewMiddleware',
        ...
    ]
    ```

    