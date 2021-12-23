### pip

[Python pip 安装与使用 ](https://www.runoob.com/w3cnote/python-pip-install-usage.html)

### venv

+   安装venv:

```bash
python3 install virtualenv
```

+   venv的使用：

**在当前目录创建虚拟环境**

```bash
python3 -m venv .
```

**在当前目录创建独立的python环境**

```bash
virtualenv --no-site-packages venv
```

**激活虚拟环境**

```bash
source venv/bin/activate
```

**停用虚拟环境**

```bash
deactivate
```

**删除虚拟环境**

```bash
rm -rf venv
```



### 安装django

1.   使用`venv`创建一个虚拟环境,并激活

2.   通过 `pip` 安装正式发布版本

     ```bash
     python -m pip install Django
     ```

     