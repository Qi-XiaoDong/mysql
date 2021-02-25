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

- 连接分类：

1. 内连接：
    1. 等值连接
    2. 非等值连接
    3. 自连接

2. 外连接
    1. 左外连接
    2. 右外连接
    3. 全外连接 (不支持)

3. 交叉连接

- 等值连接

> 当两张表中的字段名字有冲突，可以使用表名.字段限制

> 为了简单可以给表起别名 FROM 表名 AS 别名

> 起了别名后再SELECT中只能使用别名

```SELECT 字段1, 字段2 FROM 表1, 表2 WHERE 表1.XXX = 表2.XXX```

> 添加筛选

```SELECT 字段1, 字段2 FROM 表1, 表2 WHERE 表1.XXX = 表2.XXX AND 筛选条件```

