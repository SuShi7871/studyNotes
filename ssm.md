## 一.MyBatis

   MyBatis下载地址：https://github.com/mybatis/mybatis-3

​	Mybatis的优缺点：   

- MyBatis 轻量级，性能出色 

- SQL 和 Java 编码分开，功能边界清晰。Java代码专注业务.SQL语句专注数据开发效率稍逊于HIbernate，但是完全能够接受


​	JDBC 优缺点

- JDBC SQL 夹杂在Java代码中耦合度高，导致硬编码内伤 

- 维护不易且实际开发需求中 SQL 有变化，频繁修改的情况多见 

- 代码冗长，开发效率低


###   1.mybatis搭建

​	1.MySQL不同版本的注意事项 

​		驱动类driver-class-name 

​		 MySQL 5版本使用jdbc5驱动，驱动类使用：com.mysql.jdbc.Driver

​		 MySQL 8版本使用jdbc8驱动，驱动类使用：com.mysql.cj.jdbc.Driver

​		**高版本的驱动可以兼容低版本的驱动，反之不对；**

   2.mybatis-config.xml是创建MyBatis的核心配置文件

```xml
<configuration>
<!--设置连接数据库的环境-->
<!--environments：配置多个连接数据库的环境属性：
	default：设置默认使用的环境的id-->
<environments default="development">
<!--environment：配置某个具体的环境属性：id：表示连接数据库环境的唯一标识，不能重复-->
	<environment id="development">
<!--transactionManager：设置事务管理方式属性：
	type="JDBC|MANAGED"
	JDBC：表示当前环境中，执行SQL时，使用的是JDBC中原生的事务管理方式，事务的提交或回滚需要手动处理
	MANAGED：被管理，例如Spring
-->
	<transactionManagertype="JDBC"/>
<!--	dataSource：配置数据源属性：
		type：设置数据源的类型
		type="POOLED|UNPOOLED|JNDI"
		POOLED：表示使用数据库连接池缓存数据库连接
		UNPOOLED：表示不使用数据库连接池
		JNDI：表示使用上下文中的数据源
-->
	<dataSourcetype="POOLED">
	<propertyname="driver"value="com.mysql.cj.jdbc.Driver"/>
	<propertyname="url"value="jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC"/>
	<propertyname="username"value="root"/>
	<propertyname="password"value="123456"/>
</dataSource>
</environment>
</environments>
<!--引入映射文件-->
<mappers>
	<packagename="mappers/UserMapper.xml"/>
</mappers>
</configuration>
```

  **创建MyBatis的映射文件**

​	表所对应的实体类的类名+Mapper.xml 

​	例如：表t_user，映射的实体类为User，所对应的映射文UserMapper.xml 因此一个映射文件对应一个实体类，对应一张表的操作,MyBatis映射文件用于编写	  SQL，访问以及操作表中的数据 MyBatis映射文件存放的位置是src/main/resources/mappers目录下

​	MyBatis中可以面向接口操作数据，要保证两个一致：

​			mapper接口的**全类名**和**映射文件的命名空间(namespace)**保持一致
​			mapper接口中方法的**方法名**和映射文件中编写SQL的**标签的id属性**保持一致

```xml
//mapper接口的全类名和映射文件的命名空间（namespace）保持一致
<mapper namespace="com.zhang.mybatis.mapper.UserMapper">
<!--int insertUser();-->
	<insert id="insertUser">
	insert into t_user 	values(null,'admin','123456',23,'男','12345@qq.com')
	</insert>
</mapper>
```

测试插入功能

```java
//获取核心配置文件的输入流
InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
//获取sqlsessionfactory对象
SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
//获取sqlsessionfatory对象
SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
//获取sql的会话对象sqlsession，是mybatis提供的操作数据库的对象
//true自动提交事务
SqlSession sqlSession = sqlSessionFactory.openSession(true);
//获取usermapper的代理实现类对象
UserMapper mapper = sqlSession.getMapper(UserMapper.class);
//调用mapper的方法实现添加用户信息的功能
int result = mapper.insertUser();
System.out.println("结果："+result);
//关闭会话
sqlSession.close();
```

测试查询功能

​		resultType:设置结果类型，即查询到的数据要转换为java类型

​		resultMap:自定义映射，处理一对多或者多对一的映射关系

```xml
<select id="queryUser" resultType="com.zhang.pojo.User">
    select * from t_user where id=#{id}
</select>
```

mybatis核心配置模块详解

```xml
<!--
    MyBatis核心配置文件中的标签必须要按照指定的顺序配置：
    properties?,settings?,typeAliases?,typeHandlers?,
    objectFactory?,objectWrapperFactory?,reflectorFactory?,
    plugins?,environments?,databaseIdProvider?,mappers?
-->
<!--引入properties文件，此后就可以在当前文件中使用${key}的方式访问value-->
<properties resource="jdbc.properties"></properties>
<!--设置连接数据库的环境-->
<environments default="development">
    <environment id="development">
     <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <propertyname="username"value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
      </dataSource>
    </environment>
</environments>
```

```xml
<!--jdbc.properties配置文件用来连接数据库-->
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC
jdbc.username=root
jdbc.password=root
```

typeAliases：设置类型别名，即为某个具体的类型设置一个别名在MyBatis的范围中，就可以使用别名表示一个具体的类型。

```xml
<typeAliases>
        <!--type：设置需要起别名的类型
            alias：设置某个类型的别名
        -->
<typeAlias type="com.zhang.mybatis.pojo.User" alias="abc"></typeAlias>
<!--若不设置alias，当前的类型拥有默认的别名，即类名且不区分大小写-->
<typeAlias type="com.zhang.mybatis.pojo.User"></typeAlias>
<!--通过包设置类型别名，指定包下所有的类型将全部拥有默认的别名，即类名且不区分大小写-->
<package name="com.zhang.mybatis.pojo"/></typeAliases>
```

mybatis-config.xml配置文件中的mybatis的映射文件的配置

```xml
<!--引入mybatis的映射文件-->
<mappers>
        <mapper resource="mappers/UserMapper.xml"/>
        <!--
            以包的方式引入映射文件，但是必须满足两个条件：
            mapper接口和映射文件所在的包必须一致
            mapper接口的名字和映射文件的名字必须一致
        -->
 <!-- 此处需要在resource目录下建立对应的包用来存放mapper.xml文件
    建立包的方式为com/zhang/mapper必须与main下面的java下的路径保持一致-->
        <package name="com.zhang.mybatis.mapper"/>
 </mappers>
```

### 2.获取参数

MyBatis获取参数值的两种方式：**${}和#{}**

${}的本质就是字符串拼接，**#{}的本质就是占位符赋值** 

**${}使用字符串拼接**的方式拼接sql，若为字符串类型或日期类型的字段进行赋值时，需要**手动加单引号**；

但是**#{}使用占位符**赋值的方式拼接sql，此时为字符串类型或日期类型的字段进行赋值时， **可以自动添加单引号**；

```xml
<mapper namespace="com.zhang.mapper.UserMapper">
    <select id="getUserByUsername" resultType="user" >
    <!--两种获取单个参数的方式-->
        select * from t_user where username=#{username}
		select * from t_user where username='${username}'
    </select>
</mapper>
```

**注意：使用注解@Test时方法不能带参数，返回值为void**

```java
SqlSession sqlSession = SqlSessionUtil.getSqlSession();
UserMapper mapper = sqlSession.getMapper(UserMapper.class);
//通过用户名来获取用户信息,错误写法
@Test
public User testGetUserByUsername(String username){  
    User userByUsername = mapper.getUserByUsername("root");
    System.out.println(userByUsername);
}
//报如下错误
java.lang.Exception: Method testGetUserByUsername should have no parameters
//正确的写法为
@Test
public void testGetUserByUsername(){
    User userByUsername = mapper.getUserByUsername("root");
    System.out.println(userByUsername);
}
```

2.若mapper接口的参数为**多个的字面量类型**，此时mybatis会将多个参数放在map集合中，以两种方式存储数据

- ​	以arg0，arg1...为键，以参数为值；

- ​	以param1，param2...为键，以参数为值；


```java
<select id="checkLogin" resultType="user">
    //方式一
   select * from t_user where username='${arg0}' and pasword='${arg1}'
     //方式二
    select * from t_user where username='${param1}' and pasword='${param2}'
</select>
```

若mapper接口的参数为map**集合类型的参数**，则只需要通过#{}和${}访问map集合的键，就可以获取到相应的值。

```java
//usermapper.xml文件里的配置
<select id="checkLoginByMap" resultType="user">
    select * from t_user where username=#{username} and password =#{password}
</select>
//测试文件    
@Test
    public void testCheckLoginByMap(){
   		SqlSession sqlSession = SqlSessionUtil.getSqlSession();
 		UserMapper mapper = sqlSession.getMapper(UserMapper.class);
      	Map<String,Object> map =new HashMap<String, Object>();
        map.put("username","root");
        map.put("password","root");
        User user = mapper.checkLoginByMap(map);
        System.out.println(user);
    }    
```

4.若mapper接口的参数为**实体类类型的参数**，则只需要通过#{}和${}访问实体类中的属性名，就可以获得相应的属性值。

```java
<insert id="insertUser" >
    insert into t_user values (3,#{username},#{password},#{age},#{gender},#{email})
</insert>
 @Test
 public void testIsertUser(){
  		SqlSession sqlSession = SqlSessionUtil.getSqlSession();
 		UserMapper mapper = sqlSession.getMapper(UserMapper.class);
		User user = new User(3,"zhangsan","123456",20,"女","123456@qq.com");
        mapper.insertUser(user);
  }   
 
```

5.可以在mapper接口的方法参数上设置**@param注解**，此时mybatis会将这些参数放在map中，以两种方式进行存储。

- 以@param注解的value属性值为键，以参数为值；

- 以param1，param2...为键，以参数为值;


然后通过#{}和${}访问

```java
//usermapper中
User checkLoginByParam(@Param("username") String username,@Param("password") String password);
//usermapper.xml中
<select id="checkLoginByParam" resultType="user">
 	select  * from t_user where username=#{username} and password=#{password}
</select>
```

### 3.各种查询功能

查询表中的数据总条数

```java
//此处的resultType返回的是int型，此处也可以写成int，INT，Integer等，这些都是integer的类型别名，mybatis可以自动识别出来
<select id="getCount" resultType="java.lang.Integer">
    select count(*) from t_user;
</select>
 //根据id获取一条用户信息，并将其放在map集合中   
<select id="getUserByIdToMap" resultType="map">
     select * from t_user where id=#{id}
</select>   
  	//查询多条数据以map集合的形式表示，
    //解决方法一，将mapper接口方法的返回值设置为泛型是map的list集合
    //方法二是将每条数据获得的map集合放在一个大的map中，但是必须要通过@mapkey注解，将查询的某个字段的值作为大的map的键  
    //方法一
    //selectUserMapper
   List<Map<String,Object>> getAllUserToMap();  
	//selectUserMapper.xml
    <select id="getAllUserToMap" resultType="map">
        select  * from t_user 
    </select>
   //方法二，使用mapkey注解
    @MapKey("id")
    Map<String,Object> getAllUserToMap();
	<select id="getAllUserToMap" resultType="map">
        select  * from t_user 
    </select>
```

### 4.特殊SQL

#### 1.模糊查询

```java
//查询username里面包含a的用户的信息
List<User> getUsernameInA(@Param("mohu") String mohu);
<select id="getUsernameInA" resultType="User">
    //注意此处不能使用#{mohu}
     select * from t_user where username like '%${mohu}%'
</select>
//方式二，使用字符串拼接
 <select id="getUsernameInA" resultType="User">
     select * from t_user where username like concat('%',#{mohu},'%')   
 </select>
//方式三，使用""进行连接 
   <select id="getUsernameInA" resultType="User">
     select * from t_user where username like "%"#{mohu}"%")
 </select>  
```

#### 2.批量删除

```java
void deleteMoreUsers(@Param("ids") String ids);
<delete id="deleteMoreUsers">
    //注意此处还是不能使用#{}
    delete from t_user where id in(${ids})
</delete>
    SqlSession sqlSession = SqlSessionUtil.getSqlSession();
    SpecialSolMapper mapper = sqlSession.getMapper(SpecialSolMapper.class);
    mapper.deleteMoreUsers("5,6");
```

#### 3.动态设置表名

```xml
List<User> getUserList(@Param("tableName") String tableName);
<!--动态设置表名的时候只能用${}，而不能用#{}-->
<select id="getUserList" resultType="user">
   select * from ${tableName}
</select>
```

#### 4.获取自增主键

```xml
/*添加用户信息，并获得自增的主键*/
void insertUser(User user);
<!--useGeneratedKeys表示当前添加功能使用自增的主键
    keyProperty：将添加的数据的自增主键为实体类类型的参数的属性赋值
-->
 <insert id="insertUser" useGeneratedKeys="true" keyProperty="id">
     insert into t_user values(5,#{username},#{password},#{age},#{gender},#{email})
 </insert>
```

### 5.resultMap

#### 1.字段和属性映射关系

   若**字段名和实体类中的属性名不一致**，则可以通过resultMap设置自定义映射

```java
//解决方式1，给字段起别名，解决字段名和实体类中的属性名不一致
<select id="getEmpByEmpId" resultType="emp">
    select emp_id empId,emp_name empName,age,gender from t_emp where emp_id=#{empId}
</select>
```

   **字段名和属性名不一致**的情况，如何处理映射关系
        1、为查询的字段设置别名，和属性名保持一致
       2、 当字段符合MySQL的要求使用_，而属性符合java的要求使用驼峰此时可以在MyBatis的核心配置文件中设置一个全局配置，可以**自动将下划线映射为驼峰** 

```java
<!--seetings配置在properties下面-->
<settings>
    <!--将下划线映射为驼峰-->
    <setting name="mapUnderscoreToCamelCase" value="true"/>
</settings>
```

**org.apache.ibatis.executor.ExecutorException: No constructor found in poj 解决方法**

​	**在实体类里添加一个无参构造，因为mybatis在封装返回值的时候需要无参构造。**

使用resultMap处理字段和属性的映射关系不一致的情况

```xml
<!--	resultMap：设置自定义的映射关系
        id：唯一标识，对应的是主键
        type：处理映射关系的实体类的类型
        常用的标签：
        id：处理主键和实体类中属性的映射关系
        result：处理普通字段和实体类中属性的映射关系
        association：处理多对一的映射关系（处理实体类类型的属性）
        collection：处理一对多的映射关系（处理集合类型的属性）
        column：设置映射关系中的字段名，必须是sql查询出的某个字段
        property：设置映射关系中的属性的属性名，必须是处理的实体类类型中的属性名
    -->
<resultMap id="empResultMap" type="Emp">
    <id column="emp_id" property="empId"></id>
    <result column="emp_name" property="empName"></result>
    <result column="age" property="age"></result>
    <result column="gender" property="gender"></result>
</resultMap>
<select id="getEmpByEmpId" resultMap="empResultMap">
    select * from t_emp where emp_id=#{empId}
</select>
```

#### 2.多对一映射关系

多个员工对应一个部门

#####  1.级联方式处理

```xml
<resultMap id="empAndDeptResultMap" type="Emp">
    <id column="emp_id" property="empId"></id>
    <result column="emp_name" property="empName"></result>
    <result column="age" property="age"></result>
    <result column="gender" property="gender"></result>
    <result column="dept_id" property="dept.deptId"></result>
    <result column="dept_name" property="dept.deptName"></result>
</resultMap>
<select id="getEmpAndDeptByEmpId" resultMap="empAndDeptResultMap">
     select t_emp.*,t_dept.* from t_emp left join  t_dept on t_emp.dept_id=t_dept.dept_id where t_emp.emp_id=#{empId}
</select>
```

#####  2.association

​	 association：处理多对一的映射关系（处理实体类类型的属性）

```xml
<resultMap id="empAndDeptResultMap" type="Emp">
    <id column="emp_id" property="empId"></id>
    <result column="emp_name" property="empName"></result>
    <result column="age" property="age"></result>
    <result column="gender" property="gender"></result>
    <!--property：设置需要处理映射关系的属性的属性名
    	javaType:设置需要处理的属性的类型
    -->
    <association property="dept" javaType="Dept">
        <id column="dept_id" property="deptId"></id>
        <result column="dept_name" property="deptName"></result>
    </association>
</resultMap>
<select id="getEmpAndDeptByEmpId" resultMap="empAndDeptResultMap">
     select t_emp.*,t_dept.* from t_emp left join  t_dept on t_emp.dept_id=t_dept.dept_id where t_emp.emp_id=#{empId}
</select>
```

#####  3.分步查询

```xml
<resultMap id="empAndDeptByStepResultMap" type="Emp">
    <id column="emp_id" property="empId"></id>
    <result column="emp_name" property="empName"></result>
    <result column="age" property="age"></result>
    <result column="gender" property="gender"></result>
    <!--
    property :设置需要处理映射关系的属性的属性名
    select:设置分布查询的sql的唯一标识
    column:将查询出的某个字段最为分布查询的sql的条件
    -->
    <association property="dept" select="com.zhang.mapper.DeptMapper.getEmpAndDeptByStepTwo"
                 column="deptId">
    </association>
</resultMap>
<select id="getEmpAndDeptByStep" resultMap="empAndDeptByStepResultMap">
    select * from t_emp where emp_id=#{empId}
</select>
<select id="getEmpAndDeptByStepTwo" resultType="Dept">
    select * from t_dept where dept_id=#{deptId}
</select>
```

##### 4.延迟加载

分步查询的优点：可以实现延迟加载 但是必须在核心配置文件中设置全局配置信息：

-  lazyLoadingEnabled：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载 

- aggressiveLazyLoading：当开启时，任何方法的调用都会加载该对象的所有属性。否则，每个属性会按需加载 


此时就可以实现按需加载，获取的数据是什么，就只会执行相应的sql。此时可通过association和 collection中的fetchType属性设置当前的分步查询是否使用延迟加载，fetchType="lazy(延迟加载)|eager(立即加载)"

```xml
<!--开启延迟加载-->
<setting name="lazyLoadingEnabled" value="true"/>
<!--按需加载-->
<setting name="aggressiveLazyLoading" value="false"/>
    <!--对局部的配置文件进行设置是否延迟加载-->
<resultMap id="empAndDeptByStepResultMap" type="Emp" >
        <id column="emp_id" property="empId"></id>
        <result column="emp_name" property="empName"></result>
        <result column="age" property="age"></result>
        <result column="gender" property="gender"></result>
        <!--fetchType:在开启了延迟加载的环境中，通过该属性设置当前的分布查询是否使用延迟加载fetchType=eager（立即加载) lazy（延迟加载）-->
        <association 
                     property="dept" 
                     fetchType="eager" 	
                     select="com.zhang.mapper.DeptMapper.getEmpAndDeptByStepTwo"
        			 column="deptId">
        </association>
 </resultMap>    
```

##### 5.一对多映射处理

**collection**方式处理

一个部门对应多个员工

```xml
<resultMap id="deptAndEmpResultMap" type="dept">
    <id column="dept_id" property="deptId"></id>
    <result column="dept_name" property="deptName"></result>
    <!--ofType:设置集合类型的属性中存储数据的类型-->
    <collection property="emps" ofType="Emp">
        <id column="emp_id" property="empId" ></id>
        <result column="emp_name" property="empName"></result>
        <result column="age" property="age"></result>
        <result column="gender" property="gender"></result>
    </collection>
</resultMap>
<!--查询部门信息以及部门中的员工-->
<select id="getDeptAndEmpByDeptId" resultMap="deptAndEmpResultMap">
    select * from t_dept left join t_emp on t_dept.dept_id=t_emp.dept_id where t_dept.dept_id = #{deptId}
</select>
```

分布方式处理

```xml
//deptMapper
Dept getDeptAndEmpByDeptSteptOne(@Param("deptId") Integer deptId);
//deptMapper.xml
<resultMap id="deptAndEmpResultMapByStep" type="dept">
    <id column="dept_id" property="deptId"></id>
    <result column="dept_name" property="deptName"></result>
    <collection property="emps"
     select="com.zhang.mapper.EmpMapper.getDeptAndEmpByStepTwo" 
    column="dept_id"
    ></collection>
</resultMap>
<select id="getDeptAndEmpByDeptId" resultMap="">
    select * from t_dept where dept_id=#{deptId}
</select>
  //empMapper
    /*通过分布查询，查询部门中员工信息的第二部*/
List<Emp> getDeptAndEmpByStepTwo(@Param("deptId") Integer deptId);
//empMapper.xml
<select id="getDeptAndEmpByStepTwo" resultType="Emp">
  select * from t_emp where dept_id=#{deptId}
</select>
```

### 6.动态SQL

​		Mybatis框架的动态SQL技术是一种根据特定条件动态拼装SQL语句的功能，它存在的意义是为了解决拼接SQL语句字符串时的痛点问题。

#### 1.if标签

if标签可通过test属性的表达式进行判断，若表达式的结果为true，则标签中的内容会执行；反之标签中的内容不会执行

```xml
<select id="getEmpByCondition" resultType="Emp">
    select * from t_emp where
    <if test="empName !=null and empName != ''">
        emp_name=#{empName}
    </if>
    <if test="age !=null and age !=''">
        and age=#{age}
    </if>
    <if test="gender !=null and gender !=''">
        and gender=#{gender}
    </if>
</select>
```

#### 2.where标签

where和if一般结合使用： 

- ​	若where标签中的if条件都不满足，则where标签没有任何功能，即不会添加where关键字 

- ​	若where标签中的if条件满足，则where标签会自动添加where关键字，并将**条件最前方多余的 and去掉** 


注意：where标签不能去掉条件**最后多余的and**

```xml
<select id="getEmpByCondition" resultType="Emp">
    select * from t_emp
    <where>
        <if test="empName !=null and empName != ''">
            emp_name=#{empName}
        </if>
        <if test="age !=null and age !=''">
            and age=#{age}
        </if>
        <if test="gender !=null and gender !=''">
            and gender=#{gender}
        </if>
    </where>
</select>
```

#### 3.trim标签

trim用于去掉或添加标签中的内容 

常用属性：

​			 **prefix：在trim标签中的内容的前面添加某些内容**

​			  **prefixOverrides：在trim标签中的内容的前面去掉某些内容**

​			  **suffix：在trim标签中的内容的后面添加某些内容** 

​		      **suffixOverrides：在trim标签中的内容的后面去掉某些内容**

```xml
<select id="getEmpByCondition" resultType="Emp">
    select * from t_emp
    <trim prefix="where" suffixOverrides="and">
        <if test="empName !=null and empName != ''">
            emp_name=#{empName}
        </if>
        <if test="age !=null and age !=''">
            and age=#{age}
        </if>
        <if test="gender !=null and gender !=''">
            and gender=#{gender}
        </if>
    </trim>
</select>
```

#### 4.choose.when.otherwise

 choose.when. otherwise相当于if...else if..else

```xml
<select id="getEmpByChoose" resultType="emp">
     select * from t_emp
     <where>
         <choose>
             <when test="empName !=null and empName !=''">
                 emp_name=empName
             </when>
             <when test="age !=null and age !=''">
                 age=#{age}
             </when>
             <when test="gender != null and gender !=''" >
                 gender=#{gender}
             </when>
         </choose>
     </where>
</select>
```

#### 5.foreach标签

```xml
//批量添加数据
<insert id="insertMoreEmp">
    insert into t_emp values
    <foreach collection="emps" item="emp" separator=",">
        (null,#{emp.empName},#{emp.age},#{emp.gender},null)
    </foreach>
</insert>
```

批量删除

```xml
<!--deleteMoreEmps-->
<delete id="deleteMoreEmps">
    delete from t_emp where emp_id in
    (
    <foreach collection="empIds" item="empId" separator=",">
        #{empId}
    </foreach>
    )

</delete>
 <!--方式二-->
 <delete id="deleteMoreEmps">        
     delete from t_emp where emp_id in
     /*open代表以什么开始；close代表以什么结束，separator代表以什么分割*/
     <foreach collection="empIds" item="empId" separator="," open="(" close =")">
         #{empId}
     </foreach>       
 </delete>
 <!--方式三-->      
<delete id="deleteMoreEmps">
      delete from t_emp where
     <foreach collection="empIds" item="empId" separator="or">
         emp_id=#{empId}
     </foreach>
</delete>       
```

#### 6.SQL片段

sql片段，可以记录一段公共sql片段，在使用的地方通过include标签进行引入

```xml
<sql id="empColumns">
    eid,ename,age,sex,did
</sql>
select <include refid="empColumns"></include> from t_emp
```

### 7.mybatis的缓存

#### 1.MyBatis的一级缓存

 一级缓存是SqlSession级别的，通过同一个SqlSession查询的数据会被缓存，下次查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问，mybatis是默认开启一级缓存

 使一级缓存失效的四种情况：

- 不同的SqlSession对应不同的一级缓存
- 同一个SqlSession但是查询条件不同
- 同一个SqlSession两次查询期间执行了任何一次增删改操作
- 同一个SqlSession两次查询期间手动清空了缓存

```java
SqlSession.clearCache();//清空缓存手动
```

### 8.MyBatis的逆向工程

#### 1.创建逆向工程

添加依赖和插件

```xml
 <!-- 插件的依赖 -->
 <dependencies>
     <!-- 逆向工程的核心依赖 -->
     <dependency>
         <groupId>org.mybatis.generator</groupId>
         <artifactId>mybatis-generator-core</artifactId>
         <version>1.3.2</version>
      </dependency>
      <!-- MySQL驱动 -->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>8.0.16</version>
      </dependency>
 </dependencies>
<!--在maven中构建相关配置-->
<build>
    <!-- 构建过程中用到的插件 -->
    <plugins>
        <!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.2</version>
        </plugin>
    </plugins>
</build>
```

创建MyBatis的核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--引入properties文件，此后就可以在当前文件中使用${key}的方式访问value-->
    <properties resource="jdbc.properties"></properties>
    <settings>
        <!--将下划线映射为驼峰-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <!--开启延迟加载-->
        <setting name="lazyLoadingEnabled" value="true"/>
        <!--按需加载-->
        <setting name="aggressiveLazyLoading" value="true"/>
    </settings>
    <typeAliases >
        <!--实体类所对应的包，用来给包起别名-->
        <package name="com.zhang.pojo"></package>
    </typeAliases>
    <!--设置连接数据库的环境-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--引入映射文件-->
    <mappers>
        <package name="com.zhang.mapper"/>
    </mappers>
</configuration>
```

创建逆向工程的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!--
    targetRuntime: 执行生成的逆向工程的版本
    MyBatis3Simple: 生成基本的CRUD（清新简洁版）
    MyBatis3: 生成带条件的CRUD（奢华尊享版）
    -->
    <context id="DB2Tables" targetRuntime="MyBatis3Simple">
        <!-- 数据库的连接信息 -->
        <jdbcConnection 
        			driverClass="com.mysql.cj.jdbc.Driver"
         			connectionURL="jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC"
                    userId="root"
                    password="root">
        </jdbcConnection>
        <!-- javaBean的生成策略-->
        <javaModelGenerator targetPackage="com.zhang.pojo"
                            targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- SQL映射文件的生成策略 -->
        <sqlMapGenerator targetPackage="com.zhang.mapper"
                         targetProject=".\src\main\resources">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>
        <!-- Mapper接口的生成策略 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.zhang.mapper" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>
        <!-- 逆向分析的表 -->
        <!-- tableName设置为*号，可以对应所有表，此时不写domainObjectName -->
        <!-- domainObjectName属性指定生成出来的实体类的类名 -->
        <table tableName="t_emp" domainObjectName="Emp"/>
        <table tableName="t_dept" domainObjectName="Dept"/>
    </context>
</generatorConfiguration>
```

4.执行MBG插件的generate目标，创建完成

### 9.分页插件（跳过）

## 二.spring

### 1.Spring概述

官网地址：https://spring.io

​			Spring 是最受欢迎的企业级 Java 应用程序开发框架，数以百万的来自世界各地的开发人员使用 Spring 框架来创建性能好.易于测试.可重用的代码。 Spring 框架是一个开源的 Java 平台，它最初是由 Rod Johnson 编写的，并且于 2003 年 6 月首次在Apache 2.0 许可下发布。Spring 是轻量级的框架，其基础版本只有 2 MB 左右的大小。 Spring 框架的核心特性是可以用于开发任何 Java应用程序，但是在 Java EE 平台上构建 web 应用程序是需要扩展的。 Spring 框架的目标是使 J2EE 开发变得更容易使用，通过启用基于 POJO 编程模型来促进良好的编程实践。

2.SpringFramework

项目列表：https://spring.io/projects

Spring Framework特性 

非侵入式：使用 Spring Framework 开发应用程序时，Spring 对应用程序本身的结构影响非常小。对领域模型可以做到零污染；对功能性组件也只需要使用几个简单的注解进行标记，完全不会 破坏原有结构，反而能将组件结构进一步简化。这就使得基于 Spring Framework 开发应用程序 时结构清晰.简洁优雅。 

控制反转：IOC——Inversion of Control，翻转资源获取方向。把自己创建资源.向环境索取资源变成环境将资源准备好，我们享受资源注入。 

面向切面编程：AOP——Aspect Oriented Programming，在不修改源代码的基础上增强代码功能。 

容器：Spring IOC 是一个容器，因为它包含并且管理组件对象的生命周期。组件享受到了容器化的管理，替程序员屏蔽了组件创建过程中的大量细节，极大的降低了使用门槛，大幅度提高了开发 效率。 

组件化：Spring 实现了使用简单的组件配置组合成一个复杂的应用。在 Spring 中可以使用 XML 和 Java 注解组合这些对象。这使得我们可以基于一个个功能明确.边界清晰的组件有条不紊的搭建超大型复杂应用系统。

声明式：很多以前需要编写代码才能实现的功能，现在只需要声明需求即可由框架代为实现。 

一站式：在 IOC 和 AOP 的基础上可以整合各种企业应用的开源框架和优秀的第三方类库。而且 Spring 旗下的项目已经覆盖了广泛领域，很多方面的功能性需求可以在 Spring Framework的基础上全部使用 Spring 来实现。

功能模块 																		功能介绍 

| Core Container          | 核心容器，在 Spring 环境下使用任何功能都必须基于 IOC 容器。 |
| :---------------------- | ----------------------------------------------------------- |
| AOP&Aspects             | 面向切面编程                                                |
| Testing                 | 提供了对 junit 或 TestNG 测试框	架的整合。               |
| Data Access/Integration | 提供了对数据访问/集成的功能。                               |
| Spring MVC              | 提供了面向Web应用程序的集成功能。                           |

### 2.IOC思想

#### 1.IOC和DI

IOC：Inversion of Control，翻译过来是反转控制。

DI：Dependency Injection，翻译过来是依赖注入。

 DI 是 IOC 的另一种表述方式：即组件以一些预先定义好的方式（例如：setter 方法）接受来自于容器的资源注入。相对于IOC而言,这种表述更直接

所以结论是：**IOC 就是一种反转控制的思想，而 DI 是对 IOC 的一种具体实现。**

#### 2.基于xml管理bean

##### 1.创建步骤

引入依赖

```xml
<!-- 基于Maven依赖传递性，导入spring-context依赖即可导入当前所需所有jar包 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.1</version>
</dependency>
```

创建配置文件applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--
bean:配置一个bean对象，将对象交给IOC容器管理
id：bean的唯一表示，不能重复
class：设置bean对象所对应的类型
-->
    <bean id="helloWorld" class="com.zhang.pojo.HelloWorld"></bean>
</beans>
```

使用bean

```java
@Test
public void testHello(){
    //获取ioc容器
    ApplicationContext ioc=new ClassPathXmlApplicationContext("applicationContext.xml");
    //获取ioc容器中的bean
    HelloWorld helloWorld = (HelloWorld) ioc.getBean("helloWorld");
    helloWorld.sayHello();
}
```

##### 2.获取bean 的三种方式

方式一：根据id获取 

由于 id 属性指定了 bean 的唯一标识，所以根据 bean 标签的 id 属性可以精确获取到一个组件对象。 

 方式二：根据类型获取 

```java
//根据类型获取 
Student Student = ioc.getBean(Student.class);
System.out.println(Student);
```

**当根据类型获取bean时，要求IOC容器中指定类型的bean有且只能有一个**

③方式三：根据id和类型来获取bean

```java
Student studentOne = ioc.getBean("studentOne", Student.class);
System.out.println(studentOne);
```

如果组件类实现了接口，根据接口类型可以获取 bean 吗？

 可以，前提是bean唯一 

如果一个接口有多个实现类，这些实现类都配置了 bean，根据接口类型可以获取 bean 吗？ 

不行，因为bean不唯一

**根据类型来获取bean时，在满足bean唯一性的前提下，其实只是看：『对象 instanceof 指定的类型』的返回结果，只要返回的是true就可以认定为和类型匹配，能够获取到。**

##### 3.依赖注入之setter注入

```xml
<!--
    property：通过成员变量的set方法来赋值。
    name：设置需要赋值的属性名。
    value：指定属性值
-->
<bean id="studentTwo" class="com.zhang.pojo.Student">
    <property name="sid" value="10002"></property>
    <property name="sname" value="张三"></property>
    <property name="age" value="23"></property>
    <property name="gender" value="男"></property>
 </bean>
```

##### 4.依赖注入之构造器注入

```xml
<bean id="studentThree" class="com.zhang.pojo.Student">
    <constructor-arg value="10003"></constructor-arg>
    <constructor-arg value="关羽"></constructor-arg>
    <constructor-arg value="23"></constructor-arg>
    <constructor-arg value="男"></constructor-arg>
</bean>
```

##### 5.依赖注入之特殊值处理

为属性赋值为null

```xml
<bean id="studentFour" class="com.zhang.pojo.Student">
    <property name="sid" value="10005"></property>
    <property name="sname" value="刘备"></property>
    <!--为属性赋值为null-->
    <property name="age" >
        <null/>
    </property>
    <property name="gender" value="男"></property>
</bean>
```

注意：以下写法，为name所赋的值是字符串nul

```xml
<property name="name" value="null"></property>
```

CDATA节

##### 6.为类类型属性赋值

方式一：引用外部已声明的bean

```xml
<!--外部申明一个clazz类，-->
<bean id ="ClazzOne" class="com.zhang.pojo.Clazz">
    <property name="clazzId" value="111"></property>
    <property name="clazzName" value="高三一班"></property>
 </bean>
<bean id="StudentFour" class="com.zhang.pojo.Student">
    <property name="sid" value="1004"></property>
    <property name="sname" value="赵六"></property>
    <property name="gender" value="男"></property>
    <!--在student的bean里面使用ref将其引入进来，注意此处不能使用value-->
    <property name="clazz" ref="ClazzOne"></property>
</bean>
```

方式二：内部bean

在一个bean中再声明一个bean成为内部bean。内部bean只能用于给属性赋值，不能在外部通过IOC容器获取，因此可以省略id属性

```xml
<bean id="studentFive"  class="com.zhang.pojo.Student">
    <property name="sid" value="1004"></property>
    <property name="sname" value="赵六"></property>
    <property name="gender" value="男"></property>
    <property name="clazz">
        <bean id="clazzInner" class="com.zhang.pojo.Clazz" >
            <property name="clazzId"  value="222"></property>
            <property name="clazzName" value="高三二班"></property>
        </bean>
    </property>
</bean>
```

方式三：级联属性赋值

```xml
<bean id="studentSix" class="com.zhang.pojo.Student">
    <property name="sid" value="1004"></property>
    <property name="sname" value="赵六"></property>
    <property name="gender" value="男"></property>
    <!-- 一定先引用某个bean为属性赋值，才可以使用级联方式更新属性 -->
    <property name="clazz" ref="ClazzOne"></property>
    <property name="clazz.clazzName" value="最强王者版"></property>
    <property name="clazz.clazzId" value="123"></property>
</bean>
```

##### 7.为数组类型属性赋值

```xml
<bean id="studentSeven" class="com.zhang.pojo.Student">
    <property name="sid" value="1004"></property>
    <property name="sname" value="赵六"></property>
    <property name="gender" value="男"></property>
    <property name="clazz" ref="ClazzOne"></property>
    <property name="hobbies">
        <array>
            <value>抽烟</value>
            <value>喝酒</value>
            <value>烫头</value>
        </array>
    </property>
</bean>
```

##### 8.为集合类型属性赋值

```xml
<bean class="com.zhang.pojo.Clazz" id="clazzTwo">
    <property name="clazzId" value="333"></property>
    <property name="clazzName" value="高二一班"></property>
    <property name="students">
        <list>
            <ref bean="studentOne"></ref>
            <ref bean="studentTwo"></ref>
            <ref bean="studentThree"></ref>
        </list>
    </property>
</bean>
```

若集合类型为set类型，只需要将其中的list标签改为set标签即可

为Map集合类型属性赋值

```xml
<bean id="studentEight" class="com.zhang.pojo.Student">
    <property name="sid" value="1004"></property>
    <property name="sname" value="赵六"></property>
    <property name="gender" value="男"></property>
    <property name="clazz" ref="ClazzOne"></property>
    <property name="teacherMap">
        <map>
            <entry key="10010" value-ref="teacherOne"></entry>
            <entry key="10012" value-ref="teacherTwo"></entry>
        </map>
    </property>
</bean>
```

方式2：使用 **<util:mapp>**

```xml
<bean id="studentEight" class="com.zhang.pojo.Student">
    <property name="sid" value="1004"></property>
    <property name="sname" value="赵六"></property>
    <property name="gender" value="男"></property>
    <property name="clazz" ref="ClazzOne"></property>
    <property name="teacherMap" ref="teacherMap">
        <!--<map>
            <entry key="10010" value-ref="teacherOne"></entry>
            <entry key="10012" value-ref="teacherTwo"></entry>
        </map>-->
    </property>
</bean>
<util:map id="teacherMap">
    <entry key="10010" value-ref="teacherTwo"></entry>
    <entry key="10012" value-ref="teacherOne"></entry>
</util:map>
```

##### 9.p命名空间

引入p命名空间后，可以通过以下方式为bean的各个属性赋值

```xml
<bean id="studentTen" class="com.zhang.pojo.Student"
      p:sid="1003" p:sname="诸葛亮" p:teacherMap-ref="teacherMap">
</bean>
```

#### 3.引入外部属性文件（了解）

加入依赖

```xml
<!-- MySQL驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.16</version>
</dependency>
<!-- 数据源 -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.0.13</version>
</dependency>
</dependencies>
```

创建外部属性文件

```xml
jdbc.user=root
jdbc.password=atguigu
jdbc.url=jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC
jdbc.driver=com.mysql.cj.jdbc.Driver
```

引入属性文件

```xml
<!-- 引入外部属性文件 -->
<context:property-placeholder location="classpath:jdbc.properties"/>
```

配置bean

```xml
<bean id="druidDataSource" class="com.alibaba.druid.pool.DruidDataSource">
	<property name="url" value="${jdbc.url}"/>
	<property name="driverClassName" value="${jdbc.driver}"/>
	<property name="username" value="${jdbc.user}"/>
	<property name="password" value="${jdbc.password}"/>
</bean>
```

#### 4.bean的作用域（了解）

```xml
<!-- scope属性：取值singleton（默认值），bean在IOC容器中只有一个实例，IOC容器初始化时创建对象 -->
<!-- scope属性：取值prototype，bean在IOC容器中可以有多个实例，getBean()时创建对象 -->
<bean id="student" class="com.zhang.pojo.Student" scope="prototype">
<property name="sid" value="123"></property>
<property name="sname" value="tom"></property>
```

如果是在WebApplicationContext环境下还会有另外两个作用域（但不常用）：

request 在一个请求范围内有效

 session 在一个会话范围内有效

#### 5.FactoryBean

​		 FactoryBean是Spring提供的一种整合第三方框架的常用机制。和普通的bean不同，配置一个 FactoryBean类型的bean，在获取bean的时候得到的并不是class属性中配置的这个类的对象，而是 getObject()方法的返回值。通过这种机制，Spring可以帮我们把复杂组件创建的详细过程和繁琐细节都 屏蔽起来，只把最简洁的使用界面展示给我们。 将来我们整合Mybatis时，Spring就是通过FactoryBean机制来帮我们创建SqlSessionFactory对象的。

配置bean

```xml
<bean id="user2" class="com.zhang.factory.UserFactoryBean"></bean>
```

```java
public class UserFactoryBean  implements FactoryBean<User> {
    public User getObject() throws Exception {
        return new User(1,"张三","23232",23);
    }
    public Class<?> getObjectType() {
        return User.class;
    }
}
```

#### 6.基于xml的自动装配

使用bean标签的autowire属性设置自动装配效果 

自动装配方式：byType 

byType：根据类型匹配IOC容器中的某个兼容类型的bean，为属性自动赋值 

- 若在IOC中，没有任何一个兼容类型的bean能够为属性赋值，则该属性不装配，即值为默认值 null 
- 若在IOC中，有多个兼容类型的bean能够为属性赋值，则抛出异常 NoUniqueBeanDefinitionException

```xml
<bean id="userController"class="com.zhang.autowire.xml.controller.UserController" autowire="byType"></bean>
<bean id="userService" class="com.zhang.autowire.xml.service.impl.UserServiceImpl" autowire="byType"></bean>
<bean id="userDao" class="com.zhang.autowire.xml.dao.impl.UserDaoImpl"></bean>
```

自动装配方式：byName 

byName：将自动装配的属性的属性名，作为bean的id在IOC容器中匹配相对应的bean进行赋值

```xml
<bean id="userController"
	class="com.zhang.autowire.xml.controller.UserController" autowire="byName">
</bean>
<bean id="userService"
	class="com.zhang.autowire.xml.service.impl.UserServiceImpl" autowire="byName">
</bean>
<bean id="userServiceImpl"
	class="com.zhang.autowire.xml.service.impl.UserServiceImpl" autowire="byName">
</bean>
<bean id="userDao" class="com.zhang.autowire.xml.dao.impl.UserDaoImpl"></bean>
<bean id="userDaoImpl" class="com.zhang.autowire.xml.dao.impl.UserDaoImpl">
</bean>
```

#### 7.基于注解管理bean

Spring 为了知道程序员在哪些地方标记了什么注解，就需要通过扫描的方式，来进行检测。然后根据注 解进行后续操作。

- @Component：将类标识为普通组件
- @Controller：将类标识为控制层组件
- @Service：将类标 识为业务层组件 
- @Repository：将类标识为持久层组件

```xml
<!--扫描组件-->
    <!--<context:component-scan base-package="com.zhang.controller,com.zhang.dao,com.zhang.service"></context:component-scan>-->
        //也可以这样写，扫描大包
    <context:component-scan base-package="com.zhang"></context:component-scan>
```

排除扫描某个组件

```xml
<context:component-scan base-package="com.zhang">
    <!--排除扫描controller-->
    <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"></context:exclude-filter>
</context:component-scan>
```

只扫描某个组件

```xml
<context:component-scan base-package="com.zhang" use-default-filters="false">
   <!--
    	use-default-filters属性：取值false表示关闭默认扫描规则
		type：设置排除或包含的依据
		type="annotation"，根据注解排除，expression中设置要排除的注解的全类名
		type="assignable"，根据类型排除，expression中设置要排除的类型的全类名
	-->
<!--只扫描某个类型的注解-->
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

组件所对应的bean的id

在我们使用XML方式管理bean的时候，每个bean都有一个唯一标识，便于在其他地方引用。现在使用 注解后，每个组件仍然应该有一个唯一标识。

默认情况 类名首字母小写就是bean的id。例如：UserController类对应的bean的id就是userController。

 自定义bean的id 可通过标识组件的注解的value属性

```java
UserService userService = ioc.getBean("userServiceImpl",UserService.class);
@Service("userService")
public class UserServiceImpl implements UserService {
}
```

### 3.AOP

#### 1.代理模式

​		二十三种设计模式中的一种，属于结构型模式。它的作用就是通过提供一个代理类，让我们在调用目标 方法的时候，不再是直接对目标方法进行调用，而是通过代理类间接调用。让不属于目标方法核心逻辑 的代码从目标方法中剥离出来——解耦。调用目标方法时先调用代理对象的方法，减少对目标方法的调 用和打扰，同时让附加功能能够集中在一起也有利于统一维护。

- 代理：将非核心逻辑剥离出来以后，封装这些非核心逻辑的类、对象、方法。 
- 目标：被代理“套用”了非核心逻辑代码的类、对象、方法。

#### 2.AOP

##### 1.相关概念

AOP（Aspect Oriented Programming）是一种设计思想，是软件设计领域中的面向切面编程，它是面向对象编程的一种补充和完善，它以通过预编译方式和运行期动态代理方式实现**在不修改源代码的情况下给程序动态统一添加额外功能**的一种技术。

横切关注点

 从每个方法中抽取出来的同一类非核心业务。在同一个项目中，我们可以使用多个横切关注点对相关方 法进行多个不同方面的增强。 这个概念不是语法层面天然存在的，而是根据附加功能的逻辑上的需要：**有十个附加功能，就有十个横 切关注点**。

通知 

**每一个横切关注点上要做的事情都需要写一个方法来实现，这样的方法就叫通知方法**。 

- 前置通知：在被代理的目标方法前执行 
- 返回通知：在被代理的目标方法成功结束后执行（寿终正寝） 
- 异常通知：在被代理的目标方法异常结束后执行（死于非命） 
- 后置通知：在被代理的目标方法最终结束后执行（盖棺定论） 
- 环绕通知：使用try...catch...finally结构围绕整个被代理的目标方法，包括上面四种通知对应的所有位置

切面 

封装通知方法的类。

目标

 被代理的目标对象。

 代理

 向目标对象应用通知之后创建的代理对象。 

连接点 

这也是一个纯逻辑概念，不是语法定义的。 把方法排成一排，每一个横切位置看成x轴方向，把方法从上到下执行的顺序看成y轴，x轴和y轴的交叉 点就是连接点。

切入点 

定位连接点的方式。 每个类的方法中都包含多个连接点，所以连接点是类中客观存在的事物（从逻辑上来说）。 如果把连接点看作数据库中的记录，那么切入点就是查询记录的 SQL 语句。 Spring 的 AOP 技术可以通过切入点定位到特定的连接点。切点通过 org.springframework.aop.Pointcut 接口进行描述，它使用类和方法作为连接点的查询条 件。

作用 

简化代码：把方法中固定位置的重复的代码抽取出来，让被抽取的方法更专注于自己的核心功能， 提高内聚性。 

代码增强：把特定的功能封装到切面类中，看哪里有需要，就往上套，被套用了切面逻辑的方法就 被切面给增强了。

##### 2.基于注解的AOP

1.添加依赖

```xml
<dependencies>
    <!--spring-ioc的依赖-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.1</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <!--spring-aspects会帮我们传递过来的sapectjweaver-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>4.3.13.RELEASE</version>
    </dependency>
</dependencies>
```

2.准备被代理的目标资源

```java
@Component
public class CalculatorImpl implements Calculator{
    public int add(int i, int j) {
        int result = i + j;
        System.out.println("方法内部 result = " + result);
        return result;
    }
}
```

3.创建切面类并配置

```java
@Component
@Aspect //将当前组件标识为切面组件
public class LoggerAspect {
    /*前置通知
    * 在切面中，需要通过指定的注解将方法标识为通知方法
    * @Before 前置通知，在目标对象执行之前执行
    * */
    @Before("execution(public int com.zhang.aop.CalculatorImpl.add(int,int))")
    public void beforeAdviceMethod(){
        System.out.println("loggeraspect，前置通知");
    }
    //写法二
    /*切入点表达式：设置在标识统治的注解的value属性中
    * 第一个*表示任意的访问修饰符和返回值类型
    * 第二个*表示类中任意的方法
    * ..表示任意的参数列表
    * 4.获取连接点的信息
 	* 在通知方法的参数位置，设置JoinPoint类型的参数，就可以获取连接点所对应方法的信息
    * */
    @Before("execution(* com.zhang.aop.CalculatorImpl.*(..))")
     public void beforeAdviceMethod(){
         //获取连接点所对应的方法名
        Signature signature = joinPoint.getSignature();
        //获取连接点所对应的方法的参数
        Object[] args = joinPoint.getArgs();
        System.out.println("loggeraspect,方法："+signature.getName()+",参数："+ Arrays.toString(args));
        System.out.println("loggeraspect，前置通知");
    }
}
```

重用切入点表达式

 * @Pointcut声明一个公共的切入点表达式
 * @Pointcut("execution(* com.atguigu.spring.aop.annotation.CalculatorImpl.*(..))")
 * public void pointCut(){}
 * 使用方式：@Before("pointCut()")

```java
<!-- @After：后置通知，在目标对象方法的finally字句中执行-->
@After("pointCut()")
public void afterAdviceMethod(){
 Signature signature = joinPoint.getSignature();
        System.out.println("loggeraspect,方法："+signature.getName()+"执行完毕");
}
```

返回通知

```java
/**
     * 在返回通知中若要获取目标对象方法的返回值
     * 只需要通过@AfterReturning注解的returning属性
     * 就可以将通知方法的某个参数指定为接收目标对象方法的返回值的参数
     */ 
@AfterReturning(value ="pointCut()",returning = "result")
    public void afterReturnAdviceMethod(JoinPoint joinPoint,Object result){
        Signature signature = joinPoint.getSignature();
        System.out.println("loggeraspect,方法："+signature.getName()+",结果："+result);
//        System.out.println("loggeraspect，返回通知");
    }
```

异常通知

@AfterThrowing：异常通知，在目标对象方法的catch字句中执行

```java
@AfterThrowing(value = "pointCut()",throwing="ex")
public void afterThrowingAdviceMethod(JoinPoint joinPoint,Throwable ex){
    Signature signature =joinPoint.getSignature();
    System.out.println("loggeraspect,方法："+signature.getName()+",结果："+ex);
}
```

环绕通知

使用@Around注解标识，使用try...catch...finally结构围绕整个被代理的目标方法，包 括上面四种通知对应的所有位置

```java
@Around("pointCut()")
public Object aroundMethod(ProceedingJoinPoint proceedingJoinPoint){
    Object proceed=null;
    try {
        System.out.println("环绕通知，前置通知");
         proceed = proceedingJoinPoint.proceed();
        System.out.println("环绕通知，返回通知");

    } catch (Throwable throwable) {
        throwable.printStackTrace();
        System.out.println("环绕通知，异常通知");

    }finally {
        System.out.println("环绕通知，后置通知");

    }
	return proceed;
}
```

切面的优先级

​		使用@Order注解可以控制切面的优先级：

 		@Order(较小的数)：优先级高 

​		 @Order(较大的数)：优先级低

##### 3.基于xml的AOP

```xml
<!--扫描组件-->
<context:component-scan base-package="com.zhang.aop.xml"></context:component-scan>
<aop:config>
    <!--设置一个公共的切入点表达式-->
    <aop:pointcut id="pointCut" expression="execution(* com.zhang.aop.xml.CalculatorImpl.*(..))"/>
    <!--将ioc中的某个bean设置为切面-->
    <aop:aspect ref="loggerAspect">
        <aop:before method="beforeAdviceMethod" pointcut-ref="pointCut"></aop:before>
        <aop:before method="afterAdviceMethod" pointcut-ref="pointCut"></aop:before>
        <aop:after-returning method="afterReturnAdviceMethod" returning="result" pointcut-ref="pointCut"></aop:after-returning>
        <aop:after-throwing method="afterThrowingAdviceMethod" throwing="ex" pointcut-ref="pointCut"></aop:after-throwing>
    </aop:aspect>
</aop:config>
```

##### 4.声明式事务

JdbcTemplate

设置spring的配置文件

```xml
<!--引入jdbc的配置文件-->
<context:property-placeholder location="jdbc.properties"></context:property-placeholder>
<bean id="datasource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="${jdbc.driver}"></property>
    <property name="url" value="${jdbc.url}"></property>
    <property name="username" value="${jdbc.username}"></property>
    <property name="password" value="${jdbc.password}"></property>
</bean>
<bean class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="datasource"></property>
</bean>
```

测试类

```java
@RunWith(SpringJUnit4ClassRunner.class)
/*设置spring测试环境的配置文件*/
@ContextConfiguration("classpath:spring-jdbc.xml")
public class JdbcTemplateTest {
    @Autowired
    private JdbcTemplate jdbcTemplate;
    @Test
    public void testInsert(){
        String sql = "insert into t_user values(6,?,?,?,?,?)";
        jdbcTemplate.update(sql,"张三","2334",23,"男","12234@qq.com");
    }
    /*查询方法*/
    @Test
    public void testQuery(){
        String sql = "select * from t_user where id=?";
        User user=jdbcTemplate.queryForObject(sql,new BeanPropertyRowMapper<>(User.class),1);
        System.out.println(user);
    }
}
```

## 三.springmvc

### 1.SpringMVC简介

MVC是一种软件架构的思想，将软件按照模型.视图.控制器来划分

 M：Model，模型层，指工程中的JavaBean，作用是处理数据

V：View，视图层，指工程中的html或jsp等页面，作用是与用户进行交互，展示数据

 C：Controller，控制层，指工程中的servlet，作用是接收请求和响应浏览器

MVC的工作流程： 用户通过视图层发送请求到服务器，在服务器中请求被Controller接收，Controller 调用相应的Model层处理请求，处理完毕将结果返回到Controller，Controller再根据请求处理的结果 找到相应的View视图，渲染数据后最终响应给浏览器

##### 1.SpringMVC

SpringMVC 是 Spring 为表述层开发提供的一整套完备的解决方案。在表述层框架历经 Strust、WebWork、Strust2 等诸多产品的历代更迭之后，目前业界普遍选择了 SpringMVC 作为 Java EE 项目 表述层开发的首选方案。

特点：

- Spring 家族原生产品，与 IOC 容器等基础设施无缝对接 
- 基于原生的Servlet，通过了功能强大的前端控制器DispatcherServlet，对请求和响应进行统一 处理
-  表述层各细分领域需要解决的问题全方位覆盖，提供全面解决方案 
- 代码清新简洁，大幅度提升开发效率 内部组件化程度高，可插拔式组件即插即用，想要什么功能配置相应组件即可 
- 性能卓著，尤其适合现代大型.超大型互联网项目要求

### 2.入门案例

引入依赖

```xml
<dependencies>
    <!-- SpringMVC -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.7.RELEASE</version>
    </dependency>
    <!-- 日志 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.1.3</version>
    </dependency>
    <!-- ServletAPI -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
    <!-- Spring5和Thymeleaf整合包 -->
     <!-- Spring5和Thymeleaf整合包 -->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
            <version>3.0.15.RELEASE</version>
        </dependency>
</dependencies>
```

配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--配置springmvc的前端控制器-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    </servlet>
   <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
     <!--将dispatcherServlet的初始化时间提前到服务器启动时-->
    <load-on-startup>1</load-on-startup>
    <!-- url-pattern中/和/*的区别
      / ：浏览器向服务器所发送的所有请求(不包括.jsp)
    "/*" ：匹配浏览器向服务器发送的所有请求(包括jsp)-->
</web-app>
```

创建springmvc的配置文件

```xml
<!--扫描控制层组件-->
    <context:component-scan base-package="com.zhang.controller"></context:component-scan>
    <!-- 配置Thymeleaf视图解析器 -->
    <bean id="viewResolver"
          class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <property name="order" value="1"/>
        <property name="characterEncoding" value="UTF-8"/>
        <property name="templateEngine">
            <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
                <property name="templateResolver">
                    <bean
                        class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                        <!-- 视图前缀 -->
                        <property name="prefix" value="/WEB-INF/templates/"/>
                        <!-- 视图后缀 -->
                        <property name="suffix" value=".html"/>
                        <property name="templateMode" value="HTML5"/>
                        <property name="characterEncoding" value="UTF-8" />
                    </bean>
                </property>
            </bean>
        </property>
    </bean>
```

控制层代码

```java
@RequestMapping("/")
public String protal(){
    //将逻辑视图返回
    return  "index";
}
```

总结

浏览器发送请求，若请求地址符合前端控制器的url-pattern，该请求就会被前端控制器 DispatcherServlet处理。**前端控制器会读取SpringMVC的核心配置文件**，通过扫描组件找到控制器， 将请求地址和控制器中@RequestMapping注解的value属性值进行匹配，若匹配成功，该注解所标识的 控制器方法就是处理请求的方法。处理请求的方法需要返回一个字符串类型的视图名称，该视图名称会被视图解析器解析，加上前缀和后缀组成视图的路径，通过Thymeleaf对视图进行渲染，最终转发到视 图所对应页面

### 3.@RequestMapping注解

##### 1.注解的功能 

从注解名称上我们可以看到，@RequestMapping注解的作用就是将请求和处理请求的控制器方法关联起来，建立映射关系。 SpringMVC 接收到指定的请求，就会来找到在映射关系中对应的控制器方法来处理这个请求。 

##### 2.注解的位置 

@RequestMapping标识一个类：设置映射请求的请求路径的初始信息 

@RequestMapping标识一个方法：设置映射请求请求路径的具体信息

```java
@Controller
@RequestMapping("/test")
public class TestRequestMapping {
    @RequestMapping("/hello")
    public String hello(){
        return "success";
    }
}
```

```html
<!--此处访问不到http://localhost/springmvc_demo_war_exploded/hello
而可以访问到http://localhost/springmvc_demo_war_exploded/test/hello这个路径
-->
<body>
<a th:href="@{/hello}">success</a>
</body>
```

##### 3.value属性

 @RequestMapping注解的value属性通过请求的请求地址匹配请求映射

 @RequestMapping注解的value属性是一个字符串类型的数组，表示该请求映射能够匹配多个请求地址所对应的请求 **@RequestMapping注解的value属性必须设置，至少通过请求地址匹配请求映射**

```java
/**@RequestMapping注解value属性
 * 作用：通过请求的请求路径匹配请求
 * value属性是数组类型，即当前浏览器所发送请求的请求路径匹配value属性中的任何一个值
 * 则当前请求就会被注解所标识的方法进行处理
 */
@RequestMapping(
        value = {"/testRequestMapping", "/test"}
)
public String testRequestMapping(){
    return "success";
}
```

##### 4.method属性

 @RequestMapping注解的method属性通过请求的请求方式（get或post）匹配请求映射

 @RequestMapping注解的method属性是一个RequestMethod类型的数组，表示该请求映射能够匹配 多种请求方式的请求

 若当前请求的请求地址满足请求映射的value属性，但是请求方式不满足method属性，则浏览器报错 405：Request method 'POST' not supported

```java
@RequestMapping(
        value = {"/hellos", "/test"},method = RequestMethod.GET
    <!--method = {RequestMethod.GET，RequestMethod.POST}这种设置方式可以匹配两种请求 -->
)
public String testRequestMapping(){
    return "success";
}
```

```html
<body>
	<h1 style="color:red;">张三</h1>
	<a th:href="@{/hellos}">success</a>
	<a th:href="@{/test}">success</a>
</body>
```

**注意：目前只有表单或者设置为post请求，则请求方式为post请求；否则是put请求**

 1.对于处理指定请求方式的控制器方法，SpringMVC中提供了@RequestMapping的派生注解 

- 处理get请求的映射-->@GetMapping
- 处理post请求的映射-->@PostMapping 
- 处理put请求的映射-->@PutMapping 
- 处理delete请求的映射-->@DeleteMapping 

2.常用的请求方式有get，post，put，delete 但是目前浏览器只支持get和post，若在form表单提交时，为method设置了其他请求方式的字符 串（put或delete），则按照默认的请求方式get处理 若要发送put和delete请求，则需要通过spring提供的过滤器HiddenHttpMethodFilter

##### 5.params属性（了解） 

@RequestMapping注解的params属性通过请求的请求参数匹配请求映射 @RequestMapping注解的params属性是一个字符串类型的数组，可以通过四种表达式设置请求参数 和请求映射的匹配关系 

"param"：要求请求映射所匹配的请求必须携带param请求参数

"!param"：要求请求映射所匹配的请求必须不能携带param请求参数

 "param=value"：要求请求映射所匹配的请求必须携带param请求参数且param=value 

"param!=value"：要求请求映射所匹配的请求必须携带param请求参数但是param!=value

```html
<a th:href="@{/test(username='admin',password=123456)">测试@RequestMapping的
    params属性-->/test</a><br>
```

```java
@RequestMapping(
        value = {"/testRequestMapping", "/test"}
        ,method = {RequestMethod.GET, RequestMethod.POST}
        ,params = {"username","password!=123456"}
)
public String testRequestMappings(){
    return "success";
}
```

注： 若当前请求满足@RequestMapping注解的value和method属性，但是不满足params属性，此时页面回报错

400：Parameter conditions "username, password!=123456" not met for actual request parameters: username={admin}, password={123456}

##### 3.6.@RequestMapping注解的headers属性（了解） 

@RequestMapping注解的headers属性通过请求的请求头信息匹配请求映射 @RequestMapping注解的headers属性是一个字符串类型的数组，可以通过四种表达式设置请求头信息和请求映射的匹配关系 

"header"：要求请求映射所匹配的请求必须携带header请求头信息 

"!header"：要求请求映射所匹配的请求必须不能携带header请求头信息 

"header=value"：要求请求映射所匹配的请求必须携带header请求头信息且header=value 

"header!=value"：要求请求映射所匹配的请求必须携带header请求头信息且header!=value 

若当前请求满足@RequestMapping注解的value和method属性，但是不满足headers属性，此时页面 显示404错误，即资源未找到

##### 7.SpringMVC支持ant风格的路径 

？：表示任意的单个字符 

*：表示任意的0个或多个字符 ：表示任意层数的任意目录 

注意：在使用时，只能使用/**/xxx的方式

```html
<a th:href="/aaa/bb/ant">测试ant</a>
```

```java
@RequestMapping("/a?a/bb/ant")
public String testRequestMappings(){
    return "success";
}
```

##### 8.SpringMVC支持路径中的占位符（重点）

 原始方式：/deleteUser?id=1 

rest方式：/user/delete/1

SpringMVC路径中的占位符常用于RESTful风格中，当请求路径中将某些数据通过路径的方式传输到服务器中，就可以在相应的@RequestMapping注解的value属性中通过占位符{xxx}表示传输的数据，在通过@**PathVariable**注解，将占位符所表示的数据赋值给控制器方法的形参

```html
<a th:href="@{test/rest/1">测试value属性中的占位符</a>
```

```java
  @RequestMapping("test/rest/{id}")
public String testRest(@PathVariable("id") Integer id){
        System.out.println("id"+id);
        return "success";
}
```

### 4.SpringMVC获取请求参数

#### 1.通过ServletAPI获取 

将HttpServletRequest作为控制器方法的形参，此时HttpServletRequest类型的参数表示封装了当前请求的请求报文的对象

```java
@RequestMapping("/param/servletAPI")
public String getParamByServletApi(HttpServletRequest request){
    String username = request.getParameter("username");
    String password = request.getParameter("password");
    System.out.println("username:"+username+",password:"+password);
    return "success";
}
```

```html
<form th:action="@{/param/servletAPI}" method="post">
    用户名：<input type="text" name="username"><br>
    密 码：<input type="password" name="password"> <br>
    <input type="submit" value="登录"><br>
</form>
```

#### 2.通过控制器方法的形参获取请求参数

在控制器方法的形参位置，**设置和请求参数同名的形参**，当浏览器发送请求，匹配到请求映射时，在 DispatcherServlet中就会将请求参数赋值给相应的形参

```java
@RequestMapping("/param")
public String getParam( String username, String password){
    System.out.println("username:"+username+",:password:"+password);
    return "success";
}
```

```html
<form th:action="@{/param}" method="post">
    用户名：<input type="text" name="userName"><br>
    密 码：<input type="password" name="password"> <br>
    <input type="submit" value="登录"><br>
</form>
```

注： 若请求所传输的请求参数中有多个同名的请求参数，此时可以在控制器方法的形参中设置字符串数组或者字符串类型的形参接收此请求参数，若使用字符串数组类型的形参，此参数的数组中包含了每一个数据 若使用字符串类型的形参，此参数的值为每个数据中间使用逗号拼接的结果。

#### 3.@RequestParam

@RequestParam是将请求参数和控制器方法的形参创建映射关系

 @RequestParam注解一共有三个属性： 

value：指定为形参赋值的请求参数的参数名

 required：设置是否必须传输此请求参数，默认值为true 若设置为true时，则当前请求必须传输value所指定的请求参数，若没有传输该请求参数，且没有设置 defaultValue属性，则页面报错400：Required String parameter 'xxx' is not present；若设置为 false，则当前请求不是必须传输value所指定的请求参数，若没有传输，则注解所标识的形参的值为 null 

defaultValue：不管required属性值为true或false，当value所指定的请求参数没有传输或传输的值 为""时，则使用默认值为形参赋值

```java
<!-- value表示传入的参数，required表示必须设置一个参数，默认为true;defaultvalue表示传入参数的默认值-->
@RequestMapping("/param")
public String getParam(@RequestParam(value = "userName",required = true,defaultValue = "hello") String username, String password){
    System.out.println("username:"+username+",:password:"+password);
    return "success";
}
```

#### 4.@RequestHeader和@CookieValue

@RequestHeader是将请求头信息和控制器方法的形参创建映射关系

 @RequestHeader注解一共有三个属性：value、required、defaultValue，用法同@RequestParam

```java
@RequestMapping("/param")
public String getParam(@RequestParam(value = "userName",required = true,defaultValue = "hello") String username,
                       String password,
                       @RequestHeader(value = "referer") String referer){
    System.out.println("username:"+username+",:password:"+password);
    return "success";
}
```

@CookieValue是将cookie数据和控制器方法的形参创建映射关系 

@CookieValue注解一共有三个属性：value、required、defaultValue，用法同@RequestParam

```java
@RequestMapping("/param")
public String getParam(@RequestParam(value = "userName",required = true,defaultValue = "hello") String 	username,String password,@RequestHeader(value = "referer") String referer,@CookieValue("JSESSIONID") String jessionId){
    System.out.println("username:"+username+",:password:"+password);
    System.out.println("jessionid:"+jessionId);
    return "success";
}
```

#### 5.通过POJO获取请求参数

可以在控制器方法的形参位置设置一个实体类类型的形参，此时若**浏览器传输的请求参数的参数名和实体类中的属性名一致**，那么请求参数就会为此属性赋值

```html
<form th:action="@{/param/pojo}" method="post">
    用户名：<input type="text" name="username"><br>
    密 码：<input type="password" name="password"> <br>
    <input type="submit" value="登录"><br>
</form>
```

```java
@RequestMapping("/param/pojo")
public String getParamByPojo(User user){
    System.out.println(user);
    return "success";
}
```

#### 6.解决获取请求参数的乱码问题

```xml
<!--配置Spring的编码过滤器-->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

**注： SpringMVC中处理编码的过滤器一定要配置到其他过滤器之前，否则无效**

### 5.域对象共享数据

#### 1.使用ServletAPI向request域对象共享数据

```java
@RequestMapping("/testServletAPI") 
public String testServletAPI(HttpServletRequest request){ 
    request.setAttribute("testScope", "hello,servletAPI");
    return "success"; 
}
```

#### 2.使用ModelAndView向request域对象共享数据

```html
<p th:text="${testRequestScopes}"></p>
```

```java
@RequestMapping("/test/mav")
public ModelAndView testMAV(){
    /*
    * ModelAndView包含Model和View的功能
    * Model：向请求域中共享数据
    * View：设置逻辑视图实现页面跳转
    * */
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.addObject("testRequestScopes","hello,modelandview");
    modelAndView.setViewName("success");
    return modelAndView;
}
```

#### 3.使用Model和ModelMap向request域对象共享数据

```java
@RequestMapping("/test/model")
public String testModel(Model model){
    model.addAttribute("username","tom");
    return "success";
}
@RequestMapping("/test/modelmap")
    public String testModelMap(ModelMap model){
        model.addAttribute("username","tom");
        return "success";
    }
```

```html
<p th:text="${username}"></p><br>
```

Model、ModelMap的关系

 Model、ModelMap类型的参数其实本质上都是 **BindingAwareModelMap** 类型的

#### 4.向session域和application域中共享数据

```java
@RequestMapping("/testSession")
public String testSession(HttpSession session){
    session.setAttribute("testSessionScope", "hello,session");
    return "success";
}
@RequestMapping("/testApplication")
public String testApplication(HttpSession session){
    ServletContext application = session.getServletContext();
    application.setAttribute("testApplicationScope", "hello,application");
    return "success";
}
```

```html
<p th:text="${session.testSessionScope}"></p>
<p th:text="${application.testApplicationScope}"></p>
```

```html
<a th:href="@{/testSession}">测试session</a>
<a th:href="@{/test/testApplication}">测试application</a>
```

### 6.SpringMVC的视图

#### 1.ThymeleafView 

当控制器方法中所设置的视图名称没有任何前缀时，此时的视图名称会被SpringMVC配置文件中所配置 的视图解析器解析，视图名称拼接视图前缀和视图后缀所得到的最终路径，会通过转发的方式实现跳转

```java
@RequestMapping("/testHello")
public String testHello(){
	return "hello"; 
}
```

#### 2.转发视图

 SpringMVC中默认的转发视图是**InternalResourceView** 

SpringMVC中创建转发视图的情况：

 当控制器方法中所设置的视图名称以"forward:"为前缀时，创建InternalResourceView视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀"**forward:**"去掉，剩余部分作为最终路径通过转发的方式实现跳转 

例如"**forward:/**"，"**forward:/employee**"

```java
@RequestMapping("/testForward")
public String testForward(){ 
	return "forward:/testHello";
}
```

#### 3.重定向视图 

SpringMVC中默认的重定向视图是**RedirectView**

当控制器方法中所设置的视图名称以"**redirect:**"为前缀时，创建**RedirectView**视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀"redirect:"去掉，剩余部分作为最 终路径通过重定向的方式实现跳转

 例如"**redirect:/**"，"**redirect:/employee**"

```java
@RequestMapping("/testRedirect")
 public String testRedirect(){
 	return "redirect:/testHello";
 }
```

注： 重定向视图在解析时，会先将redirect:前缀去掉，然后会判断剩余部分是否以/开头，若是则会自动拼接上下文路径

#### 4.视图控制器view-controller 

当控制器方法中，仅仅用来实现页面跳转，即只需要设置视图名称时，可以将处理器方法使用**view-controller**标签进行表示

```xml
 <!--开启mvc的注解驱动-->
<mvc:annotation-driven></mvc:annotation-driven>
    <!--
	** path：设置处理的请求地址
	** view-name：设置请求地址所对应的视图名称
	-->
<mvc:view-controller path="/" view-name="index"></mvc:view-controller>
```

注： **当SpringMVC中设置任何一个view-controller时，其他控制器中的请求映射将全部失效**，此时需 要在SpringMVC的核心配置文件中设置开启mvc注解驱动的标签：

### 7.RESTful

#### 1.RESTful简介

 REST：Representational State Transfer，表现层资源状态转移。

 具体说，就是 HTTP 协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。 它们分别对应四种基本操作：

​		GET 用来获取资源，

​		POST 用来新建资源，

​		PUT 用来更新资源，

​		DELETE 用来删除资源。

#### 2.RESTful的实现

```java
@RequestMapping(value = "/user",method = RequestMethod.GET)
public String getAllUser(){
    System.out.println("查询所有的用户信息 /user--->get");
    return "success";
}
@RequestMapping(value = "/user/{id}",method = RequestMethod.GET)
public String getUserById(@PathVariable("id") Integer id){
    System.out.println("根据id查询用户信息 /user/"+id+"--->get");
    return "success";
}
@RequestMapping(value = "/user",method = RequestMethod.POST)
public String insertUser(){
    System.out.println("添加用户信息 /user--->post");
    return "success";
}
```

```html
<a th:href="@{/user}">查询所有的用户信息</a><br>
<a th:href="@{/user/1}">查询id=1的用户信息</a><br>
<form th:action="@{/user}" method="post">
    <input type="submit" value="添加用户信息">
</form>
```

#### 3.HiddenHttpMethodFilter

由于浏览器只支持发送get和post方式的请求，那么该如何发送put和delete请求呢？ 

SpringMVC 提供了 **HiddenHttpMethodFilter** 帮助我们将 POST 请求转换为 DELETE 或 PUT 请求

 HiddenHttpMethodFilter 处理put和delete请求的条件：

- 当前请求的请求方式必须为**post**

- 当前请求必须传输请求参数**_method** 


满足以上条件，HiddenHttpMethodFilter 过滤器就会将当前请求的请求方式转换为请求参数 _method的值，因此请求参数_method的值才是最终的请求方式 在web.xml中注册HiddenHttpMethodFilte

```xml
<!--设置处理请求方式的过滤器-->
<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

```html
<form th:action="@{/user}" method="post">
    <input type="hidden" name="_method" value="put">
    <input type="submit" value="修改用户信息">
</form>
```

### 8.SpringMVC处理ajax请求

#### 1.@RequestBody获取json格式的请求参数

**@RequestBody可以获取请求体信息**，使用@RequestBody注解标识控制器方法的形参，当前请求的请求体就会为当前注解所标识的形参赋值

在使用了axios发送ajax请求之后，浏览器发送到服务器的请求参数有两种格式：

1.**name=value&name=value...**，此时的请求参数可以通过**request.getParameter()**获取，对应 SpringMVC中，可以直接通过控制器方法的形参获取此类请求参数

2.{key:value,key:value,...}，此时无法通过request.getParameter()获取，之前我们使用操作 json的相关jar包gson或jackson处理此类请求参数，可以将其转换为指定的实体类对象或map集 合。在SpringMVC中，直接使用@RequestBody注解标识控制器方法的形参即可将此类请求参数转换为java对象

```java
@RequestMapping("/test/ajax")
public void testAjax(Integer id, @RequestBody String requestBody, HttpServletResponse response) throws IOException {
    System.out.println("requestBody:"+requestBody);
    response.getWriter().write("hello ,axios");
}
```

```html
<!--此时必须使用post请求方式，因为get请求没有请求体-->
<form th:action="@{/test/RequestBody}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    <input type="submit">
</form>
```

使用@RequestBody获取json格式的请求参数的条件：

导入jackson的依赖

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
```

SpringMVC的配置文件中设置开启mvc的注解驱动

```xml
<mvc:default-servlet-handler/>
<mvc:annotation-driven></mvc:annotation-driven>
```

在控制器方法的形参位置，设置json格式的请求参数要转换成的java类型（实体类或map）的参数，并使用@RequestBody注解标识

```java
//将json格式的数据转换为实体类对象
    @RequestMapping("/test/RequestBody/json")
    public void testRequestBody(@RequestBody User user, HttpServletResponse response) throws IOException {
        System.out.println(user);
	//User{id=null, username='admin', password='123456', age=null,gender = 'null'}
		response.getWriter(). print("hello,axios");
        }

```

#### 2.@ResponseBody

@ResponseBody用于标识一个控制器方法，**可以将该方法的返回值直接作为响应报文的响应体响应到浏览器**

```java
@RequestMapping("/testResponseBody")
 public String testResponseBody(){
 	//此时会跳转到逻辑视图success所对应的页面 
	return "success"; 
} 
@RequestMapping("/testResponseBody")
 @ResponseBody
 public String testResponseBody(){
 	//此时响应浏览器数据success
 	return "success"; 
 }
```

#### 3.@ResponseBody响应浏览器json数据

 服务器处理ajax请求之后，大多数情况都需要向浏览器响应一个java对象，此时必须将java对象转换为 json字符串才可以响应到浏览器，之前我们使用操作json数据的jar包gson或jackson将java对象转换为 json字符串。在SpringMVC中，我们可以直接使用**@ResponseBody**注解实现此功能

 @ResponseBody响应浏览器json数据的条件：

1.导入jackson的依赖

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
```

2.SpringMVC的配置文件中设置开启mvc的注解驱动

```xml
<!--开启mvc的注解驱动-->
<mvc:annotation-driven /> 
```

3.使用@ResponseBody注解标识控制器方法，在方法中，将需要转换为json字符串并响应到浏览器 的java对象作为控制器方法的返回值，此时SpringMVC就可以将此对象直接转换为json字符串并响应到 浏览器 

```js
testResponseBody(){
    axios.post("/SpringMVC/test/ResponseBody/json").then(response=>{
        console.log(response.data);
    });
}
```

```java
//响应浏览器list集合
@RequestMapping("/test/ResponseBody/json")
@ResponseBody
public List<User> testResponseBodys(){
    User user1 = new User(1001,"admin1","123456",23,"男");
    User user2 = new User(1002,"admin2","123456",23,"男");
    User user3 = new User(1003,"admin3","123456",23,"男");
    List<User> list = Arrays.asList(user1, user2, user3);
    return list;
}
//响应浏览器map集合
@RequestMapping("/test/ResponseBody/json")
@ResponseBody
public Map<String, Object> testResponseBody1(){
    User user1 = new User(1001,"admin1","123456",23,"男");
    User user2 = new User(1002,"admin2","123456",23,"男");
    User user3 = new User(1003,"admin3","123456",23,"男");
    Map<String, Object> map = new HashMap<>();
    map.put("1001", user1);
    map.put("1002", user2);
    map.put("1003", user3);
    return map;
}
//响应浏览器实体类对象
@RequestMapping("/test/ResponseBody/json")
@ResponseBody
public User testResponseBody(){
    return user;
}
```

### 9.文件上传和下载

####  1.文件下载

ResponseEntity用于控制器方法的返回值类型，该控制器方法的返回值就是响应到浏览器的响应报文，使用ResponseEntity实现下载文件的功能。

```java
@RequestMapping("/test/down")
public ResponseEntity<byte[]> testResponseEntity(HttpSession session) throws IOException {
    //获取ServletContext对象
    ServletContext servletContext = session.getServletContext();
    //获取服务器中文件的真实路径
    String realPath = servletContext.getRealPath("img");
    realPath = realPath + File.separator + "1.jpg";
    //创建输入流
    InputStream is = new FileInputStream(realPath);
    //创建字节数组，is.available()获取输入流所对应文件的字节数
    byte[] bytes = new byte[is.available()];
    //将流读到字节数组中
    is.read(bytes);
    //创建HttpHeaders对象设置响应头信息
    MultiValueMap<String, String> headers = new HttpHeaders();
    //设置要下载方式以及下载文件的名字
    headers.add("Content-Disposition", "attachment;filename=1.jpg");
    //设置响应状态码
    HttpStatus statusCode = HttpStatus.OK;
    //创建ResponseEntity对象
    ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(bytes, headers, statusCode);
    //关闭输入流
    is.close();
    return responseEntity;
}
```

```html
<a th:href="@{/test/down}">下载图片</a>
```

#### 2.文件上传

 文件上传要求form表单的请求方式必须为post，并且添加属性**enctype="multipart/form-data**" SpringMVC中将上传的文件封装到MultipartFile对象中，通过此对象可以获取文件相关信息 

上传步骤：

添加依赖

```xml
 <!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
    <dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.3.1</version>
    </dependency>
</dependencies>
```

在SpringMVC的配置文件中添加配置：

```xml
<!--配置文件上传解析器，将我们获取到的-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
</bean>
```

控制器方法

```java
@RequestMapping("/test/up")
public String testUp(MultipartFile photo, HttpSession session) throws IOException {
    //获取上传的文件的文件名
    String fileName = photo.getOriginalFilename();
    //获取上传的文件的后缀名
    String hzName = fileName.substring(fileName.lastIndexOf("."));
    //获取uuid
    String uuid = UUID.randomUUID().toString();
    //拼接一个新的文件名
    fileName = uuid + hzName;
    //获取ServletContext对象
    ServletContext servletContext = session.getServletContext();
    //获取当前工程下photo目录的真实路径
    String photoPath = servletContext.getRealPath("photo");
    //创建photoPath所对应的File对象
    File file = new File(photoPath);
    //判断file所对应目录是否存在
    if(!file.exists()){
        file.mkdir();
    }
    String finalPath = photoPath + File.separator + fileName;
    //上传文件
    photo.transferTo(new File(finalPath));
    return "success";
}
```

前端代码

```html
<form th:action="@{/test/up}" method="post" enctype="multipart/form-data">
    头像：<input type="file" name="photo"><br>
    <input type="submit" value="上传">
</form>
```

### 10.拦截器

####  1.拦截器的配置 

SpringMVC中的拦截器用于拦截控制器方法的执行 SpringMVC中的拦截器需要实现HandlerInterceptor SpringMVC的拦截器必须在SpringMVC的配置文件中进行配置：

```xml
<mvc:interceptors>
<!--以下两种配置方式都是对DispatcherServlet所处理的所有的请求进行拦截-->
    <!--<bean class="com.zhang.inceptor.FirstInceptor"/>-->
    <ref bean="firstInceptor"/>
</mvc:interceptors>
```

```xml
<mvc:interceptors>
    <!--以上配置方式可以通过ref或bean标签设置拦截器，通过mvc:mapping设置需要拦截的请求，通过
mvc:exclude-mapping设置需要排除的请求，即不需要拦截的请求-->
    <mvc:interceptor>
         <!--配置需要拦截的请求路径，/**表示所有的请求-->
        <mvc:mapping path="/*"/>
        <!--配置需要排除的请求的请求路径-->
        <mvc:exclude-mapping path="/abc"/>
        <ref bean="firstInceptor"></ref>
    </mvc:interceptor>
</mvc:interceptors>
```

SpringMVC中的拦截器有三个抽象方法：

 preHandle：控制器方法执行之前执行preHandle()，其boolean类型的返回值表示是否拦截或放行，返 回true为放行，即调用控制器方法；返回false表示拦截，即不调用控制器方法

 postHandle：控制器方法执行之后执行postHandle() 

afterCompletion：处理完视图和模型数据，渲染视图完毕之后执行afterCompletion()

### 11.异常处理器

####  1.基于xml配置的异常处理

 SpringMVC提供了一个处理控制器方法执行过程中所出现的异常的接口：HandlerExceptionResolver HandlerExceptionResolver接口的实现类有：DefaultHandlerExceptionResolver和 SimpleMappingExceptionResolver

SpringMVC提供了自定义的异常处理器SimpleMappingExceptionResolver，使用方式：

```xml
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <property name="exceptionMappings">
        <props>
    	<!--key设置要处理的异常，value设置要跳转的页面对应的逻辑视图-->
            <prop key="java.lang.ArithmeticException">error</prop>
        </props>
    </property>
    <!--exceptionAttribute属性设置一个属性名，将出现的异常信息在请求域中进行共享-->
    <property name="exceptionAttribute" value="ex">
    </property>
</bean>
```

#### 2.基于注解的异常处理

```java
//@ControllerAdvice将当前类标识为异常处理的组件
@ControllerAdvice
public class ExceptionController {
        //@ExceptionHandler用于设置所标识方法处理的异常
        @ExceptionHandler(ArithmeticException.class)
		//ex表示当前请求处理中出现的异常对象
        public String handleArithmeticException(Exception ex, Model model){
            model.addAttribute("ex", ex);
            return "error";
        }
}
```

## 四.springboot

#### 1.快速上手SpringBoot

##### 部署方式一

**步骤①**：创建新模块，选择Spring Initializr，并配置模块相关基础信息

**步骤②**：选择当前模块需要使用的技术集

以web开发为例，选择web→springweb

**步骤③**：开发控制器类

```java
@RestController
public class HelloController {
    @RequestMapping("/hello")
    public String hello() {
        return "Hello Spring Boot!";
    }
}
```

**步骤④**：启动入口类，然后去访问目标地址

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
//访问路径：	http://localhost:8080/books
```

##### 部署方式二

开发SpringBoot程序，可以不基于任意的IDE工具进行，其实在SpringBoot的官网里面就可以直接创建SpringBoot程序

##### 部署方式三

之前两种创建方式都需要访问国外的spring官网，但是如果国外地址被限制访问之后，阿里有个网站能提供这样的功能，用来构建项目

​	创建工程时，切换选择starter服务路径，然后手动输入阿里云提供给我们的使用地址即可。地址：http://start.aliyun.com或https://start.aliyun.com

阿里云地址默认创建的SpringBoot工程版本是<font color="#ff0000"><b>2.4.1</b></font>，所以如果你想更换其他的版本，创建项目后手工修改即可，别忘了刷新一下，加载新版本信息

##### 部署方式四

断网状态下部署springboot项目

1. 创建普通Maven工程
2. 继承spring-boot-starter-parent
3. 添加依赖spring-boot-starter-web
4. 制作引导类Application

##### Idea隐藏文件配置

步骤：

1. 【Files】→【Settings】
2. 【Editor】→【File Types】→【Ignored Files and Folders】
3. 输入要隐藏的名称，支持*号通配符
4. 回车确认添加

#### 2.SpringBoot简介

##### parent

​	<font color="#ff0000"><b>parent</b></font>自身具有很多个版本，每个<font color="#ff0000"><b>parent</b></font>版本中包含有几百个其他技术的版本号，不同的parent间使用的各种技术的版本号有可能会发生变化。当开发者使用某些技术时，直接使用SpringBoot提供的<font color="#ff0000"><b>parent</b></font>就行了，由<font color="#ff0000"><b>parent</b></font>帮助开发者统一的进行各种技术的版本管理

<font color="#ff0000"><b>parent</b></font>仅仅帮我们进行版本管理，它不负责帮你导入坐标，说白了用什么还是你自己定，只不过版本不需要你管理了。

整体上来说，<font color="#ff0000"><b>使用parent可以帮助开发者进行版本的统一管理</b></font>

```pom.xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.4</version>
</parent>
```

**总结**

1. 开发SpringBoot程序要继承spring-boot-starter-parent
2. spring-boot-starter-parent中定义了若干个依赖管理
3. 继承parent模块可以避免多个依赖使用相同技术时出现依赖版本冲突
4. 继承parent的形式也可以采用引入依赖的形式实现效果

##### starter

​	SpringBoot关注到开发者在实际开发时，对于依赖坐标的使用往往都有一些固定的组合方式，比如使用spring-webmvc就一定要使用spring-web。每次都要固定搭配着写，非常繁琐，而且格式固定，没有任何技术含量。

​	**starter**定义了使用某种技术时对于依赖的**固定搭配格式**，也是一种最佳解决方案，<font color="#ff0000"><b>使用starter可以帮助开发者减少依赖配置</b></font>

​	这个东西其实在入门案例里面已经使用过了，入门案例中的web功能就是使用这种方式添加依赖的。可以查阅SpringBoot的配置源码，看到这些定义

- 项目中的pom.xml定义了使用SpringMVC技术，但是并没有写SpringMVC的坐标，而是添加了一个名字中包含starter的依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

- 在spring-boot-starter-web中又定义了若干个具体依赖的坐标

```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-json</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>5.3.9</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.9</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

之前提到过开发SpringMVC程序需要导入spring-webmvc的坐标和spring整合web开发的坐标，就是上面这组坐标中的最后两个了。

**starter与parent的区别**

​	<font color="#ff0000"><b>starter</b></font>是一个坐标中定了若干个坐标，以前写多个的，现在写一个，<font color="#ff0000"><b>是用来减少依赖配置的书写量的</b></font>

​	<font color="#ff0000"><b>parent</b></font>是定义了几百个依赖版本号，以前写依赖需要自己手工控制版本，现在由SpringBoot统一管理，这样就不存在版本冲突了，<font color="#ff0000"><b>是用来减少依赖冲突的</b></font>

##### 引导类

​	目前程序运行的入口就是SpringBoot工程创建时自带的那个类了，带有main方法的那个类，运行这个类就可以启动SpringBoot工程的运行

```java
@SpringBootApplication
public class Springboot0101QuickstartApplication {
    public static void main(String[] args) {
        SpringApplication.run(Springboot0101QuickstartApplication.class, args);
    }
}
```

##### 内嵌tomcat

​	当前我们做的SpringBoot入门案例勾选了Spirng-web的功能，并且导入了对应的starter。

```XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**内嵌Tomcat定义位置**

​	打开查看web的starter导入了哪些东西

```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-json</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
        <version>2.5.4</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>5.3.9</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.9</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

​	第三个依赖就是这个tomcat对应的东西了，居然也是一个starter，再打开看看

```XML
<dependencies>
    <dependency>
        <groupId>jakarta.annotation</groupId>
        <artifactId>jakarta.annotation-api</artifactId>
        <version>1.3.5</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-core</artifactId>
        <version>9.0.52</version>
        <scope>compile</scope>
        <exclusions>
            <exclusion>
                <artifactId>tomcat-annotations-api</artifactId>
                <groupId>org.apache.tomcat</groupId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-el</artifactId>
        <version>9.0.52</version>
        <scope>compile</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-websocket</artifactId>
        <version>9.0.52</version>
        <scope>compile</scope>
        <exclusions>
            <exclusion>
                <artifactId>tomcat-annotations-api</artifactId>
                <groupId>org.apache.tomcat</groupId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

​	这里面有一个核心的坐标，tomcat-embed-core，叫做tomcat内嵌核心。就是这个东西把tomcat功能引入到了我们的程序中。

**内嵌Tomcat运行原理**

​	Tomcat服务器是一款软件，而且是一款使用java语言开发的软件，熟悉的小伙伴可能有印象，tomcat安装目录中保存有jar，好多个jar。

​	tomcat服务器运行其实是以对象的形式在Spring容器中运行的，怪不得我们没有安装这个tomcat，而且还能用。这东西最后是以一个对象的形式存在，保存在Spring容器中悄悄运行的。具体运行的是什么呢？其实就是上前面提到的那个tomcat内嵌核心

```XML
<dependencies>
    <dependency>
        <groupId>org.apache.tomcat.embed</groupId>
        <artifactId>tomcat-embed-core</artifactId>
        <version>9.0.52</version>
        <scope>compile</scope>
    </dependency>
</dependencies>
```

​	那既然是个对象，如果把这个对象从Spring容器中去掉是不是就没有web服务器的功能呢？是这样的，通过依赖排除可以去掉这个web服务器功能

```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```

​	上面对web-starter做了一个操作，使用maven的排除依赖去掉了使用tomcat的starter。这下好了，容器中肯定没有这个对象了，重新启动程序可以观察到程序运行了，但是并没有像之前那样运行后会等着用户发请求，而是直接停掉了，就是这个原因了。

**更换内嵌Tomcat**

​	那根据上面的操作我们思考是否可以换个服务器呢？必须的嘛。根据SpringBoot的工作机制，用什么技术，加入什么依赖就行了。SpringBoot提供了3款内置的服务器

- tomcat(默认)：apache出品，粉丝多，应用面广，负载了若干较重的组件

- jetty：更轻量级，负载性能远不及tomcat

- undertow：负载性能勉强跑赢tomcat

  想用哪个，加个坐标就OK。前提是把tomcat排除掉，因为tomcat是默认加载的。

```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```

​	现在就已经成功替换了web服务器，核心思想就是用什么加入对应坐标就可以了。如果有starter，优先使用starter。

**总结**

1. 内嵌Tomcat服务器是SpringBoot辅助功能之一
2. 内嵌Tomcat工作原理是将Tomcat服务器作为对象运行，并将该对象交给Spring容器管理
3. 变更内嵌服务器思想是去除现有服务器，添加全新的服务器

#### 3.SpringBoot基础配置

##### application.properties

SpringBoot通过配置文件application.properties就可以修改默认的配置，

```properties
server.port=80 //修改默认的启动端口
# 关闭运行日志图表（banner)
spring.main.banner-mode=off
# 设置运行日志的显示级别
logging.level.root=debug
#可以通过修改application.properties，修改访问的端口号和上下文路径
server.context-path=/test
```

<font color="#f0f"><b>温馨提示</b></font>

​	所有的starter中都会依赖下面这个starter，叫做spring-boot-starter。这个starter是所有的SpringBoot的starter的基础依赖，里面定义了SpringBoot相关的基础配置。

```JAVA
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <version>2.5.4</version>
    <scope>compile</scope>
</dependency>
```

**总结**

1. SpringBoot中导入对应starter后，提供对应配置属性
2. 书写SpringBoot配置采用关键字+提示形式书写

##### 配置文件分类

​	SpringBoot除了支持properties格式的配置文件，还支持另外两种格式的配置文件。分别如下:

- properties格式
- yml格式
- yaml格式


application.yml（yml格式）

```YML
server:
  port: 81
```

application.yaml（yaml格式）

```yaml
server:
  port: 82
```

yml和yaml文件格式就是一模一样的，只是文件后缀不同，所以可以合并成一种格式来看。以后基本上都是用yml格式的，以后在企业开发过程中用这个格式的机会也最多，一定要重点掌握。

**YAML**（YAML Ain't Markup Language），一种数据序列化格式。具有容易阅读.容易与脚本语言交互.以数据为核心，重数据轻格式的特点。常见的文件扩展名有两种：

- .yml格式（主流）

- .yaml格式

  对于文件自身在书写时，具有严格的语法格式要求，具体如下：

- 大小写敏感
- 属性层级关系使用多行描述，**每行结尾使用冒号结束**
- 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格（不允许使用Tab键）
- 属性值前面添加空格（属性名与属性值之间使用冒号+空格作为分隔）

上述规则不要死记硬背，按照书写习惯慢慢适应，并且在Idea下由于具有提示功能，慢慢适应着写格式就行了。核心的一条规则要记住，<font color="#ff0000"><b>数据前面要加空格与冒号隔开</b></font>

```xml
boolean: TRUE  						#TRUE,true,True,FALSE,false，False均可
float: 3.14    						#6.8523015e+5  #支持科学计数法
int: 123       						#0b1010_0111_0100_1010_1110    #支持二进制.八进制.十六进制
null: ~        						#使用~表示null
string: HelloWorld      			#字符串可以直接书写
string2: "Hello World"  			#可以使用双引号包裹特殊字符
date: 2018-02-17        			#日期必须使用yyyy-MM-dd格式
datetime: 2018-02-17T15:02:31+08:00  #时间和日期之间使用T连接，最后使用+代表时区
```

yaml格式中也可以表示数组，在属性名书写位置的下方使用减号作为数据开始符号，每行书写一个数据，减号与数据间空格分隔

```yml
enterprise:
	name: itcast
    age: 16
subject:
   - Java
   - 前端
   - 大数据
likes: [王者荣耀,刺激战场]			#数组书写缩略格式
```

##### yml的数据读取

##### 配置文件优先级

1. 配置文件间的加载优先级	properties（最高）>  yml  >  yaml（最低）
2. 不同配置文件中相同配置按照加载优先级相互覆盖，不同配置文件中不同配置全部保留 

##### springboot热部署

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional> <!-- 这个需要为 true 热部署才有效 -->
</dependency>
<plugin>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
```

Springboot提供了热部署的方式，当发现任何类发生了改变，马上通过JVM类加载的方式，加载最新的类到虚拟机中。 这样就不需要重新启动也能看到修改后的效果了

#### 4.基于SpringBoot进行整合

##### 整合JUnit

第一种方式使用属性的形式进行，在注解@SpringBootTest中添加classes属性指定配置类

```java
@SpringBootTest(classes = Springboot04JunitApplication.class)
class Springboot04JunitApplicationTests {
    //注入你要测试的对象
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        //执行要测试的对象对应的方法
        bookDao.save();
        System.out.println("two...");
    }
}
```

第二种方式回归原始配置方式，仍然使用@ContextConfiguration注解进行，效果是一样的

```java
@SpringBootTest
@ContextConfiguration(classes = Springboot04JunitApplication.class)
class Springboot04JunitApplicationTests {
    //注入你要测试的对象
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        //执行要测试的对象对应的方法
        bookDao.save();
        System.out.println("two...");
    }
}
```

<font color="#f0f"><b>温馨提示</b></font>

​		使用SpringBoot整合JUnit需要保障导入test对应的starter，由于初始化项目时此项是默认导入的，所以此处没有提及，其实和之前学习的内容一样，用什么技术导入对应的starter即可。

##### 整合MyBatis

整合步骤

**步骤①**：创建模块时勾选要使用的技术，MyBatis，由于要操作数据库，还要勾选对应数据库

以mysql为例，创建模块时候需要勾选**MysqlDriver**和**MybatisFramework**

**步骤②**：配置数据源相关信息

```xml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/how2java
    username: root
    password: root
```

**步骤③**：写具体的业务代码以及实体类，完成编码

```java
@Mapper
public interface BookDao {
    @Select("select * from tbl_book where id = #{id}")
    public Book getById(Integer id);
}
```

<font color="#ff0000"><b>注意</b></font>：当前使用的SpringBoot版本是2.5.4，对应的坐标设置中Mysql驱动使用的是8x版本。当SpringBoot2.4.3（不含）版本之前会出现一个小BUG，就是MySQL驱动升级到8以后要求强制配置时区，如果不设置会出问题。解决方案很简单，驱动url上面添加上对应设置就行了

```xml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/how2java?serverTimezone=UTC
    username: root
    password: root
```

​	这里设置的UTC是全球标准时间，你也可以理解为是英国时间，中国处在东八区，需要在这个基础上加上8小时，这样才能和中国地区的时间对应的，也可以修改配置不写UTC，写Asia/Shanghai也可以解决这个问题。

```xml
url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=Asia/Shanghai
```

##### 整合Druid

没有指定数据源的时候，springboot帮我们选了一个它认为最好的数据源对象，这就是HiKari。通过启动日志可以查看到对应的身影

```verilog
2023-09-27 10:55:51.329  INFO 26164 --- [ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown initiated...
2023-09-27 10:55:51.336  INFO 26164 --- [ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown completed.
```

如果需要更换数据源，其实只需要两步即可。

1. 导入对应的技术坐标
2. 配置使用指定的数据源类型

 **步骤①**：导入对应的坐标（注意，是坐标，此处不是starter）

```xml
<dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.16</version>
</dependency>
```

**步骤②**：修改配置，在数据源配置中有一个type属性，专用于指定数据源类型

```yml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: root
    type: com.alibaba.druid.pool.DruidDataSource
```

目前的数据源配置格式是一个通用格式，不管你换什么数据源都可以用这种形式进行配置。但是如果对数据源进行个性化的配置，例如配置数据源对应的连接数量，这个时候就有新的问题了。每个数据源技术对应的配置名称都不一样，此时就要使用专用的配置格式了。

**步骤①**：导入对应的starter

```XML
<dependencies>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid-spring-boot-starter</artifactId>
        <version>1.2.6</version>
    </dependency>
</dependencies>
```

**步骤②**：修改配置

```YAML
spring:
  datasource:
    druid:
      driver-class-name: com.mysql.cj.jdbc.Driver
      url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
      username: root
      password: root
```

​	注意观察，配置项中，在datasource下面并不是直接配置url这些属性的，而是先配置了一个druid节点，然后再配置的url这些东西。

Lombok，一个Java类库，提供了一组注解，简化POJO实体类开发，SpringBoot目前默认集成了lombok技术，并提供了对应的版本控制，所以只需要提供对应的坐标即可，在pom.xml中添加lombok的坐标。

```xml
<!--lombok-->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>
```

使用lombok可以通过一个注解@Data完成一个实体类对应的getter，setter，toString，equals，hashCode等操作的快速添加

```java
@Data
public class Book {
    private Integer id;
    private String type;
    private String name;
    private String description;
}
```

##### springBootJPA

JPA(Java Persistence API)是Sun官方提出的Java持久化规范，用来方便大家操作数据库。真正干活的可能是Hibernate,TopLink等等实现了JPA规范的不同厂商,默认是Hibernate。


