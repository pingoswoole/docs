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
  
  成功返回影响数值，失败抛出异常
```

事务操作：多条sql增删改查请使用事务

```
  //初始化连接
  $Model->initConnect()
  //获取一个PDO连接
  $Pdo = $Model->getPdo();
  //设定其它操作模型共用一个pdo，一定要在使用操作数据前设定模型的pdo
  $Model2 = new Model2();
  $Model2->setPdo($Pdo);
  $Model3 = new Model3();
  $Model3->setPdo($Pdo);
  
  $Model2->select / insert / update / count / 
  $Model3->select / insert / update / count / increment
  //开启事务
  $Model->beginTransaction()
  //提交事务
  $Model->commit()
  //回滚事务
  $Model->rollback()
  //归还pdo连接
  $Model->return()
  
```

### 2、DB类

增删查改类似模型，初始化对象选择数据表即可操作CURD

 
实例化初始采用快捷方法db
```
  $data = ['title' => 'database', 'name' => 'pingo']
  db()->table('user')->insert($data)
```



事务操作

```
 //初始化对象
 $db = db()->initConnect();
 //开启事务
  $db->beginTransaction()
  $db->table('tb1')->insert($data)
  $db->table('tb2')->update($data)
  $db->table('tb3')->delete()
  $db->table('tb4')->decrement($data)
  
  //提交事务
  $db->commit()
  //回滚事务
  $db->rollback()
  //归还pdo连接
  $db->return()
```
 

### 结构化查询器
 
  采用laravel 构造器形式
  支持：where / whereIn whereNotIn  / orderBy / groupBy / select / limit / page / having /
 
