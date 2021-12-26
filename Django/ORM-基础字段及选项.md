### 创建模型类流程

1.   创建应用

2.   在应用下的`models.py`中编写模型类

3.   迁移同步 `makemigrations`&`migrate`

+   任何关于表结构的修改,务必在对应模型类上修改

+   例:为`bookstore_book`表添加一个名为`info`的字段

    解决方案:

    +   模型类中添加对应类属性
    +   执行数据库迁移



### 字段类型

+   `BooleanField()`
    +   数据库类型: `tinyint(1)`
    +   编程语言中: 使用`True`或`False`来表示值
    +   在数据库中: 使用`1`或`0`来表示具体的值
+   `CharField()`
    +   数据库类型: `varchar`
    +   注意: 必须要指定`max_length`参数值
+   `DateField()`
    +   数据库类型: `date`
    +   作用: 表示日期
    +   参数
        1.   `auto_now`: 每次保存对象,自动设置该字段为当前时间(取值:`True`&`False`)
        2.   `auto_now_add`: 当对象第一次被创建时自动设置当前时间(取值:`True`&`False`)
        3.   `default`: 默认值(取值:字符串格式时间如:'2021-12-25')
    +   以上三个参数只能选其中一个

+   `DATeTimeField()`
    +   数据库类型: `datetime(6)`
    +   作用: 表示日期和时间
    +   参数同`DateField()`

+   `FloatField()`
    +   数据库类型: `double`
    +   编程语言和数据库中都是用小数表示值
+   `DecimalField()`
    +   数据库类型: `decimal(x,y)`
    +   在编程语言和数据库中都使用小数表示值
    +   参数
        1.   `max_digits`: 位数总数,包括小数点后的位数,该值必须大于等于`decimal_places`
        2.   `decimal_places`: 小数点后的位数
+   `EmailField()`
    +   数据库类型: `varchar`
    +   编程语言和数据库都使用字符串表示值
+   `IntegerField()`
    +   数据库类型: `int`
    +   编程语言和数据库都使用字符串表示值

+   `ImageField()`
    +   数据库类型: `varchar(100)`
    +   作用: 在数据库中保存图片的路径
    +   编程语言和数据库都使用字符串表示值
+   `TextField()`
    +   数据库类型: `longtext`
    +   作用: 表示不定长的字符数据



### 创建模型类(练习)

+   在`bookstore`应用中添加一个模型类
+   Author - 作者
    +   name - CharField 姓名 最大长度11
    +   age - IntegerField 年龄
    +   email - EmailField 邮箱

```python
class Author(models.Model):
    name = models.CharField('姓名',max_length=11)
    age = models.IntegerField('年龄')
    email = models.EmailField('邮箱')
```