# MySQL设计规范

# 前言

## 编写目的

本文档结合行业内对于MySQL设计规范的共识，以及福建英迈软件公司常年使用MySQL数据库的经验基础上编写。通过编写此文档，帮助团队成员在开发中降低沟通成本，统一命名方式，形成开发默契。

## 版本说明

2020-08-19   黄志炜  发布第一版

2021-01-04   黄平三  修订第二版

# 规范约定

## 通用规范

- 默认使用utf8mb4字符集，数据库排序规则使用utf8mb4_general_ci，（由于数据库定义使用了默认，数据表可以不再定义，但为保险起见，建议都写上）。

- 表引擎取决于实际应用场景；如无说明，建表时一律采用innodb引擎。
- 采用26个英文字母(区分大小写)和0-9的自然数(经常不需要)加上下划线'_'组成，命名简洁明确，多个单词用下划线'_'分隔，命名要以字母开头。
- 尽量使用更自然的集合术语，例如：使用employee，而不是employees
- 尽量避免使用缩写词。使用时一定确定这个缩写简明易懂。
- 所有表、字段均应用 comment 列属性来描述此表、字段所代表的真正含义，如枚举值则建议将该字段中使用的内容都定义出来。

## 数据库与表命名

- 禁止使用数据库关键字，如：name，time ，datetime，password等
- 表名称不应该取得太长（一般不超过三个英文单词）
- 表的名称一般使用名词或者动宾短语
- 表必须填写描述信息（使用SQL语句建表时）
- 每个表需要指定表主键

## 表字段

### 字段命名规范

- 全部小写命名，禁止出现大写
- 总是使用单数形式
- 字段名称一般采用名词或动宾短语
- 采用字段的名称必须是易于理解，一般不超过三个英文单词
- 在命名表的列时，不要重复表的名称【例如，在名employe的表中避免使用名为employee_lastname的字段】

- 不要在列的名称中包含数据类型
- 字段命名使用完整名称，不建议缩写
- 主键可以使用id，外键可以使用 “表名”+id 


### 命名统一后缀参考

下列后缀有统一的意义，能保证SQL代码更容易被理解。在合适的时候使用正确的后缀。

- `_id` 独一无二的标识符，如主键。
- `_status` 标识值或任何表示状态的值，比如`publication_status`。
- `_total` 总和或某些值的和。
- `_num` 表示该域包含数值。
- `_name` 表示名字。
- `_seq` 包含一系列数值。
- `_date` 表示该列包含日期。
- `_tally` 计数值。
- `_size` 大小，如文件大小或服装大小。
- `_addr` 地址，有形的或无形的，如`ip_addr`
- `tmp_`临时表
- `_bak` 备份表

### 表字段命名示例

- 名词：user_id、user_name、sex
- 动宾短语：is_friend、is_good
- 建议使用完整命名：使用persion_id，而不是pid；使用user_id，而不是uid

### 表字段类型规范

- 所有字段在设计时，除以下数据类型timestamp、image、datetime、smalldatetime、uniqueidentifier、binary、sql_variant、binary 、varbinary外，必须有默认值，字符型的默认值为一个空字符值串’’，数值型的默认值为数值0，逻辑型的默认值为数值0
- 系统中所有逻辑型中数值0表示为“假”，数值1表示为“真”，datetime、smalldatetime类型的字段没有默认值，必须为NULL
- 用尽量少的存储空间来存储一个字段的数据。

```text
使用int就不要使用varchar、char，

用varchar(16)就不要使varchar(256)

IP地址使用int类型

固定长度的类型最好使用char，例如：邮编(postcode)

能使用tinyint就不要使用smallint，int

最好给每个字段一个默认值，最好不能为null

使用短数据类型，比如取值范围为0-80时，使用TINYINT UNSIGNED

存储精确浮点数必须使用DECIMAL替代FLOAT和DOUBLE

```

- 用合适的字段类型节约空间

```text
字符转化为数字(能转化的最好转化，同样节约空间、提高查询性能)

避免使用NULL字段(NULL字段很难查询优化、NULL字段的索引需要额外空间、NULL字段的复合索引无效)

少用text和blob类型(尽量使用varchar代替text字段)
```

- DATETIME：所有需要精确到时间(时分秒)的字段均使用DATETIME,不要使用TIMESTAMP类型

### 字段索引建立依据

- 如果字段事实上是与其它表的关键字相关联而未设计为外键引用，需建索引
- 如果字段与其它表的字段相关联，需建索引
- 如果字段需做模糊查询之外的条件查询，需建索引
- 除了主关键字允许建立簇索引外，其它字段所建索引必须为非簇索引

## SQL 编码规范

- 所有关键字必须大写，如：INSERT、UPDATE、DELETE、SELECT及其子句，IF……ELSE、CASE、DECLARE等

- 所有函数及其参数中除用户变量以外的部分必须大写
- 在定义变量时用到的数据类型必须小写

**参考资料：**

- [SQL样式指南 · SQL Style Guide](https://www.sqlstyle.guide/zh/)
- [[MySQL命名、设计及使用规范《MySQL命名、设计及使用规范》](https://www.cnblogs.com/weiguoaa/p/8952943.html)](https://www.cnblogs.com/weiguoaa/p/8952943.html)
- [Mysql命名规范](https://blog.csdn.net/fujian9544/article/details/86649096)
- [MySQL开发规范](https://juejin.im/post/5d8c8410e51d45784c3d9856)

- [MySQL coding-style](https://dev.mysql.com/doc/internals/en/coding-style.html)