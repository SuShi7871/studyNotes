### 1.基本介绍

**数据库：**
	英文单词.，简称DB。按照一定格式存储数据的一些文件的组合。顾名思义：存储数据的仓库，实际上就是一堆文件。这些文  件中存储了具有特定格式的数据。

**数据库管理系统：**
	.Management，简称DBMS。数据库管理系统是专门用来管理数据库中数据的，数据库管理系统可以对数据库当中的数据进	行增删改查。

**常见的数据库管理系统：**
		MySQL.Oracle.MS SqlServer.DB2.sybase等....

**SQL：结构化查询语言**
	程序员需要学习SQL语句，程序员通过编写SQL语句，然后DBMS负责执行SQL语句，最终来完成数据库中数据的增删改查操作。

​	SQL是一套标准，程序员主要学习的就是SQL语句，这个SQL在mysql中可以使用，同时在Oracle中也可以使用在DB2中也可以使用。

三者之间的关系？
	DBMS--执行--> SQL --操作--> DB

先安装数据库管理系统MySQL，然后学习SQL语句怎么写，编写SQL语句之后，DBMS
对SQL语句进行执行，最终来完成数据库的数据管理。

#### 启动和关闭mysql服务

```mysql
net stop 服务名称;
net start 服务名称;
```

#### mysql常用命令：

```mysql
exit  -- 退出mysql
show .s; -- 查看mysql中有哪些数据库
create . bjpowernode;-- 创建数据库
select version();	-- 查看mysql数据库的版本号：
select .();-- 查看当前使用的是哪个数据库
-- 注意：mysql是不见“;”不执行，“;”表示结束！
```

#### 表table

数据库当中是以表格的形式表示数据的。因为表比较直观。任何一张表都有行和列：
	行（row）：被称为数据/记录。
	列（column）：被称为字段。

​	约束：约束也有很多，其中一个叫做唯一性约束，这种约束添加之后，该字段中的数据不能重复。

### 2.SQL语句的分类

SQL语句有很多，最好进行分门别类，这样更容易记忆。分为：
		DQL：
			**数据查询语言**（凡是带有select关键字的都是查询语句）
			select...

​		DML：
​			**数据操作语言**（凡是对表当中的数据进行增删改的都是DML）
​			insert delete update
​			insert 增
​			delete 删
​			update 改

​			这个主要是操作表中的数据data。

​		DDL：
​			**数据定义语言**
​			凡是带有create.drop.alter的都是DDL。
​			DDL主要操作的是表的结构。不是表中的数据。
​			create：新建，等同于增
​			drop：删除
​			alter：修改
​			这个增删改和DML不同，这个主要是对表结构进行操作。

​		TCL：
​			是事务控制语言
​			包括：
​				事务提交：commit;
​				事务回滚：rollback;

​		DCL：
​			是数据控制语言。
​			例如：授权grant.撤销权限revoke....

不看表中的数据，只看表的结构，有一个命令：

```mysql
desc 表名;	-- describe缩写为：desc
```

#### 1.简单查询

​	查询一个字段

```mysql
select 字段名 from 表名;
```

​		其中要注意：
​			select和from都是关键字。字段名和表名都是标识符。

​	强调：
​		对于SQL语句来说，是通用的，
​		所有的SQL语句以“;”结尾。
​		另外SQL语句**不区分大小写**，都行。

```mysql
select deptno,dname as deptname from dept;-- 给查询的列起别名？
```

​	使用as关键字起别名。
​	注意：只是将显示的查询结果列名显示为deptname，原表列名还是叫：dname
​	使用as关键字起别名。

```mysql
-- as关键字可以省略吗？可以的
select deptno,dname deptname from dept;
```

​	假设起别名的时候，别名里面有空格，怎么办？
​		select deptno,dname dept name from dept;
​		DBMS看到这样的语句，进行SQL语句的编译，不符合语法，编译报错。

```mysql
select deptno,dname 'dept name' from dept; -- 加单引号，解决
select deptno,dname "dept name" from dept; -- 加双引号，解决
```

​		**注意：在所有的数据库当中，字符串统一使用单引号括起来，**
​		**单引号是标准，双引号在oracle数据库中用不了。但是在mysql中可以使用。**

​		再次强调：数据库中的字符串都是采用单引号括起来。这是标准的。双引号不标准。

```mysql
select ename,sal*12 as yearsal from emp; -- 查询12月的工资 
select ename,sal*12 as '年薪' from emp; -- 别名是中文，用单引号括起来。
```

#### 2.条件查询

​	语法格式：

```mysql
		select
			字段1,字段2,字段3....
		from 
			表名
		where
			条件;
```

```mysql

	-- 查询薪资等于800的员工姓名和编号？
	select empno,ename from emp where sal = 800;
	-- 小于号和大于号组成的不等号
	select empno,ename from emp where sal <> 800;
	select empno,ename,sal from emp where sal < 2000;
	-- between … and …. 两个值之间, 等同于 >= and <=
	-- 查询薪资在2450和3000之间的员工信息？包括2450和3000
	-- 第一种方式：>= and <= （and是并且的意思。）
	select empno,ename,sal from emp where sal >= 2450 and sal <= 3000;
	select empno,ename,sal from emp where sal between 2450 and 3000;
	-- 使用between和and的时候，必须遵循左小右大。
	-- between and是闭区间，包括两端的值。
	-- 错误的写法
	-- select empno,ename,sal,comm from emp where comm = null;
	select empno,ename,sal,comm from emp where comm is null;
	-- 注意：在数据库当中null不能使用等号进行衡量。需要使用is null，因为数据库中的null代表什么也没有，它不是一个值，所以不能使用等号衡量。
	select empno,ename,sal,comm from emp where comm is not null;
	and 并且
	查询工作岗位是MANAGER并且工资大于2500的员工信息？
	select 
		empno,ename,job,sal 
	from 
		emp 
	where 
		job = 'MANAGER' and sal > 2500;
```

```mysql
or 或者
-- 查询工作岗位是MANAGER或SALESMAN的员工？
	select 
		empno,ename,job
	from
		emp
	where 
		job = 'MANAGER' or job = 'SALESMAN';
-- and和or同时出现的话，and优先级比or高。
-- 查询工资大于2500，并且部门编号为10或20部门的员工？
-- 下面的语句会先执行and，然后执行or。
select * from emp where sal > 2500 and deptno = 10 or deptno = 20;
-- and和or同时出现，and优先级较高。如果想让or先执行，需要加“小括号”
select * from emp where sal > 2500 and (deptno = 10 or deptno = 20);
-- 注意：以后在开发中，如果不确定优先级，就加小括号就行了。
-- in 包含，相当于多个 or （not in 不在这个范围中）
select empno,ename,job from emp where job = 'MANAGER' or job = 'SALESMAN';
select empno,ename,job from emp where job in('MANAGER', 'SALESMAN');
-- 注意：in不是一个区间。in后面跟的是具体的值。
-- 查询薪资是800和5000的员工信息？
select ename,sal from emp where sal = 800 or sal = 5000;
select ename,sal from emp where sal in(800, 5000); -- 这个不是表示800到5000都找出来。
select ename,sal from emp where sal in(800, 5000, 3000);
--  not in 表示不在这几个值当中的数据。
select ename,sal from emp where sal not in(800, 5000,3000);
-- not 可以取非，主要用在 is 或 in 中
	is null
	is not null
	in
	not in
```

##### 1.like 

​	称为模糊查询，支持%或下划线匹配
​	%匹配任意多个字符
​	下划线：任意一个字符。
​	（%是一个特殊的符号，_ 也是一个特殊符号）

```MySQL
-- 找出名字中含有O的？
select ename from emp where ename like '%O%';
-- 找出名字以T结尾的？
select ename from emp where ename like '%T';
-- 找出名字以K开始的？
select ename from emp where ename like 'K%';
-- 找出第二个字每是A的？
select ename from emp where ename like '_A%';
-- 找出第三个字母是R的？
select ename from emp where ename like '__R%';
-- 找出名字中有“_”的？
select name from t_student where name like '%_%'; -- 这样不行。
-- 找出名字中有“_”的(正确写法)
select name from t_student where name like '%\_%'; --  \转义字符。
```

##### 2.排序

```mysql
-- 查询所有员工薪资，排序
select ename,sal from emp order by sal; --  默认是升序！！！
```

降序

```mysql
-- 指定降序：
select ename,sal from emp order by sal desc;
```

##### 3.多个字段排序

​	查询员工名字和薪资，要求按照薪资升序，如果薪资一样的话，再按照名字升序排列。

```mysql
 --  sal在前，起主导，只有sal相等的时候，才会考虑启用ename排序。
 select  ename,sal  from  emp   order by   sal asc, ename asc;
-- 找出工资在1250到3000之间的员工信息，要求按照薪资降序排列。
select ename,sal from emp where sal between 1250 and 3000order by sal desc;
```

```mysql
-- 关键字顺序不能变：
	select
		...
	from
		...
	where
		...
	order by
		...
	
```

以上语句的执行顺序必须掌握：
	第一步：from
	第二步：where
	第三步：select
	第四步：order by（排序总是在最后执行！）

##### 4.数据处理函数

数据处理函数又被称为单行处理函数

单行处理函数的特点：一个输入对应一个输出。和单行处理函数相对的是：多行处理函数。
多行处理函数特点：多个输入，对应1个输出！

单行处理函数常见的有哪些？

```mysql
	-- lower 转换小写
 	select lower(ename) as ename from emp;
	-- 14个输入，最后还是14个输出。这是单行处理函数的特点。
	-- upper 转换大写
	 select * from t_student;
	 select upper(name) as name from t_student;
	-- substr 取子串（substr( 被截取的字符串, 起始下标,截取的长度)）
	select substr(ename, 1, 1) as ename from emp;
	-- 注意：起始下标从1开始，没有0.
	-- 找出员工名字第一个字母是A的员工信息？
	-- 第一种方式：模糊查询
	select ename from emp where ename like 'A%';
	-- 第二种方式：substr函数
	select ename from emp where substr(ename,1,1) = 'A';
	-- 首字母大写？
	select upper(substr(name,1,1)) from t_student;
	select substr(name,2,length(name) - 1) from t_student;
	select concat(upper(substr(name,1,1)),substr(name,2,length(name) - 1)) as result from t_student;
	-- concat函数进行字符串的拼接
	select concat(empno,ename) from emp;
	-- length 取长度
	select length(ename) enamelength from emp;

	-- trim 去空格
	select * from emp where ename = '  KING';

	select * from emp where ename = trim('   KING');

	-- str_to_date 将字符串转换成日期
	-- date_format 格式化日期
	-- format 设置千分位

	-- case..when..then..when..then..else..end
	-- 当员工的工作岗位是MANAGER的时候，工资上调10%，当工作岗位是-- SALESMAN的时候，工资上调50%,其它正常。
	-- 注意：不修改数据库，只是将查询结果显示为工资上调）
	select 
		ename,
		job, 
		sal as oldsal,
		(case job when 'MANAGER' then sal*1.1 when 'SALESMAN' then sal*1.5 	else sal end) as newsal 
	from emp;
	-- round 四舍五入
	select 字段 from 表名;
	select ename from emp;
	select 'abc' from emp; --  select后面直接跟“字面量/字面值”
	select 'abc' as bieming from emp;
	select 1000 as num from emp; --  1000 也是被当做一个字面量/字面值。
	-- 结论：select后面可以跟某个表的字段名（可以等同看做变量名），也可以跟-- 字面量/字面值（数据）。
 	select round(1236.567, 0) as result from emp; -- 保留整数位。
	select round(1236.567, 1) as result from emp; -- 保留1个小数
	select round(1236.567, 2) as result from emp; -- 保留2个小数
	select round(1236.567, -1) as result from emp; --  保留到十位。
	-- rand() 生成随机数
 	select round(rand()*100,0) from emp; --  100以内的随机数
	-- ifnull 可以将null转换成一个具体值
	-- ifnull是空处理函数，专门处理空的。
	-- 在所有数据库当中，只要有NULL参与的数学运算，最终结果就是NULL。
	select ename, sal + comm as salcomm from emp;
	select ename, (sal + comm) * 12 as yearsal from emp;
	-- 注意：NULL只要参与运算，最终结果一定是NULL。为了避免这个现象，需要使用ifnull函数。
	-- ifnull函数用法：ifnull(数据, 被当做哪个值)
	-- 如果“数据”为NULL的时候，把这个数据结构当做哪个值。
	-- 补助为NULL的时候，将补助当做0
	select ename, (sal + ifnull(comm, 0)) * 12 as yearsal from emp;
```

##### 10.4.分组函数

（多行处理函数）

```mysql
-- 多行处理函数的特点：输入多行，最终输出一行。
	count	计数
	sum	求和
	avg	平均值
	max	最大值
	min	最小值
注意：
	分组函数在使用的时候必须先进行分组，然后才能用。
	如果你没有对数据进行分组，整张表默认为一组。
		-- 找出最高工资？
	 	select max(sal) from emp;
		-- 找出最低工资？
	 	select min(sal) from emp;
		-- 计算工资和：
		select sum(sal) from emp;
		-- 计算平均工资：
    	select avg(sal) from emp;
		-- 计算员工数量？
 		select count(ename) from emp;
 分组函数在使用的时候需要注意哪些？
	第一点：分组函数自动忽略NULL，不需要提前对NULL进行处理。
		select sum(comm) from emp;
		select count(comm) from emp;
		select avg(comm) from emp;
	第二点：分组函数中count(*)和count(具体字段)有什么区别？
		select count(*) from emp;
		select count(comm) from emp;
		count(具体字段)：表示统计该字段下所有不为NULL的元素的总数。
		count(*)：统计表当中的总行数。(只要有一行数据count则++）
		因为每一行记录不可能都为NULL，一行数据中有一列不为NULL，则这行数据就是有效的。
	第三点：分组函数不能够直接使用在where子句中。
		找出比最低工资高的员工信息。
	select ename,sal from emp where sal > min(sal);-- 错误
			表面上没问题，运行一下？
				ERROR 1111 (HY000): Invalid use of group 
	第四点：所有的分组函数可以组合起来一起用。
select sum(sal),min(sal),max(sal),avg(sal),count(*) from emp;
```

##### **10.5.分组查询**

（非常重要：五颗星*）

什么是分组查询？
	在实际的应用中，可能有这样的需求，需要先进行分组，然后对每一组的数据进行操作。

group by:按照某个字段或者某些字段进行分组。

having：是对分组之后的数据进行再次过滤。

```mysql
		select
			...
		from
			...
		group by	
```

将之前的关键字全部组合在一起，来看一下他们的执行顺序？
	select          5
		...
	from             1
		...
	where      	2
		...
	group by     3
		...
	order by     4
		...

	以上关键字的顺序不能颠倒，需要记忆。以上关键字的执行顺序是什么？

​			from

​			where

​			group by

​			select

​			order by

为什么分组函数不能直接使用在where后面？
select ename,sal from emp where sal > min(sal);-- 报错。
	因为分组函数在使用的时候必须先分组之后才能使用。
	where执行的时候，还没有分组。所以where后面不能出现分组函数。

分组函数一般都会和group by联合使用，并且每一个分组函数(count,sum,avg,max,min)都是在group by结束之后才会执行。当一张表没有group by的话整张表的数据会自成一组。

```mysql
-- 找出工资高于平均工资的员工

-- 方法一：

select avg(sal)   from  emp;

select  ename,sal from emp where sal > 123

-- 方法2：

 select  ename,sal from emp where sal > (select avg(sal)   from  emp）;
 
 -- 这个没有分组，为啥sum()函数可以用呢？因为select在group by之后执行。
 -- select sum(sal) from emp; 
	
```

```mysql
-- 找出每个工作岗位的工资和？实现思路：按照工作岗位分组，然后对工资和。
	select job,sum(sal)  from emp  group by job; -- 正确
```

以上这个语句的执行顺序？
			先从emp表中查询数据。
			根据job字段进行分组。
			然后对每一组的数据进行sum(sal)

​	重点结论：
​		**在一条select语句当中，如果有group by语句的话，select后面只能跟：参加分组的字段，以及分组函数。其它的一律不能跟。**

```mysql
-- 找出每个部门的最高薪资，实现思路是什么？，按照部门编号分组，求每一组的最大值。
		select后面添加ename字段没有意义，另外oracle会报错。
select ename,deptno,max(sal) from emp group by deptno; -- 错误
select deptno,max(sal) from emp group by deptno; 
```

```mysql
-- 找出“每个部门，不同工作岗位”的最高薪资？
	技巧：两个字段联合成1个字段看。（两个字段联合分组）
	select 
		deptno, job, max(sal)
	from
		emp
	group by
		deptno, job;
```

##### 10.6.having语句	

使用having可以对分完组之后的数据进一步过滤。
**having不能单独使用，having不能代替where，having必须和group by联合使用**。

```mysql
	-- 找出每个部门最高薪资，要求显示最高薪资大于3000的？
	-- 第一步：找出每个部门最高薪资
	-- 按照部门编号分组，求每一组最大值。
	select deptno,max(sal) from emp group by deptno;
	-- 第二步：要求显示最高薪资大于3000
	select  deptno,max(sal)  from  emp  group by  deptno havingmax(sal) > 3000;
```


```mysql
-- 思考一个问题：以上的sql语句执行效率是不是低？比较低，实际上可以这样考虑：先将大于3000的都找出来，然后再分组。
		select 
			deptno,max(sal)
		from
			emp
		where
			sal > 3000
		group by
			deptno;
-- 优化策略：
where和having，优先选择where，where实在完成不了了，再选择having。
	where都没办法解决的
		-- 找出每个部门平均薪资，要求显示平均薪资高于2500的。
		第一步：找出每个部门平均薪资
	select deptno,avg(sal) from emp group by deptno;
		第二步：要求显示平均薪资高于2500的
			select 
				deptno,avg(sal) 
			from 
				emp 
			group by 
				deptno
			having
				avg(sal) > 2500;
```

##### 10.7.单表查询总结

​	select 
​		...
​	from
​		...
​	where
​		...
​	group by
​		...
​	having
​		...
​	order by
​		...

以上关键字只能按照这个顺序来，不能颠倒。

执行顺序？
	1. from
	2. where
	3. group by
	4. having
	5. select
	6. order by

从某张表中查询数据，
先经过where条件筛选出有价值的数据。
对这些有价值的数据进行分组。
分组之后可以使用having继续筛选。
select查询出来。
最后排序输出！

##### 10.8.去重

把查询结果去除重复记录【distinct】

​	注意：原表数据不会被修改，只是查询结果去重。
​	去重需要使用一个关键字：distinct

```mysql
select distinct job from emp;
-- 这样编写是错误的，语法错误，distinct只能出现在所有字段的最前方。
	select ename,distinct job from emp;	 -- 错误
-- distinct出现在job,deptno两个字段之前，表示两个字段联合起来去重。
	 select distinct job,deptno from emp;
	 -- 例题，统计一下工作岗位的数量？
	select count(distinct job) from emp;
```

#### 11.连接查询

1.多张表联合起来查询数据，被称为连接查询。

2.根据语法的年代分类：
		SQL92：1992年的时候出现的语法
		SQL99：1999年的时候出现的语法

3.根据表连接的方式分类：
		内连接：
					等值连接
					非等值连接
					自连接

​		外连接：
​					左外连接（左连接）
​					右外连接（右连接）

​		全连接（不讲）

4.当两张表进行连接查询，没有任何条件限制的时候，最终查询结果条数，是两张表条数的乘积，这种现象被称为：笛卡尔积现象。

如何避免笛卡尔积现象？连接时加条件，满足这个条件的记录被筛选出来！

```mysql
select ename,dname from emp,dept where emp.deptno= dept.deptno;
```

5.表起别名。很重要。效率问题。

```mysql
select e.ename,d.dname from emp e,dept d where e.deptno = d.deptno;
```

##### 11.1.等值连接和非等值连接

条件是等量关系

SQL92语法：

```mysql
select 
		e.ename,d.dname
	from
		emp e, dept d
	where
		e.deptno = d.deptno;
```

sql92的缺点：结构不清晰，表的连接条件，和后期进一步筛选的条件，都放到了where后面。

SQL99语法：（常用）

```mysql
select 
		e.ename,d.dname
	from
		emp e
	join
		dept d
	on
		e.deptno = d.deptno;
```

sql99优点：表连接的条件是独立的，连接之后，如果还需要进一步筛选，再往后继续添加where。

非等值连接：条件是非等量关系

找出每个员工的薪资等级，要求显示员工名.薪资.薪资等级

```mysql
select e.ename,e.sal,s.grade from emp e join salgrade s on e.SAL BETWEEN s.LOSAL and s.HISAL
```

内连接之自连接：最大的特点是一张表看作两张表。

```mysql
-- 查询员工的上级领导，要求显示员工名和对应的领导名
emp a 员工表
+-------+--------+------+
| empno | ename  | mgr  |
+-------+--------+------+
|  7369 | SMITH  | 7902 |
|  7499 | ALLEN  | 7698 |
|  7521 | WARD   | 7698 |
|  7566 | JONES  | 7839 |
|  7654 | MARTIN | 7698 |
|  7698 | BLAKE  | 7839 |
|  7782 | CLARK  | 7839 |
|  7788 | SCOTT  | 7566 |
|  7839 | KING   | NULL |
|  7844 | TURNER | 7698 |
|  7876 | ADAMS  | 7788 |
|  7900 | JAMES  | 7698 |
|  7902 | FORD   | 7566 |
|  7934 | MILLER | 7782 |
+-------+--------+------+
emp b 领导表
+-------+--------+------+
| empno | ename  | mgr  |
+-------+--------+------+
|  7369 | SMITH  | 7902 |
|  7499 | ALLEN  | 7698 |
|  7521 | WARD   | 7698 |
|  7566 | JONES  | 7839 |
|  7654 | MARTIN | 7698 |
|  7698 | BLAKE  | 7839 |
|  7782 | CLARK  | 7839 |
|  7788 | SCOTT  | 7566 |
|  7839 | KING   | NULL |
|  7844 | TURNER | 7698 |
|  7876 | ADAMS  | 7788 |
|  7900 | JAMES  | 7698 |
|  7902 | FORD   | 7566 |
|  7934 | MILLER | 7782 |
+-------+--------+------+

-- 技巧，员工的领导编号 = 领导的员工编号
select a.ename,b.ename from emp a  join emp b on a.mgr = b.EMPNO;
```

##### 11.2.外连接

​		内连接：假设a和b表进行连接，使用内连接的话，凡是a表和b表能够匹配上的记录查出来，这就是**内连接。ab两张表是平等的**。

​		外连接：假设a和b表进行连接，使用外连接的话，有一张表是主表，有一张是副表，主要查询主表中的数据，捎带着查询副表，当副表中的数据没有和主表中的数据匹配，副表自动模拟出null与之匹配。

带有right的是右外连接，又叫做右连接。带有left的是左外连接，又叫做左连接。任何一个右连接都有左连接的写法任何一个左连接都有右连接的写法。

**right代表什么：表示将join关键字右边的这张表看成主表**，主要是为了将这张表的数据全部查询出来，捎带着关联查询左边的表。在外连接当中，两张表连接，产生了主次关系。

```mysql
-- 查询每个员工的上级领导，要求显示所有员工的名字和领导名
select a.ename,b.ename from emp a left join emp b on a.mgr = b.empno
```

```mysql
-- 查询哪个部门没有员工
select d.* from emp e RIGHT JOIN dept d on e.deptno =d.deptno where e.empno is NULL
```

三张表，四张表怎么连接

**一条SQL中内连接和外连接可以混合。都可以出现！**

```mysql
-- 找出每个员工的部门名称以及工资等级，要求显示员工名.部门名.薪资.薪资等级？
	select 
		e.ename,e.sal,d.dname,s.grade
	from
		emp e
	join
		dept d
	on 
		e.deptno = d.deptno
	join
		salgrade s
	on
		e.sal between s.losal and s.hisal;
```

##### 11.3.子查询

​	什么是子查询？
​		select语句中嵌套select语句，被嵌套的select语句称为子查询。where		子句中的子查询

```mysql
-- 找出比最低工资高的员工姓名和工资
select ename,sal from emp where sal>min(sal)-- 错误，where子句中不能直接使用分组函数。

select  ename ,sal from emp where sal >	(select avg(sal)  from  emp)
```

​			from子句中的子查询

```mysql
-- 找出每个部门的平均薪水的部门等级

-- 第一步，先找出每个部门的平均薪水

select deptno,avg(sal)  as  avgsal from emp group by   deptno 

-- 第二部，将上面查出的信息作为一张新表与薪水登记表连接起来

select t.*,s.grade from (select deptno,avg(sal)  as  avgsal from emp group by  deptno)  t join salgrade s on t.avgsal  between s.losal   on s.hisal;

-- 找出每个部门平均的薪水等级
-- 第一步找出每个员工的薪水等级
select e.ename ,e.sal,e.deptno,s.grade from emp e join salgrade s on e.sal between s.losal and s.hisal
-- 基于以上的结果，继续按照deptno分组，求grade的平均值
select e.deptno,avg(s.grade) from emp e join salgrade s on e.sal between s.losal and s.hisal group by e.deptno
```

select后面出现的子查询（这个内容不需要掌握，了解即可！！！）

##### 11.4.union合并查询结果集

```mysql
-- 查询工作岗位是MANAGER和SALESMAN的员工
select ename,job from emp where job = 'MANAGER' or job = 'SALESMAN';
select ename,job from emp where job   in('MANAGER','SALESMAN');
```

​	union的效率要高一些。对于表连接来说，每连接一次新表，则匹配的次数满足笛卡尔积，成倍的翻。但是union可以减少匹配的次数。在减少匹配次数的情况下，还可以完成两个结果集的拼接。

##### 11.5.limit（非常重要）

limit作用：将查询结果集的一部分取出来。通常使用在分页查询当中。

​		完整用法：limit startIndex, length
​				startIndex是起始下标，length是长度。
​				起始下标从0开始。

```mysql
select ename,sal from emp order by sal desc limit 0,5
-- 取出工资排名在[5-9]名的员工
select ename,sal from emp order by sal desc limit 4, 5;
```

​		每页显示3条记录
​				第1页：limit 0,3		[0 1 2]
​				第2页：limit 3,3		[3 4 5]
​				第3页：limit 6,3		[6 7 8]
​				第4页：limit 9,3		[9 10 11]

​				第pageNo页：limit (pageNo - 1) * pageSize  , pageSize

#### 12.DDL语句

##### 12.1.表的创建

###### 1.建表属于DDL

​	DDL包括：create drop alter)

```mysql
create table 表名(
		字段名1 数据类型, 
		字段名2 数据类型, 
		字段名3 数据类型
	);
```

###### 2.mysql中的数据类型

​				varchar(最长255)
​						**可变长度的字符串**
​						比较智能，节省空间。
​						会根据实际的数据长度动态分配空间。

​						优点：节省空间
​						缺点：需要动态分配空间，速度慢。

​				char(最长255)
​						定长字符串
​						不管实际的数据长度是多少。
​						分配固定长度的空间去存储数据。
​						使用不恰当的时候，可能会导致空间的浪费。

​						优点：不需要动态分配空间，速度快。
​						缺点：使用不当可能会导致空间的浪费。

​			varchar和char我们应该怎么选择？
​					性别字段你选什么？因为性别是固定长度的字符串，所以选择char。
​					姓名字段你选什么？每一个人的名字长度不同，所以选择varchar。

​			int(最长11)
​					数字中的整数型。等同于java的int。

​			bigint
​					数字中的长整型。等同于java中的long。

​			float	
​					单精度浮点型数据

​			double
​					双精度浮点型数据

​			date
​					短日期类型

​			datetime
​					长日期类型

​			clob
​				字符大对象
​				最多可以存储4G的字符串。
​				比如：存储一篇文章，存储一个说明。超过255个字符的都要采用				CLOB字符大对象来存储。Character Large OBject:CLOB

​			blob
​					二进制大对象
​					Binary Large OBject
​					专门用来存储图片.声音.视频等流媒体数据。
​					往BLOB类型的字段上插入数据的时候，例如插入一个图片.视频					等，你需要使用IO流才行。

###### 3.插入数据insert (DML）

```mysql
insert into 表名(字段名1,字段名2,字段名3...) values(值1,值2,值3);
```

​	注意：字段名和值要一一对应。 数量要对应。数据类型要对应。

​		        insert语句但凡是执行成功了，那么必然会多一条记录。没有给其它字段指定值的话，默认值是NULL。

​				insert语句中的“字段名”可以省略吗？可以	

```mysql
insert into t_student values(2); -- 错误的
		--  注意：前面的字段名省略的话，等于都写上了！所以值也要都写上！
insert into t_student values(2, 'lisi', 'f', 20, 'lisi@123.com');
```

​	insert数字格式化

​			格式化数字：format(数字, '格式')

```mysql
	select ename,format(sal,'$999,999') as sal from emp;
```
​			str_to_date：将字符串varchar类型转换成date类型
​			date_format：将date类型转换成具有一定格式的varchar字符串类型。

​			数据库有一条命名规范：所有的标识符都是全部小写，单词和单词之间使用下划线进行衔接。

​			str_to_date函数可以将字符串转换成日期类型date

```mysql
	-- 语法格式：
	-- %Y年%m月%d日%h	时   %i	分   %s	秒
	-- str_to_date('字符串日期', '日期格式')
into t_user(id,name,birth) VALUES(1,'zhangsan',STR_TO_DATE('01-10-1990','%d-%m-%Y'));
```

​			注意：如果你提供的日期字符串是%Y-%m-%d这个格式，str_to_date函数就不需要了

```mysql
		insert into t_user(id,name,birth) values(2, 'lisi', '1990-10-01');
```

​			date_format，这个函数可以将日期类型转换成特定格式的字符串。

```mysql
	select id,name,DATE_FORMAT(birth,'%m/%d/%y') as birth from t_user
```

​		date_format函数怎么用？date_format(日期类型数据, '日期格式')这个函数通常使用在查询日期方面。设置展示的日期格式。

```mysql
	select id ,name,DATE_FORMAT(birth,'%y/%m/%d') as birth from t_user;
	java中的日期格式。yyyy-MM-dd HH:mm:ss SSS
```

​		date和datetime两个类型的区别

​				date是短日期：只包括年月日信息。
​				datetime是长日期：包括年月日时分秒信息。

​				mysql短日期默认格式：%Y-%m-%d
​				mysql长日期默认格式：%Y-%m-%d %h:%i:%s

​	**insert语句可以一次插入多条记录【掌握】**

```mysql
		-- 语法
		insert into t_user(字段名1,字段名2) values(),(),(),();
		insert into t_user(id,name,birth) VALUES(1,'zs','1980-10-11'), 
		(2,'lisi','1981-10-11'),
		(3,'wangwu','1982-10-11');		
```
###### 4.修改update（DML）

```mysql
update 表名 set 字段名1=值1,字段名2=值2,字段名3=值3... where 条件;

-- 注意：没有条件限制会导致所有数据全部更新。
```

```mysql
update t_user set name = 'jack', birth = '2000-10-11',where id = 2;

	-- 更新所有
	update t_user set name = 'abc';
```

###### 5.删除数据 delete (DML）

```mysql
	delete from 表名 where 条件;
	-- 注意：没有条件，整张表的数据会全部删除！
	delete from t_user where id = 2;
	delete from t_user; --  删除所有！
```

​	快速创建表

```mysql
 create table emp2 as select * from emp;
```

​		原理：
​			将一个查询结果当做一张表新建,这个可以完成表的快速复制,表创建出·			来，同时表中的数据也存在了

#### 13.约束

##### 1.内容

主要包括以下几个约束

​	非空约束：not null
​	唯一性约束: unique
​	主键约束: primary key （简称PK）
​	外键约束：foreign key（简称FK）
​	检查约束：check（mysql不支持，oracle支持）

##### 2.非空约束

​		非空约束not null约束的字段不能为NULL

```mysql
	drop table if exists t_vip;
	create table t_vip(
		id int,
		name varchar(255) not null  --  not null只有列级约束，没有表级约束！
	);
```

​		批量的执行SQL语句，可以使用sql脚本文件，mysql当中执行sql脚本

```mysql
source D:\course\03-MySQL\document\vip.sql
```

##### 3.唯一性约束

​	唯一性约束**unique约束的字段不能重复，但是可以为NULL**。

```mysql
drop table if exists t_vip;
create table t_vip(
		id int,
		name VARCHAR(255) UNIQUE,
		email varchar(255)
	
);

insert into t_vip(id,name,email) VALUES(1,'zs','zs@qq.com');
insert into t_vip(id,NAME,email) VALUES(2,'zs','zs@qq.com');
[Err] 1062 - Duplicate entry 'zs' for key 'name'
-- name字段虽然被unique约束了，但是可以为NULL。
	insert into t_vip(id) values(4);
	insert into t_vip(id) values(5);
```

新需求：name和email两个字段联合起来具有唯一性

```mysql
drop table if exists t_vip;
	create table t_vip(
			id int,
			name varchar(255),
            --  约束没有添加在列的后面，这种约束被称为表级约束。
            email varchar(255),unique(name,email) 
	);
insert into t_vip(id,name,email) values(1,'zhangsan','zhangsan@123.com');
	insert into t_vip(id,name,email) values(2,'zhangsan','zhangsan@sina.com');
```

需要给多个字段联合起来添加某一个约束的时候，需要使用表级约束。

unique 和not null可以联合操作

​		在mysql当中，如果一个字段同时被not null和unique约束的话，
​		该字段自动变成主键字段。（注意：oracle中不一样！）

```mysql
drop table if EXISTS t_vip;
CREATE table t_vip(
		id int,
		name VARCHAR(255) not null UNIQUE
);
desc t_vip
insert into t_vip(id,name) VALUES(1,'zhangsan');
insert into t_vip(id,name) VALUES(2,'zhangsan');-- 错误了：name不能重复
insert into t_vip(id) values(2); -- 错误了：name不能为NULL。
```

##### 4.主键约束pk（重要）

​		主键约束：就是一种约束。
​		主键字段：该字段上添加了主键约束，这样的字段叫做：主键字段
​		主键值：主键字段中的每一个值都叫做：主键值。

​		主键值是每一行记录的唯一标识。

​		记住：任何一张表都应该有主键，没有主键，表无效

​		主键的特征：**not null + unique（主键值不能是NULL，同时也不能重复！**）

```mysql
	drop table if EXISTS t_vip;
	create table t_vip(
 		id int PRIMARY key,
		name VARCHAR(255)
	);
	insert into t_vip(id,name) values(1,'zhangsan');
	insert into t_vip(id,name) values(2,'lisi');
	INSERT INTO t_vip(id,name) VALUES(1,'wangwu');-- 错误主键不能重复
	insert into t_vip(name) values('zhaoliu'); -- 错误，不能为空
```
表级约束

表级约束主要是给多个字段联合起来添加约束

```mysql
drop table if exists t_vip;
		--  id和name联合起来做主键：复合主键
		create table t_vip(
			id int,
			name varchar(255),
			email varchar(255),
			primary key(id,name)
		);
	insert into t_vip(id,name,email) values(1,'zhangsan','zhangsan@123.com');
	insert into t_vip(id,name,email) values(1,'lisi','lisi@123.com');-- 错误：不能-- 重复		
```

注意：

​	1.在实际开发中不建议使用：复合主键。建议使用单一主键！
​	因为主键值存在的意义就是这行记录的身份证号，只要意义达到即可，单一主键可以做到。
​		复合主键比较复杂，不建议使用

​	2.一张表，主键约束只能添加1个。（主键只能有1个。）

​    3.主键值建议使用：int.bigint.char等类型。

​	不建议使用：varchar来做主键。主键值一般都是数字，一般都是定长的！

4.主键除了：单一主键和复合主键之外，还可以这样进行分类？
			自然主键：主键值是一个自然数，和业务没关系。
			业务主键：主键值和业务紧密关联，例如拿银行卡账号做主键值。这就是业务主键！

​	在实际开发中使用业务主键多，还是使用自然主键多一些？
​		自然主键使用比较多，因为主键只要做到不重复就行，不需要有意义。
​	业务主键不好，因为主键一旦和业务挂钩，那么当业务发生变动的时候，
​		可能会影响到主键值，所以业务主键不建议使用。尽量使用自然主键。

5.在mysql当中，有一种机制，可以帮助我们自动维护一个主键值？

```mysql
drop table if exists t_vip;
create table t_vip(
	id int primary key auto_increment, -- auto_increment表示自增，从1开始，以1递增！
	name varchar(255)
);
```

##### 5.外键约束FK（重要）

​			外键约束：一种约束（foreign key）
​			外键字段：该字段上添加了外键约束
​			外键值：外键字段当中的每一个值。

​	业务背景：
​			请设计数据库表，来描述“班级和学生”的信息？

​				第一种方案：班级和学生存储在一张表中

​					数据冗余，空间浪费
​					这个设计是比较失败的

​				第二种方案：班级一张表.学生一张表

```mysql
			drop table if EXISTS t_student;
			drop table if EXISTS t_class;
			CREATE table t_class(
 				classno int PRIMARY KEY,
				classname VARCHAR(255)
			);
			create table t_student(
				no int PRIMARY key auto_increment,
				name VARCHAR(255),
				cno int,
				foreign key(cno) REFERENCES t_class(classno)
			);
```

​						t_class是父表，t_student是子表

​						删除表的顺序？
​							先删子，再删父。

​						创建表的顺序？
​							先创建父，再创建子。

​						删除数据的顺序？
​							先删子，再删父。

​						插入数据的顺序？
​							先插入父，再插入子。

##### 6.检查约束

#### 14.存储引擎（了解）

##### 1.什么是存储引擎

​	存储引擎是MySQL中特有的一个术语，其它数据库中没有。（Oracle中有，但是不叫这个名字）
​	存储引擎这个名字高端大气上档次。实际上存储引擎是一个表存储/组织数据的方式。不同的存储引擎，表存储数据的方式不同。

可以在建表的时候给表指定存储引擎。

```mysql
CREATE TABLE `t_student` (
	  `no` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(255) DEFAULT NULL,
	  `cno` int(11) DEFAULT NULL,
	  PRIMARY KEY (`no`),
	  KEY `cno` (`cno`),
	  CONSTRAINT `t_student_ibfk_1` FOREIGN KEY (`cno`) REFERENCES `t_class` (`classno`)
	) ENGINE=InnoDB AUTO_INCREMENT=11 DEFAULT CHARSET=utf8
```

在建表的时候可以在最后小括号的")"的右边使用：
		ENGINE来指定存储引擎。
		CHARSET来指定这张表的字符编码方式。

结论：
		**mysql默认的存储引擎是：InnoDB**
		**mysql默认的字符编码方式是：utf8**

```mysql
-- 建表时指定存储引擎，以及字符编码方式。
create table t_product(
	id int primary key,
	name varchar(255)
)engine=InnoDB default charset=gbk;
-- 查看mysql支持的存储引擎
show engines 
-- 查看数据库版本
select version();
```
##### 2.常见的存储引擎

  MyISAM存储引擎
    	 它管理的表具有以下特征：
		 使用三个文件表示每个表：
			格式文件 — 存储表结构的定义（mytable.frm）
			数据文件 — 存储表行的内容（mytable.MYD）
			索引文件 — 存储表上索引（mytable.MYI）：索引是一本书的目录，缩小扫描范围，提高查询效率的一种机制。可被转换为压			缩.只读表来节省空间

​		MyISAM存储引擎特点：
​			可被转换为压缩.只读表来节省空间
​			这是这种存储引擎的优势
​			MyISAM不支持事务机制，安全性低。

InnoDB存储引擎

​		这是mysql默认的存储引擎，同时也是一个重量级的存储引擎。
​		InnoDB支持事务，支持数据库崩溃后自动恢复机制。
​		**InnoDB存储引擎最主要的特点是：非常安全。**

​	它管理的表具有下列主要特征：
​		每个 InnoDB 表在数据库目录中以.frm 格式文件表示
​		InnoDB 表空间 tablespace 被用于存储表的内容（表空间是一个逻辑名称。表空间存储数据+索引。）

​		提供一组用来记录事务性活动的日志文件
​	 	用 COMMIT(提交).SAVEPOINT 及ROLLBACK(回滚)支持事务处理提供全 ACID 兼容

​		在 MySQL 服务器崩溃后提供自动恢复
​	    多版本（MVCC）和行级锁定
​	   支持外键及引用的完整性，包括级联删除和级联更新

​		InnoDB最大的特点就是支持事务：
​				以保证数据的安全。效率不是很高，并且也不能压缩，不能转换为只读，不能很好的节省存储空间。

MEMORY存储引擎

​		使用 MEMORY 存储引擎的表，其数据存储在内存中，且行的长度固定，
​		这两个特点使得 MEMORY 存储引擎非常快。

​		MEMORY 存储引擎管理的表具有下列特征：
​			– 在数据库目录内，每个表均以.frm 格式的文件表示。
​			– 表数据及索引被存储在内存中。（目的就是快，查询快！）
​			– 表级锁机制。
​			– 不能包含 TEXT 或 BLOB 字段。

​		MEMORY 存储引擎以前被称为HEAP 引擎。

​		**MEMORY引擎优点：查询效率是最高的。不需要和硬盘交互。**
**​		MEMORY引擎缺点：不安全，关机之后数据消失。因为数据和索引都是在内存当中。**

#### **15.事务**

##### 1.什么是事务

​		一个事务其实就是一个完整的业务逻辑。是一个最小的工作单元。不可再分。

​		只有DML语句才会有事务这一说，其它语句和事务无关。insert，delete，update
​		只有以上的三个语句和事务有关系，其它都没有关系。因为 只有以上的三个语句是数据库表中数据进行增.删.改的。只要你的操		作一旦涉及到数据的增.删.改，那么就一定要考虑安全问题

​	说到本质上，一个事务其实就是多条DML语句同时成功，或者同时失败。事务就是批量的DML语句	同时成功，或者同时失败！

​	提交事务
​			清空事务性活动的日志文件，将数据全部彻底持久化到数据库表中。
​			提交事务标志着，事务的结束。并且是一种全部成功的结束。

​	回滚事务
​				将之前所有的DML操作全部撤销，并且清空事务性活动的日志文件
​				回滚事务标志着，事务的结束。并且是一种全部失败的结束。

##### 2.事务包括4个特性

​		原子性
​				说明事务是最小的工作单元。不可再分。

​		一致性
​				所有事务要求，在同一个事务当中，所有操作必须同时成功，或者同时失败，以保证数据的一致性。

​		隔离性
​				A事务和B事务之间具有一定的隔离。
​				教室A和教室B之间有一道墙，这道墙就是隔离性。
​				A事务在操作一张表的时候，另一个事务B也操作这张表会那样

​		持久性
​				事务最终结束的一个保障。事务提交，就相当于将没有保存到硬盘上的数据保存到硬盘上

##### 3.事务的隔离性

​		**读未提交：read uncommitted（最低的隔离级别）**

​				事务A可以读取到事务B未提交的数据。

​				这种隔离级别存在的问题就是：
​						脏读现象！(Dirty Read)，我们称读到了脏数据。
​				这种隔离级别一般都是理论上的，大多数的数据库隔离级别都是二档起步！

​		**读已提交：read committed**

​				事务A只能读取到事务B提交之后的数据。解决了脏读的现象

​				这种隔离级别存在什么问题
​					不可重复读取数据。

​				什么是不可重复读取数据
​						在事务开启之后，第一次读到的数据是3条，当前事务还没有结束，可能第二次再读取的时候，读到的数据是4条，3不等于						4称为不可重复读取。

​				这种隔离级别是比较真实的数据，每一次读到的数据是绝对的真实。
​					oracle数据库默认的隔离级别是：read committed

​		**可重复读：repeatable read**

​				提交之后也读不到，永远读取的都是刚开启事务时的数据。

​				解决了不可重复读取数据。

​			可重复读存在的问题是什么？
​				可以会出现幻影读。每一次读取到的数据都是幻象。不够真实！

​			mysql中默认的事务隔离级别

​		**序列化/串行化：serializable（最高的隔离级别）**

​				这是最高隔离级别，效率最低。解决了所有的问题。
​				这种隔离级别表示事务排队，不能并发！
​				synchronized，线程同步（事务同步）
​				每一次读取到的数据都是最真实的，并且效率是最低的。

##### 4.演示事务

-- 查看隔离级别

select @@tx_isolation;

-- 设置事务的隔离级别

 set global transaction isolation level read uncommitted;

#### 16.索引

##### 1.什么是索引

​	索引是在数据库表的字段上添加的，是为了提高查询效率存在的一种机制。
​	一张表的一个字段可以添加一个索引，当然，多个字段联合起来也可以添加索引。
​	索引相当于一本书的目录，是为了缩小扫描范围而存在的一种机制。

对于一本字典来说，查找某个汉字有两种方式：
	第一种方式：一页一页挨着找，直到找到为止，这种查找方式属于全字典扫描。效率比较低。
	第二种方式：先通过目录（索引）去定位一个大概的位置，然后直接定位到这个位置，做局域性扫描，缩小扫描的范围，快速的查找。  	这种查找方式属于通过索引检索，效率较高。

**什么条件下**，**我们会考虑给字段添加索引呢**

​		**在mysql当中，主键上，以及unique字段上都会自动添加索引的！！！！**

​	条件1：数据量庞大（到底有多么庞大算庞大，这个需要测试，因为每一个硬件环境不同）
​	条件2：该字段经常出现在where的后面，以条件的形式存在，也就是说这个字段总是被扫描。
​	条件3：该字段很少的DML(insert delete update)操作。(因为DML之后，索引需要重新排序。)

​	建议不要随意添加索引，因为索引也是需要维护的，太多的话反而会降低系统的性能。
​	建议通过主键查询，建议通过unique约束的字段进行查询，效率是比较高的。

##### 2.创建和删除索引

```mysql
--创建索引：
		create index emp_ename_index on emp(ename);
		--给emp表的ename字段添加索引，起名：emp_ename_index
--删除索引：
		drop index emp_ename_index on emp;
--将emp表上的emp_ename_index索引对象删除
```
##### 3.索引的实现原理

1：在任何数据库当中主键上都会自动添加索引对象，id字段上自动有索引，因为id是PK。另外在mysql当中，一个字段上如果有unique 		约束的话，也会自动创建索引对象。

2：在任何数据库当中，任何一张表的任何一条记录在硬盘存储上都有一个硬盘的物理存储编号。

3：在mysql当中，索引是一个单独的对象，不同的存储引擎以不同的形式存在，在MyISAM存储引擎中，索引存储在一个.MYI文件中。在		InnoDB存储引擎中索引存储在一个逻辑名称叫做tablespace的当中。在MEMORY存储引擎当中索引被存储在内存当中。不管索引存		储在哪里，索引在mysql当中都是一个树的形式存在。（自平衡二叉树：B-Tree）

##### 4.索引的分类

​	单一索引：给单个字段加索引

​	复合索引：给多个字段联合起来加一个索引

​	主键索引：主键上会自动添加索引

​	唯一索引：在unique约束的字段上会自动添加索引

##### 5.索引什么时候会失效

​	select * from emp where ename like '%T';

​	ename上即使添加了索引，也不会走索引，为什么？
​		原因是因为模糊匹配当中以“%”开头了！尽量避免模糊查询的时候以“%”开始，这是一种优化的手段/策略。

#### 17.视图（view）

1.什么是视图？
	view:站在不同的角度去看待同一份数据。

```mysql
--创建视图对象：
	create view dept2_view as select * from dept2;
--删除视图对象：
	drop view dept2_view;
--注意：只有DQL语句才能以view的形式创建。
create view view_name as --这里的语句必须是DQL语句;
```
用视图做什么？

我们可以面向视图对象进行增删改查，对视图对象的增删改查，会导致原表被操作（视图的特点：通过对视图的操作，会影响到原表数据。）

2.DBA常用命令

```mysql
数据导出？
	注意：在windows的dos命令窗口中：
mysqldump bjpowernode>D:\bjpowernode.sql -uroot -p123456
--可以导出指定的表吗？
mysqldump bjpowernode emp>D:\bjpowernode.sql -uroot p123456

--数据导入？
	--注意：需要先登录到mysql数据库服务器上。
	然后创建数据库：create . bjpowernode;
	使用数据库：use bjpowernode
	然后初始化数据库：source D:\bjpowernode.sql
```
#### 18.数据库设计三范式

​	第一范式：要求任何一张表必须有主键，每一个字段原子性不可再分。

​			最核心，最重要的范式，所有表的设计都需要满足。

​	第二范式：建立在第一范式的基础之上，要求所有非主键字段完全依赖主	键，不要产生部分依赖。

```mysql
	使用三张表来表示多对多的关系！！！！
	学生表
	学生编号(pk)			学生名字
	------------------------------------
	1001					张三
	1002					李四
	1003					王五
	
	教师表
	教师编号(pk)		教师姓名
	--------------------------------------
	001					王老师
	002					赵老师

	学生教师关系表
	id(pk)			学生编号(fk)			教师编号(fk)
	------------------------------------------------------
	1						1001						001
	2						1002						002
	3						1003						001
	4						1001						002
```

背口诀：
	多对多怎么设计？
		**多对多，三张表，关系表两个外键**

​	第三范式：建立在第二范式的基础之上，要求所有非主键字段直接依赖主	键，不要产生传递依赖。

```mysql
学生编号（PK） 学生姓名 班级编号        班级名称

---------------------------------------------------------

	1001		张三		01			一年一班
	1002		李四		02			一年二班
	1003		王五		03			一年三班
	1004		赵六		03			一年三班
```

以上表的设计是描述：班级和学生的关系。很显然是1对多关系！
一个教室中有多个学生。

​		分析以上表是否满足第一范式？
​			满足第一范式，有主键。

​		分析以上表是否满足第二范式？
​			满足第二范式，因为主键不是复合主键，没有产生部分依赖。主键是			单一主键。

​		分析以上表是否满足第三范式？
​			第三范式要求：不要产生传递依赖！
​				一年一班依赖01，01依赖1001，产生了传递依赖。不符合第三范				式的要求。产生了数据的冗余。

那么应该怎么设计一对多呢

```mysql
	班级表：一
	班级编号(pk)				班级名称
	----------------------------------------
	01								一年一班
	02								一年二班
	03								一年三班

	学生表：多
	学生编号（PK） 学生姓名 班级编号(fk)
	-------------------------------------------
	1001				张三			01			
	1002				李四			02			
	1003				王五			03			
	1004				赵六			03		
	
```
背口诀：
		**一对多，两张表，多的表加外键**

一对一的表设计

**口诀：一对一，外键唯一**

数据库设计三范式是理论上的。

​		实践和理论有的时候有偏差。

​		最终的目的都是为了满足客户的需求，有的时候会拿冗余换执行速度。

​		因为在sql当中，表和表之间连接次数越多，效率越低。（笛卡尔积）

​		有的时候可能会存在冗余，但是为了减少表的连接次数，这样做也是合理的，并且对于开发人员来说，sql语句的编写难度也会降低。

#### 19.编程题练习

```mysql
-- 1.取得每个部门最高薪水的人员名称
	-- 首先取得每个部门的最高薪水
	select deptno,MAX(sal) from emp group by deptno;
	-- 然后将上述的表作为一张临时表与emp表进行连接
	select e.ename,d.* from emp e RIGHT JOIN (select deptno,MAX(sal) as maxsal from emp group by deptno) d
	on d.deptno=e.DEPTNO and d.maxsal =e.sal;
-- 2.哪些人的薪水在部门的平均薪水之上
	-- 首先查出部门的平均薪水
	select deptno,AVG(sal) as avgsal from emp GROUP BY DEPTNO;
	select e.ename,e.sal,d.* from emp e  JOIN  (select deptno,AVG(sal) as avgsal from emp GROUP BY DEPTNO) d
	on e.DEPTNO =d.deptno and e.SAL >d.avgsal;
-- 3.取得部门中（所有人的）平均的薪水等级
	-- 首先取得部门说有人的平均薪水
	select DEPTNO,AVG(sal) from emp GROUP BY DEPTNO
	select d.*,s.GRADE from salgrade s JOIN (select DEPTNO,AVG(sal) as avgsal from emp GROUP BY DEPTNO) d on 	d.avgsal BETWEEN s.LOSAL and s.HISAL	
-- 4.不准用组函数（Max ），取得最高薪水
	-- 方法一降序，然后使用limit关键字
	select e.ename ,e.sal from emp e ORDER BY e.sal desc limit 1;
	-- 方法二：表的自连接，不是很理解？
   select sal from emp where sal not in(select distinct a.sal from emp a join emp b on a.sal < b.sal);
-- 5.取得平均薪水最高的部门的部门编号
	-- 首先得到每个部门的平均薪水
	select deptno,AVG(sal) from emp GROUP BY DEPTNO;
	-- 方法一：使用limit关键字，然后降序
	select deptno from emp group by DEPTNO ORDER BY AVG(sal) desc limit 1;
-- 方法二：使用max
	-- 	还是首先找出每个部门的平均薪资
	select deptno,avg(sal) from emp group by DEPTNO;
	-- 找出平均薪资里面的最大值
	select MAX(t.avgsal) from (select deptno,avg(sal) as avgsal from emp group by DEPTNO) t;
	-- 第三步：
	select 
		deptno,avg(sal) as avgsal 
	from 
		emp 
	group by 
		deptno
	having
		avgsal = (select max(t.avgsal) from (select avg(sal) as avgsal from emp group by deptno) t);
-- 6.取得平均薪水最高的部门的部门名称
-- 首先取得每个部门的平均薪水
  select deptno,AVG(sal) as avgsal from emp GROUP BY DEPTNO;
select d.dname, t.avgsal from dept d LEFT JOIN 
(select deptno,AVG(sal) as avgsal from emp GROUP BY DEPTNO) t on t.deptno=d.DEPTNO limit 1;	
-- 7.求平均薪水的等级最低的部门的部门名称
-- 还是先求每个部门的平均薪水
select deptno,avg(sal) from emp GROUP BY DEPTNO;
-- 找出每个部门的平均薪水等级
select s.GRADE,t.* from (select deptno,avg(sal) from emp GROUP BY DEPTNO) t JOIN salgrade s ON
t.avgsal BETWEEN s.LOSAL and s.HISAL ;

select 
	t.*,s.grade
from
	(select d.dname,avg(sal) as avgsal from emp e join dept d on e.deptno = d.deptno group by d.dname) t
join
	salgrade s
on
	t.avgsal between s.losal and s.hisal
where s.grade = (select grade from salgrade where (select avg(sal) as avgsal from emp group by deptno
ORDER BY avgsal asc limit 1) BETWEEN losal and hisal);
-- 方法二，求平均薪水的等级最低的部门的部门名称，即使，平均薪水最低的部门的薪水等级一定是最低的
-- 还是先求每个部门的平均薪水
select avg(sal) as avgsal from emp group by deptno order by avgsal asc limit 1;
select grade from salgrade where (select avg(sal) as avgsal from emp group by deptno order by avgsal asc limit 1) between losal and hisal;
-- 8.取得比普通员工(员工代码没有在mgr字段上出现的)的最高薪水还要高的领导人姓名
-- 找出所有的领导编号
select distinct MGR from emp;
-- 找出工资最高的普通员工
select max(sal) from emp where EMPNO not in (select distinct MGR from emp where mgr is not null);
-- 找出比普通员工工资还高的领导
select ename,sal from emp where sal>(select max(sal) from emp where EMPNO not in (select distinct MGR from emp where mgr is not null));
-- 9.取得薪水最高的前五名员工
select ename,sal from emp ORDER BY sal desc limit 5; 
-- 10.取得薪水最高的第六到第十名员工
select ename,sal from emp ORDER BY sal limit 5,5;
-- 11.取得每个薪水等级有多少员工
-- 首先找出每个员工的薪水等级
select e.ename,e.sal,s.grade from emp e JOIN salgrade s on e.SAL BETWEEN s.losal and s.hisal;
-- 第二步：继续按照grade分组统计数量
select s.grade,count(*) from emp e join salgrade s ON e.sal BETWEEN s.LOSAL and s.HISAL GROUP BY s.grade; 
-- 14.列出所有员工及领导的姓名
select 
	a.ename '员工', b.ename '领导'
from
	emp a
left join
	emp b
on
	a.mgr = b.empno;
-- 14.列出受雇日期早于其直接上级的所有员工的编号,姓名,部门名称
select * from emp;
select a.ename '员工', a.HIREDATE,b.ename '领导' ,b.HIREDATE from emp a LEFT JOIN emp b on a.MGR=b.EMPNO and a.HIREDATE <b.HIREDATE;

```

