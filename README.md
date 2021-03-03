# mysql（DBMS)

## 数据库概念

- DB

> 数据库(database): 存储数据的仓库，保存了一系列有组织的数据

- DBMS

> 数据库管理系统() 数据库是通过DBMS创建和操作的容器

- sQL

> 结构化查询语言(): 用来于数据库通信的语言。

- 存储数据的特点

> 将数据放到表中，表再放到库中
> 一个数剧库有很多各表，每个表的名字举有唯一性
> 表具有一些特性，这些特性定义了数据再表中如何存储。
> 表中的数据按行存储的
> 表由列构成，成为字段

## 数据库服务启动和停止

- 启动：net start 服务名

- 关闭： net stop 服务名

## MySQL 服务端的登录和退出

- 登录： mysql -h 主机号 -P 端口号 -u 用户名 -p密码

- 退出： exit / ctrl+c

## MySQL 的常见命令

- show databases: 查看所有的数据库;

- use 库名: 进入某个数据库;

- show tables ： 查看当前库中的表

- show tables FROM mysql: 查看指定库中的表

- SELECT database() : 查看当前所在的库的名字

- create table 名字(表的字段);

- desc 表名： 查看表的结构

- SELECT * FROM stuinfo ：查看表中的

- insert into stuinfo (字段名1,字段名2) values(值1, 值2)： 表中插值

- update stuinfo set name="小明" WHERE id = 1; 更新表中的某个数据

- delete FROM 表名 WHERE id = 1; 删除标中id为一的数据;

- 查看版本： SELECT version();

- 查看版本： musql --version

## MySQL语法规范

- 不区分大小写, 关键子大写，表名 列名小写。
- 每条命令最好用分号结尾
- 每条命令根据需要，可以进行缩进和换行(关键字独占一行)
- 注释

> 单行：#注释， -- 注释。 多行：/*注释文字*/

> from - join - on - on - group by - having - select - order by - limit

## 查询语言DQL

- 查询前进入数据库： use 库名;

### 基础查询

> 当字段和关键子冲突时可以使用 ``作为区分

```SELECT 查询列表 FROM 表名;```

> 查询列表可以是：表中的字段 常量 表达式 函数
> 查询的结果是一个虚拟的表格

- 查询表中的单个字段：

> 查询employees表中的last_name字段

```SELECT last_name FROM employees```

- 查询表中的多个字段

> 查询employees表中的last_name salary email多个字段

```SELECT last_name, salary, email FROM employees```

- 查询表中的所有字段

> 使用*查询出来的字段顺序和数据库中的完全一致

```SELECT * FROM employees```

- 查询常量值

```SELECT 100;``` ```SELECT abc```

- 查询表达式

```SELECT 2 * 3```

- 查询函数：相当于调用函数

```SELECT version()```

- 为字段起别名(为了避免命名冲突)：

>别名中出现特殊字符使用引号,可以不使用as 直接使用空格

```SELECT 2 + 3 as sum```

```SELECT last_name as 姓, first_name 名 FROM employees```

- 去重**distinct**

> 查询员工表中的部门编号，并且去重
```SELECT distinct departement_id FROM employees```

- +号的作用: 只有运算符的功能

> [如果要拼接则使用concat函数](#jump)

```js
SELECT 2 + 3 as sum 两个都为数值型，则作加法运算
SELECT "123" + 45; 一方为字符型，试图讲字符型转换为数值型 
                  转换成功，则继续加法运算
SELECT "hellow"+10; 失败，则讲字符型转换为0，继续加法运算
SELECT null+10;  一方为null 则结果肯定为nul
```

### 条件查询

```SELECT 查询列表 FROM 表名 WHERE 筛选条件;```

> 执行顺序： FROM WHERE SELECT
> 条件表达式筛选： ```> <  =  != >= <= <> <=>```
> 逻辑表达式筛选： ```&& || ! AND OR NOT```用于连接条件表达式
> 模糊查询： ```LIKE in  is null between AND```

- 条件表达式筛选

> 查询员工工资超过12000的员工信息

```SELECT * FROM WHERE employees salary > 12000```

> 查询部门编号不等于90号的员工姓名和部门id
> != 在mysql中可以用<>

```SELECT concat(last_name, frist_name) as 姓名, department_id FROM employees WHERE department_id != 90;```

- 逻辑表达式筛选

> 工资在10000 到20000之间的员工名，工资 和奖金

```SELECT concat(last_name, frist_name) as 姓名, department_id, salary FROM employees WHERE salary >= 10000 AND salary <= 20000;```

> 部门编号不在90到110之间或者工资高于15000的员工信息

```SELECT * FROM employees WHERE salary >= 15000 OR (department_id < 90 OR  department_id > 100)```

```SELECT * FROM employees WHERE salary >= 15000 OR NOT(department_id >= 90 AND  department_id <= 100)```

- 模糊查询

> 通配符可以使用\转义
> **LIKE**：可以判断字符和数值 一般和通配符搭配使用. %任意多个字符，包含0个字符， _任意单个字符,
> **ESCAPE**:可以自定义当前sql语句中的转义字符
> **BETWEEN AND** :某个区间范围(区间值包含边界值**大小不能颠倒**)
> **in**: 判断某个字段是否为in列表中的某一项
> **is null**: 判断为null的值， is not null 判断不是null的值(只可判断null)
> **<=>**: 可以判断 null和普通类型的值
> 员工名中包含A的员工信息

``` SELECT * FROM employees WHERE  last_name LIKE "%a%" ```

> 员工命中第三个字符为e第五个字符为b的员工信息

```SELECT * FROM employees WHERE last_name LIKE "__e_b%"```

> 员工命中第三个字符为_的员工信息

```SELECT * FROM employees WHERE last_name LIKE "__\_%```

```SELECT * FROM employees WHERE last_name LIKE "__$_% ESCAPE "$"```

> 员工命中工资在10000-15000的员工信息

``` SELECT * FROM employees BETWEEN 10000 AND 15000 ```

> 员工的工种编号为 'AD_VP' 或者 'AD_PRES', 或者 "AD_PRET"的员工信息

```SELECT * FROM employees WHERE job_id IN('AD_VP', "AD_PRES", "AD_PRET")```

> 没有奖金的员工名单和奖金率

```SELECT * FROM employees WHERE commission_pct IS NULL```

> 有奖金的员工名单和奖金率

```SELECT * FROM employees WHERE commission_pct <=> NULL```

### 排序查询

> 单个字段 多个字段、 表达式、 别名、 函数
> order by 一般放在查询语句的最后边
> limit 放在 order by 的后边

```SELECT * FROM 表名 【WHERE 条件】 ORDER BY 要排序的字段 【asc | desc】```

> 员工信息 工资从高到低排序j降序

```SELECT * FROM employess ORDER BY salary DESC```

> 升序 默认为ASC

```SELECT * FROM employess ORDER BY salary ASC```

> 部门编号大于90的员工，并且按照入职时间先后

```SELECT * FROM employess WHERE department_id >=90 ORDER BY hiredate ASC```

> 年薪的高低对员工进行排序(按表达式排序)

```SELECT *, salary* 12 *(1 + ifnull(commission_pct,0)) AS 年薪 FROM employess ORDER BY salary* 12 * (1 + ifnull(commission_pct,0)) ASC```

> 年薪的高低对员工进行排序(按别名排序)

```SELECT *, salary* 12 *(1 + ifnull(commission_pct,0)) AS 年薪 FROM employess ORDER BY 年薪 ASC```

> 根据姓名的字节长度显示员工信息(函数排序)

```SELECT *, LENGTH(last_name) AS 姓名字节长度 FROM employess ORDER BY 姓名字节长度ASC```

> 员工信息， 工资排序然后按照工资编号排序（按多个字段排序）

```SELECT * FROM employess ORDER BY salary, empolyee_id DESC```

### 常见函数库(函数可以嵌套使用)

#### 单行函数对传进去的值进行处理然后返回

- 字符函数

1. length() 判断值的字节长度

```查看字符集： show variables like "%%"```

3. upper、lower： 大小写转换

4. substr ： 截取字符

```SELECT SUBSTR("福建按龙卷风" 3:起始索引) as 输出 //龙卷风```

```SELECT SUBSTR("福建按龙卷风" 1, 4:截取的长度) as 输出 //福建按龙```

```SELECT SUBSTR("福建按龙卷风" 3) as 输出 //龙卷风```

```SELECT SUBSTR("福建按龙卷风" 3) as 输出 //龙卷风```

> 员工姓名首字母大写其他小写"-"拼接，然后返回

```SELECT CONCAT(UPPER(SUBSTR(last_name,1,1), "-", LOWER(SUBSTR(last_name,2))) As 姓名) From employees```

5. instr：返回字串在大串的起始索引，如果找不到返回0

6. trim： 去前后空格

> 可以自定义要去除的字符

```SELECT TRIM('a' from "aaaaa哈哈aaaa哈哈aaaaa") AS 输出```

7. lpad: 左填充(超出右边截断)

> lpad(原始值,期望的长度，如果长度不够使用的填充符号)

8. rpad 右填充

9. replace： 替换

> replace("原始值"，被替换的值，填充的值)

- 数学函数

1. round： 四舍五入

> round(数值) 四舍五入
> round(数值，保留的小数位)

2. ceil： 向上取整返回大于等于参数的最小整数

3. floor：向下取整返回小于等于参数的最大整数

4. turncate： 截断

> truncate(1.69,1) 保留以为小数，后边直接截断

5. mod 取余

- 日期函数 [https://blog.csdn.net/weixin_43705953/article/details/108691313]

1. str_to_date(str, format)（字符串转换为日期)

2. date_format(date,format), time_format(time,format)日期/时间转换为字符串函数

- 其他函数

- 流程控制函数 [https://blog.csdn.net/QQQZSJ/article/details/104108424]

#### 分组函数做统计使用(聚合函数)

> 文章：[https://blog.csdn.net/weixin_43064399/article/details/89406780]

- ifnull(字段名, 为null时显示的值): 判断是否为空

> commission_pct 为null 时显示为0；

```SELECT IFNULL(commission_pct,0) FROM employees```

- ISNULL("字段")判断该字段的值是否为null 1 true 0 false

- <span id="jump">concat(): 内置函数,作为拼接字符使用</span>

> 查询员工名和姓连成一个字段 并显示为 姓名

```SELECT CONCAT(last_name, first_name) as 姓名```

### 分组查询

- GROUP BY 子句语法：将表中的数据分成若干个组

```js
SELECT 分组函数(), 列名 From 表名 WHERE 分组前筛选条件 GROUP BY 筛选列名;
// 对分组查询的结果在 HAVINg 
SELECT 分组函数(), 列名 From 表名 WHERE 分组前筛选条件 GROUP BY 筛选列名 HAVINg 分组后筛选条件;
```

- 分组筛选条件分为两类

> 优先使用分组前筛选

1. 分组前筛选 WHERE

> 筛选的数据在原始表中

2. 分组后筛选 HAVING

> 筛选的数据在分组后的筛选集
> 分组函数做条件肯定放在HAVING中

- 按表达式或者函数分组

- 按多个字段分组

```js
SELECT 分组函数(), 列名1, 列名2 From 表名 WHERE 分组前筛选条件 GROUP BY 筛选列名1, 筛选列名2;
```

- 分组添加排序

```js
SELECT 分组函数(), 列名 From 表名 WHERE 分组前筛选条件 GROUP BY 筛选列名 HAVINg 分组后筛选条件 ORDER BY 排序列名;
```

### 连接查询(多表查询)

> 当查询字段来自多个表时，使用连接查询

> 笛卡尔乘积现象： 表1 M行 表2 N行， 结果 M*N 行

> 发生原因： 没有有效的链接条件

- 连接分类：功能分类

1. 内连接：取两表的交集
    1. 等值连接
    2. 非等值连接
    3. 自连接

2. 外连接 一个表中有，另一个表中没有的数剧
    1. 左外连接
    2. 右外连接
    3. 全外连接 (不支持)

> 外连接查询的是主表的全部数据，如果从表中有和它匹配的则显示匹配的值 如果没有则显示null
> 外连接查询的结果为内连接和主表中有从表中没有的记录
> 左外连接：left 左边的为主表
> 右外连接：right 右边的为主表
> 全外连接：内连接，表1中有表2中没有的记录，表2中有，表1中没有的数据

3. 交叉连接

> 不用连接条件就是交叉连接

- 年代分类：92 语法 99语法

- 等值连接 92语法

> 当两张表中的字段名字有冲突，可以使用表名.字段限制

> 为了简单可以给表起别名 FROM 表名 AS 别名

> 起了别名后再SELECT中只能使用别名

```SELECT 字段1, 字段2 FROM 表1, 表2 WHERE 表1.XXX = 表2.XXX```

> 添加筛选

```SELECT 字段1, 字段2 FROM 表1, 表2 WHERE 表1.XXX = 表2.XXX AND 筛选条件```

> 多表联查

```SELECT 字段1, 字段2 FROM 表1, 表2, 表3 WHERE 表1.XXX = 表2.XXX AND 表2.xxx === 表3.xxx AND 筛选条件```

- 非等值连接

> 和等值连接一样，只是不在是等于

- 自连接

> 一张表自己连接，

```SELECT 别名1.字段1, 别名2.字段2 FROM 表1 AS 别名1, 表1 AS 别名2 WHERE 别名1.XXX = 别名2.XXX AND 筛选条件```

#### 99语法

```js
    select 查询列表 from 表1 AS 别名 [连接类型] join 表2 AS 别名 on 连接条件 where 刷选条件 group by 分组条件 having 分组后的筛选条件 order by 排序条件
    内连接： inner
    外连接： 左外 left[outer] 右外 right[outer] 全外 full[outer]
    交叉连接： cross
```

### 子查询

> 出现在其它语句中的SELECT语句成为子查询或者内查询
> 外部的查询语句称为主查询或者外查询

```js
    /**
        分类:
        1. 按照子查询的位置
            select：标量子查询
            from ： 表子查询
          √ where和having 标量子查询、列子查询、行子查询
            exists ：表子查询
        2. 按照结果集的行列数不同
            标量子查询：结果集一行一列 (单行子查询)
            列子查询： 结果集一列多行  (多行子查询)
            行子查询：结果集一行多列或者多行多列
            表子查询： 随便一般为多行多列
    */

```

- where或者having

> 子查询放在小括号内
> 子查询一般放在条件的右侧
> 标量子查询一般搭配单行操作符使用：> <> <  >= <= =
> 列子查询一般搭配多行操作符使用： in any/same all

1. 标量子查询

> 谁的工资比abel的工资高

```SELECT last_name FROM employees WHERE salary >= (SELECT salary FROM WHERE last_name = "abel");```

2. 列子查询

3. 行子查询

> 当判断条件都相同时候可以用行子查询
> 查询员工编号最小并且工资最高的员工信息

```SELECT * FROM employees WHERE employee_id = (SELECT MIN(employee_id) FROM employees) AND salary = (SELECT MAX(salary) FROM emloyees);```

```SELECT * FROM employees WHERE (employee_id, salary) = (SELECT MIN(employee_id), MAX(salary) FROM employees);```

- select 后边的子查询

> 仅仅支持标量子查询

> 查询每个部门的员工个数

```SELECT d.*, (SELECT COUNT(*) FROM employees AS e WHERE e.department_id = d.department_id ) FROM department AS d```

- form 后边的子查询

> 将子查询的结果充当一张表，要求必须起别名
> 结果集充当表所以范围随便

> 每个部门的平均工资的工资等级

```SELECT FROM (SELECT AVG(salary) AS ag, department_id FROM employees GROUP BY department_id) AS ag_de inner job_grades AS jb_g on ag_de.ag BETWEEN lowest_sal AND highest_sal```

- exists: 相关子查询

> 布尔类型：返回子查询的结果是否有值

### 分页查询

- limit：(起始索引, 查询条数)

> 当起始索引为0时,可以不填

> 分页请求公式：显示页码：page, 页容量:size
> limit p(age - 1 * size), size

### 联合查询 union

> 要查询的的结果来自于多个表，且多个表没有直接的联系关系，但查询结果一致时

- 注意的事项

1. 查询的列数必须一样
2. 多条查询语句，每一列的类型和数据最好一致
3. 会自动去重，如果不想去重，使用 **union all**

## DML 语言 增删改

### 插入 insert

- 语法
    - - 方式一

> insert into 表名 (列名,....) values (值,...)
> 插入值的类型于列名的类型要保持一致
> 列的顺序可以调换只要值和列一一对应
> 可以省略列名，默认所有列而且列的顺序和表中的顺序完全一致
> 不可以为空的值必须插入值，可以为空的值有两种插入方式
```insert into a (id,age,phone) values("12", "23" , null)``` 第一种
```insert into a (id,age) values("12", "23")``` 第二种，可为空的直接不写
> 支持插入多行
```insert into a (id,age,phone) values("12", "23",null ),("13", "23",null),("14", "23",null)```
> 支持子查询，可以将查询的值插入到对应的表中
```insert into a (id,age,phone) SELRECT("15", 56, "12346679794")```

- - 语法二

> insert into 表名 set 列名=值， 列名=值

### 修改 update

- 语法

    - - 修改单表的记录

```update 表名 set 列=新值, 列=新值,... where 筛选条件```

- - 修改多表的记录

```update 表名1 As 别名1 inner join 表2 AS 别名2 on 连接条件 set 列=新值, 列=新值,... where 筛选条件```

### 删除 delete

- 语法

    - - 单表的删除

```delete 表名 from where 筛选条件```

- - 多表的删除

```delete 表名的别名(要删除的) from 表1 AS 别名 inner 表2 AS 别名 on 连接条件where 删除条件```

- truncate语句: **删除表中所有的数据不允许加where条件**

```truncate table 表名```

- 区别

> 表中有自增长序列，delete 删除后，插入新的值从断点处开始
> truncate删除后从1开始
> truncate没有返回值，delete有返回值
> truncate没有回滚，delete可以回滚

## DDL(数据定义语言)

> 库和表的管理操作
> 创建 create
> 删除 dorp
> 修改: alter

- 创建库
```create database [if not exists] 库名``` 如果没有就创建

- 创建表
```create table [if not exists] 表名(列名， 列的类型，[长度 约束]，列名， 列的类型，[长度 约束]...)```

- 修改库: 一般不修改
  - - 更改字符集

```alter database 表名 character set 字符集```

- 修改表

> 修改列名

```alter table 表名 change column 旧列名 新列名 类型```
> 修改列的类型或者约束

```alter table 表名 modife column 列名 新类型```
> 列的添加删除

```alter table 表名 add column 列名 类型```

```alter table 表名  drop column 列名```
> 修改表名

```alter table 表名 rename to 新的表名```

- 删除库

```drop database [if exists] 库名``` 如果存在就删除

- 删除表

```drop [if exists] table 表名``` 如果存在就删除

- 表的复制

> 复制表2的结构到表1

```create table 表名1 like 表名2```

> 复制表2的结构和内容到表1

```create table 表名1 select * from 表2```

## 常见的数据类型

- 数值型
  - - 整型
  - - 小数: 定点数， 浮点数

- 字符型
  - - 较短的文本：char varchar
  - - 较长的文本：text blob 较长的二进制数据

- 日期型
