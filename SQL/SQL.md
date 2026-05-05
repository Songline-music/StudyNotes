# SQL/MySQL学习

## 数据库类型

`关系型数据库` 由多张互相连接的二维表组成的数据库

## 数据类型

- 整型
- `tinyInt`   1 byte `小型整数`
- `smallInt`  2 byte `大型整数`
- `mediumInt` 3 byte `大型整数`
- `int`       4 byte `大型整数`
- `bigInt`    8 byte `极大型整数`

- 浮点型
- `float`   4 byte `单精度浮点值`
- `double`  8 byte `双精度浮点值`
- `decimal` 依赖于`精度`以及`标度`的 `小数值`
> 浮点数精度可以通过 double(精度，标度) 来限制大小
> 数据类型_unsigned 转换为无符号类型

- 字符段型
- `char` `定长字符串`
- `varChar` `变长字符串`
- `tinyBlob` 不超过255个字符的二进制数据
- `tinyText` `短文本字符串`
- `mediumBlob` 二进制形式的中等长度文本数据
- `mediumText` `中等长度文本数据`
- `blob` 二进制形式的长文本数据
- `text` `长文本数据`
- `longBlob` 二进制形式的极大文本数据
- `longText` `极大文本数据`

## SQL通用语法

- MySQL数据库的SQL语句不区分大小写
- 注释方法
```
-- 这是一条注释内容
#  这是一条注释内容
/* 这是一条注释内容 */
```

## SQL分类
- `DDL` 数据定义语言，用来定义数据库对象
- `DML` 数据操作语言，用来对数据库中表的内容进行增删改
- `DQL` 数据查询语言，用来查询数据库中表的记录
- `DCL` 数据控制语言，用来创建数据库用户，控制数据库的访问权限

### DDL

#### DDL-数据库操作

- 查询
```
show databases;  查询所有数据库
show database(); 查询当前数据库
```

- 创建
```
create datebase [if not exists] 数据库名 [default charset 字符集] [collate 排序规则];
```
> [ ] 内部内容可以不写，输入默认值
> [ ] 在实际书写时删去

- 删除
```
drop database [if exists] 数据库名;
```

- 使用
```
use 数据库名;
```
> 将当前数据库切换为指定数据库

#### DDL-表操作

- 查询
```
show tables; 查询当前数据库的所有表
desc 表名; 查询当前表的结构
show create table 表名; 查询指定表的建表语句
```

- 创建
```
create table 表名(
    字段1 字段1类型[comment 字段1注释],
    字段2 字段2类型[comment 字段2注释],
    ....
    字段3 字段3类型[comment 字段3注释]
)[comment 表注释];
```
> 最后一行语句没有,
> 每行写完后回车进入第二行书写