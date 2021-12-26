## ORM操作

基本操作包括增删改查,即CRUD操作

CRUD是指在计算处理时的增加(Create),读取查询(Read),更新(Update)和删除(Delete)

ORM CRUD核心 --> 模型类.管理器对象

### 管理器对象

每个继承自`models.Model`的模型类,都会有一个`objects`对象被继承下来.这个对象叫管理器对象

```python
class MyModel(models.Model):
    ...
    
Mymodel.objects.create(...) # objects是管理器对象
```



### 创建数据

Django ORM使用一种直观的方式把数据库表中的数据表示成python对象

创建数据中每一条记录就是创建一个数据对象

#### 方案一

```python
Mymodel.objects.create(属性1=值1,属性2=值2,...)
```

+   成功: 返回创建好的实体对象
+   失败: 抛出异常

#### 方案二

创建MyModel实例对象,并调用`save()`进行保存

```python
obj = MyModel(属性1=值1,...)
obj.属性1 = 值1
obj.save()
```



### Django shell

在django提供了一个交互式的操作项目叫django shell, 它能够在交互模式用项目工程的代码执行相应的操作

利用django shell 可以代替编写view的代码来进行直接操作

**注意:** 项目代码发生变化时,重新进入django shell

启动方式: `python manage.py shell`





### 查询操作

+   数据库查询需要使用管理器对象进行

+   通过`MyModel.objects`管理器方法调用查询方法

    | 方法      | 说明                          |
    | --------- | ----------------------------- |
    | all()     | 查询全部记录,返回QuerySet对象 |
    | get()     | 查询符合条件的单一记录        |
    | filter()  | 查询符合条件的多条记录        |
    | exclude() | 查询符合条件之外的全部记录    |
    | ...       | ...                           |



#### all()方法

+   用法: `MyModel.objects.all()`

+   作用: 查询MyModel实体中所有的数据,等同于`select * from tabel`

+   返回值: QuerySet容器对象,内部存放MyModel实例

    ```python
    from bookstore.models import Book
    books = Book.objects.all()
    for book in books:
        print('书名',book.title,'出版社',book.pub)
    ```



#### values()方法

+   用法: `MyModel.objects.values(列1,列2,...)`

+   作用: 查询部分列的数据,等同于`select 列1,列2 from tabel`

+   返回值: QuerySet容器对象,内部存放字典,每个字典代表1条数据,格式为:{'列1':'值1','列2':'值2'}

    

#### values_list()方法

+   用法: `MyModel.objects.values_list(列1,列2,...)`
+   作用: 返回元组形式的查询结果
+   返回值: QuerySet容器对象,内部存放元组



#### order_by()方法

+   用法: `MyModel.objects.order_by('-列'或'列')`
+   作用: 与`all()`方法不同,它会用sql语句的`order by`子句对查询结果根据某个字段选择性的进行排序
+   说明: 默认是按照升序排序,降序排序则需在列前增加`-`表示



#### filter()方法

+   用法: `MyModel.objects.filter(属性1=值1,属性2=值2)`
+   作用: 返回包含此条件的全部的数据集
+   返回值: QuerySet容器对象,内部存放MyModel实例
+   说明: 多个属性在一起为"与"关系



#### exclude()方法

+   用法:  `MyModel.objects.exclude(属性1=值1,属性2=值2)`
+   作用: 返回不包含此条件的全部的数据集



#### get()方法

+   用法:  `MyModel.objects.get(条件)`
+   作用: 返回满足条件的唯一一条数据
+   说明: 该方法只能返回一条数据,查询结果若多于一条数据则抛出`Model.MultipleObjectsReturned`异常,若没有数据则抛出`Model.DoesNotExist`异常



### 查询谓词

定义: 做更灵活的条件查询时需使用查询谓词

说明: 每一个查询谓词是一个独立的查询功能

+   `__exact`: 等值匹配

    ```python
    Book.objects.filter(id__exact=1)
    # 等同于select * from book where id=1
    ```

+   `__contains`: 包含指定值

    ```python
    Book.objects.filter(title__exact='y')
    # select * from book where title like '%y%'
    ```

+   `__startwith`: 以xxx开始

+   `__endwith`: 以xxx结束

+   `__gt`: 大于指定值

    ```python
    Book.objects.filter(price__gt=50)
    # select * from book where price>50
    ```

+   `__gte`: 大于等于

+   `__lt`: 小于

+   `__lte`: 小于等于

+   `__in`: 查询数据是否在指定范围内

    ```python
    Book.objects.filter(pub__in=['清华大学出版社','机械工业出版社'])
    # select * from book where pub in ('清华大学出版社','机械工业出版社')
    ```

+   `__range`: 查找数据是否在指定的区间范围内

    ```python
    Book.objects.filter(price__in=(50,100))
    # select * from book where price between 50 and 100
    ```

    

### 更新数据

#### 更新单个数据

+   修改单个实体的某些字段值的步骤:
    1.   查 - 通过`get()`得到要修改的实体对象
    2.   改 - 通过`对象.属性`的方式修改数据
    3.   保存 - `对象.save()`保存数据

#### 批量更新数据

直接调用`QuerySet`的`update(属性=值)`实现批量修改

```python
# 将所有书的定价改为100
books = Book.objects.all()
books.update(price=100)
```



### 删除数据

#### 单个数据删除

+   步骤

    1.   查找查询结果对应的一个数据对象
    2.   调用这个数据对象的delete()方法实现删除

    ```python
    try:
        book = Book.objects.get(id=1)
        book.delete()
    except:
        print('删除失败')
    ```

#### 批量删除

+   步骤

    1.   查找查询结果集中满足条件的QuerySet查询集合对象
    2.   调用查询集合对象的`delete()`方法实现删除

    ```python
    books = Book.objects.filter(price__gt=50)
    books.delete()
    ```

    

#### 伪删除

+   通常不会在业务里把数据真正删掉,取而代之的是做伪删除,即在表中添加一个布尔型字段(`is_active`),默认是`True`;执行删除时,将欲删除数据的`is_active`字段设置为`False`
+   注意: 用伪删除时,确保显示数据的地方,均加了`is_active=True`的过滤查询



### F对象

+   一个F对象代表数据库中某条记录的字段的信息

+   作用:

    +   通常是对数据库的字段值在不获取的情况下进行操作
    +   用于类属性(字段)之间的比较

+   语法:

    ```python
    from django.db.models import F
    F('列名')
    ```

+   示例

    ```python
    # 所有书的定价增长10元
    Book.objects.all().update(price=F('price')+10)
    ```

+   区别:

    ```python
    # id:1 title:Python price:50
    
    Book.objects.get(id=1).price = Book.objects.get(id=1).price + 10
    # update book set price = 60 where id=1
    
    Book.objects.get(id=1).price = F('price') + 10
    # update book set price = price + 10 where id=1
    ```

    

### Q对象

+   当在获取查询结果集 使用复杂的逻辑或|,逻辑非~等操作时可以借助于Q对象操作

```python
# 如想找出定价低于20元 或 清华大学出版社的全部书
from django.db.models import Q

Book.objects.filter(Q(price__lt=20)|Q(pub='清华大学出版社'))
```



### 聚合查询

聚合查询是对一个数据表中的一个字段的数据进行部分或全部进行统计查询,如查平均价格,总个数

聚合查询分为

+   整表查询
+   分组查询

#### 整表聚合

不带分组的聚合查询是将全部数据进行集中统计查询

聚合函数[需要导入]

+   导入方法: ` from django.db.models import *`
+   聚合函数: `Sum,Avg,Count,Max,Min`

语法: `MyModel.objects.aggregate(结果变量名=聚合函数('列'))`

返回结果: 结果变量名和值组成的字典

#### 分组聚合

分组聚合是指通过计算查询结果中每一个对象所关联的对象集合,从而得出总计值(也可以是平均值或总和),即为查询集的每一项生成聚合

语法: `QuerySet.annotate(结果变量名=聚合函数('列')`

返回值: QuerySet

步骤:

1.   用`MyModel.objects.values()`查找要分组聚合的列
2.   通过返回结果的`QuerySet.annotate()`方法分组聚合得到分组结果

```python
from django.db.models import Count
from bookstore.models import Book

qs = Book.objects.values('pub')
qs.annotate(mycount = Count('pub'))

# 输出
<QuerySet [{'pub': '清华大学出版社', 'mycount': 3}, {'pub': '机械工业出版社', 'mycount': 2}]>
```



### 原生数据库操作

django也可以支持直接用sql语句方式通信数据库

查询: 使用`MyModel.objects.raw()`进行数据库查询操作

语法: `MyModel.objects.raw(sql语句)`

返回值: RawQuerySet集合对象[只支持基础操作,比如循环]



完全跨过模型类操作数据库 - 查询/更新/删除

1.   导入cursor所在的包

     `from django.db import connection`

2.   用创建cursor类的构造函数创建cursor对象,再使用cursor对象,为保证在出现异常时能释放cursor资源,通常使用with语句进行创建操作

     ```python
     from django.db import connection
     with connetion.cursor() as cur:
         cur.excute('执行sql语句','拼接参数')
     ```

     



