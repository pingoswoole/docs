## 简介

提供MySQL数据库操作CURD, 以数据库进程池方式调用连接操作，采用PDO方式实例化连接。

操作模式：

- 模型类Model(提供模型关联操作：一对一|一对多|多对多，以及类型转行、获取器)
- DB类
### 1、模型

模型定义位置 /app/Model

```
pip install -r requirements.txt
```

### 2、DB类

在安装完所需的第三方库并配置好数据库信息之后，我们需要对数据库进行初始化。

在项目路径下打开命令行界面，运行如下命令生成数据库迁移：

```
python manage.py makemigrations 
```

运行如下命令执行数据库迁移:

```
python manage.py migrate
```
执行完毕之后，数据库就初始化完成了。

### 3、创建管理员账户
在初始化完数据库之后，需要创建一个管理员账户来管理整个MrDoc，在项目路径下打开命令行终端，运行如下命令：
```
python manage.py createsuperuser
```
按照提示输入用户名、电子邮箱地址和密码即可。
