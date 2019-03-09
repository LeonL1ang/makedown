## iBatis 操作手册









##### spring整合iBatis

1. 准备pom文件

   ```xml
   <properties>
   		<spring.version>4.0.9.RELEASE</spring.version>
   	</properties>
   
   	<dependencies>
   		<dependency>
   			<groupId>c3p0</groupId>
   			<artifactId>c3p0</artifactId>
   			<version>0.9.1.2</version>
   		</dependency>
   		<dependency>
   			<groupId>junit</groupId>
   			<artifactId>junit</artifactId>
   			<version>4.12</version>
   			<scope>test</scope>
   		</dependency>
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-test</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-aop</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-webmvc</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-web</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-beans</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-context</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-jdbc</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-tx</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-core</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-context</artifactId>
   			<version>${spring.version}</version>
   		</dependency>
   		<dependency>
   			<groupId>org.apache.ibatis</groupId>
   			<artifactId>ibatis-sqlmap</artifactId>
   			<version>2.3.4.726</version>
   		</dependency>
   		<dependency>
   			<groupId>org.springframework</groupId>
   			<artifactId>spring-ibatis</artifactId>
   			<version>2.0.8</version>
   		</dependency>
   		<dependency>
   			<groupId>springframework</groupId>
   			<artifactId>spring-orm</artifactId>
   			<version>1.1.3</version>
   		</dependency>
   		<dependency>
   			<groupId>mysql</groupId>
   			<artifactId>mysql-connector-java</artifactId>
   			<version>5.1.6</version>
   		</dependency>
   </dependencies>
   ```

   

2. 配置环境

   `application.xml`

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
       http://www.springframework.org/schema/beans  http://www.springframework.org/schema/beans/spring-beans.xsd 
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/tx  http://www.springframework.org/schema/tx/spring-tx.xsd
       http://www.springframework.org/schema/aop  http://www.springframework.org/schema/aop/spring-aop.xsd"
       default-autowire="byName">
   
       <context:component-scan base-package="wiki.leon.demo" />
       
   	<bean id="sqlMapClient" class="org.springframework.orm.ibatis.SqlMapClientFactoryBean">  
   	  <property name="configLocation" value="classpath:sqlMapConfig.xml"/>
   	</bean>
   	
   	 <bean id="sqlMapClientTemplate" class="org.springframework.orm.ibatis.SqlMapClientTemplate">  
           <property name="sqlMapClient" ref="sqlMapClient"></property> 
       </bean>
       
   	 <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
               <!-- 加载jdbc驱动 -->
               <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
               <!-- jdbc连接地址 -->
               <property name="jdbcUrl" value="jdbc:mysql://47.101.221.215:3306/blog?useUnicode=true&amp;characterEncoding=utf-8"></property>
               <!-- 连接数据库的用户名 -->
               <property name="user" value="root"></property>
               <!-- 连接数据库的密码 -->
               <property name="password" value="blogroot"></property>
               <!-- 数据库的初始化连接数 -->
               <property name="initialPoolSize" value="3"></property>
               <!-- 数据库的最大连接数 -->
               <property name="maxPoolSize" value="10"></property>
               <!-- 数据库最多执行的事务 -->
               <property name="maxStatements" value="100"></property>
               <!-- 连接数量不够时每次的增量 -->
               <property name="acquireIncrement" value="2"></property>           
           </bean>
           <!--  创建jdbcTemplate对象 -->
           <!-- <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
               <property name="dataSource" ref="dataSource">
               </property>
           </bean> -->
   	<!-- transactionManager不是必需  -->
   	    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
   	        <property name="dataSource" ref="dataSource"/>
   	    </bean>
   </beans>
   ```

   

`sqlMapConfig.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE sqlMapConfig PUBLIC "-//ibatis.apache.org//DTD SQL Map Config 2.0//EN" 
    "http://ibatis.apache.org/dtd/sql-map-config-2.dtd">

<sqlMapConfig>
	<settings   useStatementNamespaces="true"   />
    <!-- 这里无需配置sqlMap,可以通过spring的配置加载某个目录的多个SqlMap.xml-->
    <sqlMap resource="user_sqlmap.xml"/>
    
</sqlMapConfig>
```



`user_sqlmap.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE sqlMap PUBLIC "-//ibatis.apache.org//DTD SQL Map 2.0//EN" 
    "http://ibatis.apache.org/dtd/sql-map-2.dtd">
    
<sqlMap namespace="userDao">
 
  <typeAlias type="wiki.leon.demo.ibatis.pojo.User" alias="user"/> 
  
  <resultMap id="userMap" class="user" >  
    <result column="id" property="id" jdbcType="INTEGER" />
    <result column="email" property="email" jdbcType="INTEGER" />
    <result column="password" property="password" jdbcType="INTEGER" />
    <result column="profile" property="profile" jdbcType="INTEGER" />
    <result column="name" property="name" jdbcType="VARCHAR" />
    <result column="head_img" property="headImg" jdbcType="VARCHAR" />
    <result column="create_time" property="createTime" jdbcType="datetime" />
    <result column="update_time" property="updateTime" jdbcType="datetime" />
    <result column="login_time" property="loginTime" jdbcType="datetime" />
  </resultMap>  
    
  <!-- 获得全查询列表 -->  
  <select id="selectOne" resultMap="userMap">
    select * from tb_user where id = #id#
  </select> 
  
  <insert id="insert">
  	insert into tb_user (email, password , profile, name, head_img, create_time, update_time, login_time)
  	values(#email#, #password# , #profile#, #name#, #headImg#, #createTime#, #updateTime#, #loginTime#)
  </insert>
  
  <update id="update">
  	update
  </update>
  
  <select id="selectAll" resultMap="userMap">
  	select * from tb_user
  </select>
  
  <delete id="deleteById" parameterClass="Integer">
  	delete from tb_user where id = #id#
  </delete>
  
</sqlMap>  

```



3. 测试

   `BaseTest.java`

   ```java
   package wiki.leon.demo.test;
   
   import java.util.Date;
   import java.util.List;
   
   import org.junit.Test;
   import org.junit.runner.RunWith;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.test.context.ContextConfiguration;
   import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
   
   import wiki.leon.demo.ibatis.dao.UserDao;
   import wiki.leon.demo.ibatis.pojo.User;
   
   @RunWith(SpringJUnit4ClassRunner.class) //使用junit4进行测试  
   @ContextConfiguration(locations={"classpath:application.xml"}) //加载配置文件   
   //------------如果加入以下代码，所有继承该类的测试类都会遵循该配置，也可以不加，在测试类的方法上///控制事务，参见下一个实例    
   //这个非常关键，如果不加入这个注解配置，事务控制就会完全失效！    
   //@Transactional    
   //这里的事务关联到配置文件中的事务控制器（transactionManager = "transactionManager"），同时//指定自动回滚（defaultRollback = true）。这样做操作的数据才不会污染数据库！    
   //@TransactionConfiguration(transactionManager = "transactionManager", defaultRollback = true)    
   public class BaseTest {
   	
   	@Autowired
   	UserDao<User> UserDao;
   	
   	@Test
   	public void insert(){
   		User user = new User();
   		Date date = new Date();
   		user.setCreateTime(date);
   		user.setEmail("l@qq.com");
   		user.setHeadImg("我的头像。jpg");
   		user.setLoginTime(date);
   		user.setCreateTime(date);
   		user.setPassword("+++++****");
   		user.setUpdateTime(date);
   		user.setName("Leon");
   		System.out.println(UserDao.insert(user));
   		System.out.println("_______________________________________");
   	}
   	
   	@Test
   	public void selectOne() {
   		
   		System.out.println(UserDao.selectOne(2));
   		System.out.println("_________________________________________");
   		
   	}
   
   	
   	@Test
   	public void selectAll() {
   		List<User> list = UserDao.selectAll();
   		for(User u : list) {
   			System.out.println(u);
   		}
   		System.out.println("_________________________________________");
   		
   	}
   	
   	@Test
   	public void delectById() {
   		System.out.println(UserDao.deleteById(1));
   	}
   }
   
   ```

   