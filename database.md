## 简介

提供MySQL数据库操作CURD, 以数据库进程池方式调用连接操作，采用PDO方式实例化连接。

操作模式：

- 模型类Model(提供模型关联操作：一对一|一对多|多对多，以及类型转行、获取器)
- DB类

## 规范

- 数据库操作代码块必须加try catch捕获异常
- 多个模型操作数据库务必用初始化连接共用同一个连接 保证数据事务操作完整性

### 1、模型

模型定义位置 /app/Model

实例化模型 构造函数不能有参数
```
  $Model = new UserModel()
```
添加数据
```
  单条
  $data = ['title' => 'zhangxiaolong', 'sex' => 2];
  批量
  $data = [
    ['title' => 'zhangxiaolong', 'sex' => 2],
    ['title' => 'ma', 'sex' => 1],
    ['title' => 'xu', 'sex' => 2],
  ]
  $insert_id = $Model->insert($data)
  成功返回插入ID，否则抛出异常
```
修改数据
```
  $where['id'] = 1;
  $data['money'] = 666666;
  $data['grade'] = 2;
  $affect_row = $Model->where($where)->update($data);
  成功返回大于0影响行数，失败返回0|或抛出异常
```
删除数据
```
  $where['id'] =  2;
  $affect_row = $Model->where($where)->delete();
  成功返回大于0影响行数，失败返回0|或抛出异常
```
列表查询数据 ,查询条件构造器类似laravel
```
  $Model->whereIn('id', [1, 2, 3])
    ->select('id', 'title')
    ->append(['state_txt', 'grade_txt'])
    ->with(['member', 'order', 'assetlog' => function($Query){ $Query->select('id', 'title'); }])
    ->get();
  成功返回结果数组列表（没数据为空数组），失败抛出异常
```
单条数据查询
```
  $Model->where('id', 1)->append(['state_txt'])->first();
  成功返回结果数组列表（没数据为空数组），失败抛出异常
```
聚合查询：count/sum/min/max/avg
```
  $Model->where('id', '>=', 1)->count();
  $Model->where('id', '>=', 1)->groupBy('country')->sum('amount');
  $Model->where('id', '>=', 1)->max('age');
  $Model->where('id', '>=', 1)->min('score');
  $Model->where('id', '>=', 1)->avg('commission');
  成功返回数字，失败抛出异常
```
字段的递增递减
```
  //单个字段
  $Model->where('id', 1)->increment('votes', 1)
  $Model->where('id', 1)->decrement('votes', 2)
  //批量
  $Model->where('id', 1)->increment(['age' => 1, 'amount' => 100])
  $Model->where('id', 1)->decrement(['score' => 2, 'money' => 6])
  成功返回大于0影响行数，失败返回0|或抛出异常
```

原生查询操作
```
  $Model->query($sql, $where)
  成功返回结果数组，失败抛出异常
```
执行非查询sql语句，drop/create/alter/insert等ddl

```
  $Model->exec($sql, $where)
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
