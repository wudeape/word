##  Hibernate和映射文件的简单配置
   # Hibernate是一个彻底的ORM(Object Relational Mapping,对象关系映射)开源框架
   其中PO=POJO+映射文件

   根据体系结构视图可以了解到整个利用Hibernate框架实现的项目包括整个重要的配置文件：
Hibernate配置文件：实现Hibernate基础配置，是Hibernate能够友好的与DB进行交互基础；
实现Hibernate基础配置，是Hibernate能够友好的与DB进行交互基础；
开发时放置src目录下,取名为：hibernate.cfg.xml(hibernate.properties)

   Hibernate映射文件：实现POJO与DB表格的映射配置；
 为了维护方便一般将其放置和相对应的POJO同一目录下，取名为POJOName.hbm.xml。
虽然一个映射文件中可以配置多个POJO与数据库表的映射关系但是还是建议
一个映射文件中只配置一个POJO与数据库表的映射关系

 ## 配置文件详解
   Hibernate配置文件有两种形式：XML与properties

   XML（hibernate.cfg.xml）文件详解
 <?xml version="1.0" encoding="GBK"?>  
<!-- 指定Hibernate配置文件的DTD信息 -->  
<!DOCTYPE hibernate-configuration PUBLIC  
    "-//Hibernate/Hibernate Configuration DTD 3.0//EN"  
    "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">  
<!-- hibernate- configuration是连接配置文件的根元素 -->  
<hibernate-configuration>  
    <session-factory>  
        <!-- 指定连接数据库所用的驱动 -->  
        <property name="connection.driver_class">com.mysql.jdbc.Driver</property>  
        <!-- 指定连接数据库的url，hibernate连接的数据库名 -->  
        <property name="connection.url">jdbc:mysql://localhost/数据库名</property>  
        <!-- 指定连接数据库的用户名 -->  
        <property name="connection.username">root</property>  
        <!-- 指定连接数据库的密码 -->  
        <property name="connection.password">32147</property>  
        <!-- 指定连接池里最大连接数 -->  
        <property name="hibernate.c3p0.max_size">20</property>  
        <!-- 指定连接池里最小连接数 -->  
        <property name="hibernate.c3p0.min_size">1</property>  
        <!-- 指定连接池里连接的超时时长 -->  
        <property name="hibernate.c3p0.timeout">5000</property>  
        <!-- 指定连接池里最大缓存多少个Statement对象 -->  
        <property name="hibernate.c3p0.max_statements">100</property>  
        <property name="hibernate.c3p0.idle_test_period">3000</property>  
        <property name="hibernate.c3p0.acquire_increment">2</property>  
        <property name="hibernate.c3p0.validate">true</property>  
        <!-- 指定数据库方言 -->  
        <property name="dialect">org.hibernate.dialect.MySQLInnoDBDialect</property>  
        <!-- 根据需要自动创建数据表 -->  
        <property name="hbm2ddl.auto">update</property>  
        <!-- 显示Hibernate持久化操作所生成的SQL -->  
        <property name="show_sql">true</property>  
        <!-- 将SQL脚本进行格式化后再输出 -->  
        <property name="hibernate.format_sql">true</property>  
        <!-- 罗列所有的映射文件 -->  
        <mapping resource="映射文件路径/News.hbm.xml"/>  
    </session-factory>  
</hibernate-configuration> 


 ## 映射文件详解 
    <?xml version="1.0"?>  
<!DOCTYPE hibernate-mapping PUBLIC   
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"  
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">      
<!--   
    <hibernate-mapping>一般不去配置，采用默认即可。  
    schema:指定映射数据库的schema(模式/数据库)，如果指定该属性，则表名会自动添加该schema前缀  
    package:指定包前缀 指定持久化类所在的包名 这样之后calss子元素中就不必使用全限定性的类名  
    default-cascade="none"：默认的级联风格，表与表联动。  
    default-lazy="true"：默认延迟加载  
 -->  
<hibernate-mapping>  
    <!--   
        <class>：使用class元素定义一个持久化类。  
        name="cn.javass.user.vo.UserModel"：持久化类的java全限定名；  
        table="tbl_user"：对应数据库表名，默认持久化类名作为表名；  
        proxy:指定一个接口，在延迟装载时作为代理使用，也可在这里指定该类自己的名字。  
        mutable="true"：默认为true，设置为false时则不可以被应用程序更新或删除，等价于所有<property>元素的update属性为false，表示整个实例不能被更新。  
        dynamic-insert="false"：默认为false，动态修改那些有改变过的字段，而不用修改所有字段；  
        dynamic-update="false"：默认为false，动态插入非空值字段；  
        select-before-update="false"：默认为false，在修改之前先做一次查询，与用户的值进行对比，有变化都会真正更新；  
        optimistic-lock="version"：默认为version(检查version/timestamp字段)，取值：all(检查全部字段)、dirty(只检查修改过的字段)；  
                                   none(不使用乐观锁定)，此参数主要用来处理并发，每条值都有固定且唯一的版本，版本为最新时才能执行操作；  
        如果需要采用继承映射，则class元素下还会增加<subclass.../>元素等用于定义子类。  
     -->  
    <class name="cn.javass.user.vo.UserModel" table="tbl_user" >  
          
        <!--   
            <id>：定义了该属性到数据库表主键字段的映射。  
            type  指定该标识属性的数据类型，该类型可以是Hibernate的内建类型，也可以是java类型，如果是java类型则需要使用全限定类名（带包名）。该属性可选，如果没有指定类型， 则hibernate自行判断该标识属性数据类型。通常建议设定。  
            name="userId"：标识属性的名字；  
            column="userId"：表主键字段的名字，如果不填写与name一样；  
              
         -->  
        <id name="userId">  
            <!-- <generator>：指定主键由什么生成，推荐使用uuid，assigned指用户手工填入。设定标识符生成器  
            适应代理主键的有：  
                increment：有Hibernat自动以递增的方式生成标识符，每次增量1；  
                identity：由底层数据库生成标识符，前提条件是底层数据库支持自动增长字段类型。（DB2,MYSQL）  
                uuid:用128位的UUID算法生成字符串类型标识符。  
            适应自然主键：  
                assigned:由java程序负责生成标识符，为了能让java应用程序设置OID,不能把setId（）方法设置成private类型。  
                让应用程序在save()之前为对象分配一个标识符。相当于不指定<generator.../>元素时所采用的默认策略。  
                应当尽量避免自然主键  
            -->  
            <generator class="uuid"/>  
        </id>  
          
        <!--   
            <version/>：使用版本控制来处理并发，要开启optimistic-lock="version"和dynamic-update="true"。  
            name="version"：持久化类的属性名，column="version"：指定持有版本号的字段名；  
         -->  
        <version name="version" column="version"/>  
          
        <!--   
            <property>：为类定义一个持久化的javaBean风格的属性。  
            name="name"：标识属性的名字，以小写字母开头；  
            column="name"：表主键字段的名字，如果不填写与name一样；  
            update="true"/insert="true"：默认为true，表示可以被更新或插入；  
            access="property/field"：指定Hibernate访问持久化类属性的方式。默认property。property表示使用setter/getter方式。field表示运用java反射机制直接访问类的属性。  
            formula="{select。。。。。}"：该属性指定一个SLQ表达式，指定该属性的值将根据表达式类计算，计算属性没有和它对应的数据列。  
            formula属性允许包含表达式：sum,average,max函数求值的结果。  
            例如：formula="(select avg(p.price) from Product P)"  
         -->  
        <property name="name" column="name" />  
        <property name="sex" column="sex"/>  
        <property name="age" column="age"/>  
          
        <!--   
            组件映射：把多个属性打包在一起当一个属性使用，用来把类的粒度变小。  
            <component name="属性，这里指对象">  
                <property name="name1"></property>  
                <property name="name2"></property>  
            </component>  
         -->  
           
         <!--   
            <join>:一个对象映射多个表，该元素必须放在所有<property>之后。  
            <join table="tbl_test：子表名">  
                <key column="uuid：子表主键"></key>  
            <property name="name1：对象属性" column="name：子表字段"></property>  
         </join>  
          -->  
           
    </class>    
</hibernate-mapping>


##关于HQL 语言的学习
  关于HQL的一些认识：
 HQL(Hibernate Query Language)Hibernate查询语言，语法类似于SQL，可以直接使用实体类及属性
     使用HQL 可以避免使用JDBC 查询的一些弊端
不需要再编写繁复的SQL 语句，针对实体类及其属性进行查询
 查询结果是直接存放在List 中的对象，不需要再次封装
独立于数据库，对不同的数据库根据Hibernate dialect 属性的配置自动生成不同的SQL 语句执行
  
 