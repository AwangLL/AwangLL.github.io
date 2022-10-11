# JDBC

JDBC的全称是Java数据库连接(Java Database Connectivity )，它是一套用于执行SQL语句的Java API。应用程序可通过这套Jav API连接到关系数据库，并使用SQLa语句完成对数据库中数据的查询、新增、更新和删除等操作。使用JDBC连接数据库驱动，用户就不必用户不必编写Java程序与数据库交互的底层代码，使得代码的通用性更强。

## Driver接口

Driver接口是所有JDBC驱动程序必须实现的接口，该接口专门提供给数据库厂商使用。需要注意的是，在编写JDBC程序时，必须要把所使用的数据库驱动程序(这里指MySQL驱动JAR包)或类库加载到项目的classpath中。

## DriverManager类

使用JDBC连接数据库，需要用到DriverManager类，DriverManager类用于加载JDBC驱动并且创建JDBC程序与数据库的连接。在DriverManager类中，定义了两个比较重要的静态方法，如下所示。

|方法声明|功能描述|
|---|---|
|registerDriver(Driver driver)|该方法用于向DriverManager中注册给定的JDBC驱动程序
|getConnection(String url,String user,String password)|该方法用于建立和数据库的连接，并返回表示连接的Connection对象

## Connection接口

DriverManager类的getConnection()方法返回了一个Connection对象，Connection对象是表示数据库连接的对象，只有获得该连接对象，才能访问并操作数据库。一个应用程序可与单个数据库建立一个或多个连接，也可以与多个数据库建立连接。Connection接口的常用方法如下所示。

|方法声明|功能描述|
|---|---|
|createStatement()|用于创建一个Statement对象，Statement对象可以将SQL语句发送到数据库
|prepareStatement(String sql|用于创建一个PreparedStatement对象，该对象可以将参数化的动态SQL语句发送到数据库
|prepareCall(String sql)|用于创建一个CallableStatement对象来调用数据库的存储过程
|isReadOnly()|用于查看当前Connection对象的读取模式是否为只读模式
|setReadOnly()|用于设置当前Connection对象的读写模式，默认是非只读模式
|commit()|使所有上一次提交/回滚后进行的更改成为持久更改，并释放此Connection对象当前持有的所有数据库锁
|setAutoCommit(boolean autoCommit)|设置是否关闭自动提交模式
|roolback()|用于取消在当前事务中进行的所有更改，并释放此Connection对象当前持有的所有数据库锁
|close()|用于立即释放Connection对象的数据库和JDBC资源，而不是等它们被自动释放
|isClose()|用于判断Connection对象是否已被自动关闭

## Statement接口

Statement接口用于执行静态的SQL语句，并返回一个结果对象。Statement接口对象可以通过Connection实例的createStatement()方法获得，该对象会把静态的SQL语句发送到数据库中编译执行，然后返回数据库的处理结果。Statement接口提供了3个常用的执行SQL语句的方法，如下所示。

|方法声明|功能描述|
|---|---|
|execute(String sql)|用于执行各种SQL语句。该方法返回一个boolean类型的值，如果返回值为true，表示所执行的SQL语句有查询结果，可以通过Statement的getResultSet()方法获得查询结果。
|executeUpdate(String sql)|用于执行SQL中的insert、update和delete语句。该方法返回一个int类型的值，表示数据库中受该SQL语句影响的条数。
|executeQuery(String sql)|用于执行SQL中的select语句。该方法返回一个表示查询结果的ResultSet对象。

## PreparedStatement接口

PreparedStatement是Statement的子接口，用于执行预编译的SQL语句。
PreparedStatement接口扩展了带有参数SQL语句的执行操作，该接口中的SQL语句可以使用占位符?代替参数，然后通过setter方法为SQL语句的参数赋值。PreparedStatement接口提供的常用方法，如下所示。

|方法声明|功能描述|
|---|---|
|executeUpdate()|在PreparedStatement对象中执行SQL语句，SQL语句必须是一个DML语句或者是无返回内容的SQL语句，如DDL语句。
|executeQuery)|在PreparedStatement对象中执行SQL查询，该方法返回的是ResultSet对象。
|setInt(int parameterIndex, int x)|将指定参数设置成给定的int值。
|setFloat(int index,float f)|将指定位置的参数设置为float值。
|setLong(int index,long l)|将指定位置的参数设置为long值。
|setDouble(int index,double d)|将指定位置的参数设置为double值。
|setBoolean(int index,boolean b)|将指定位置的参数设置为boolean值。
|void setString(int parameterIndex,String x)|将指定参数设置成给定的String值。

## ResultSet接口

ResultSet接口用于保存JDBC执行查询时返回的结果集，该结果集封装在一个逻辑表格中。在ResultSet接口内部有一个指向表格数据行的游标(或指针)，ResultSet对象初始化时，游标在表格的第一行之前，调用next()方法可以将游标移动到下一行。如果下一行没有数据，则next()方法返回false。在应用程序中经常使用next()方法作为while循环的条件来迭代ResultSet结果集。ResultSet接口的常用方法，如下所示。

|方法声明|功能描述|
|---|---|
|getString(int columnIndex)|用于获取指定字段的String类型的值，参数columnIndex代表字段的索引
|getString(String columnName)|用于获取指定字段的String类型的值，参数columnName代表字段的名称
|getInt(int columnIndex)|用于获取指定字段的int类型的值，参数columnIndex代表字段的索引
|getInt(String columnName)|用于获取指定字段的int类型的值，参数columnName代表字段的名称
|absolute(int row)|将游标移动到结果集的第row条记录
|relative(int row)|按相对行数(正或负)移动游标
|previous()|将游标从结果集的当前位置移动到上一行
|next()|将游标从结果集的当前位置移动到下一行
|beforeFirst()|将游标移动到结果集的开头(第一行之前)
|isBeforeFirst()|判断游标是否位于结果集的开头(第一行之前)
|afterLast()|将游标指针移动到结果集的末尾(最后一行之后
|isAfterLast()|判断游标是否位于结果集的末尾(最后一行之后
|first()|将游标移动到结果集的第一行
|isFirst()|判断游标是否位于结果集的第一行
|last()|将游标移动到结果集的最后一条记录
|getRow()|返回当前记录的行号
|getStatement()|返回生成结果集的Statement对象
|close()|释放当前结果集的数据库和JDBC资源

## JDBC编程

使用JDBC的常用API实现JDBC程序的步骤如下所示

![](static/image/Pasted%20image%2020221011171352.png)

### 1. 加载并注册数据库驱动程序

在连接数据库之前，要加载数据库的驱动到JVM。加载数据库驱动操作可以通过java.lang.Class类的静态方法forName(String className)或DriverManager类的静态方法registerDriver(Driver driver)实现。

调用registDriver()方法时，其实会创建了两个Driver对象，对于只需加载驱动类来讲有些浪费资源;使用forName()方法的话，驱动程序类的名称是以字符串的形式填写，使用时可以把该驱动类名称放到配置文件中，如果需要切换驱动类会非常方便。所以在实际开发中，常调用forName()方法注册数据库驱动。

```java
// 以mysql为例
// MySQL 6.0.2之前版本
Class.forName("com.mysql.jdbc.Driver");
// MySQL 6.0.2之前版本
Class.forName("com.mysql.cj.jdbc.Driver");
```

### 2. 通过DriverManager获取数据库连接

```java
Connection conn = DriverManager.getConnection(String url, String user, String pwd);
// url: 连接数据库的URL
// user: 登录数据库的用户名
// pwd: 登录数据库的密码
```

url的书写格式：`jdbc:mysql://hostname:port/databasename`，其中jdbc:mysql:是固定的写法，后面的hostname指的是主机的名称，port指的是连接数据库的端口号(MySQL端口号默认为3306 ) ，databasename指的是MySQL中相应数据库的名称。

### 3. 通过connection对象获取Statement对象

获取Connetction对象之后，还必须要创建一个Statement对象，将各种SQL语句发送到所连接的数据库中执行。如果把Connection对象看作一条连接程序和数据库的索道，那么Statement对象就可以看作是索道上的一辆缆车，它为数据库传输SQL语句，并返回执行结果。

Connection创建Statement对象的方法∶
- createStatement():创建基本的Statement对象。
- prepareStatement():根据传递的SQL语句创建PreparedStatement对象。
- prepareCall():根据传入的SQL语句创建CallableStatement对象。

```java
// 创建基本的Statement对象
Statement stmt = conn.createStatement();
```

### 4. 使用Statement执行SQL语句

创建了Statement对象后，就可以通过该对象执行SQL语句。如果SQL语句运行后产生了结果集，Statement对象会将结果集封装成ResultSet对象并返回。Statement有以下3个执行SQL语句的方法。
- execute():可以执行任何SQL语句。
- executeQuery()∶通常执行查询语句，执行后返回代表结果集的ResultSet对象。
- executeUpdate():主要用于执行DML语句和DDL语句。执行DML语句，如INSERT、UPDATE或DELETE时，返回受SQL语句影响的行数;执行DDL语句返回0。

### 5. 操作结果集

如果执行的SQL语句是查询语句，执行结果将返回一个ResultSet对象，该对象保存了SQL语句查询的结果。程序可以通过操作该ResultSet对象取出查询结果。

### 6. 关闭连接，释放资源

每次操作数据库结束后都要关闭数据库连接并释放资源，以防止系统资源浪费。资源的关闭顺序和声明顺序相反，关闭顺序依次为关闭结果集ResultSet、关闭Statement对象、关闭Connection对象。为了保证在异常情况下也能关闭资源，通常在try...catch语句的finally代码块中统一关闭资源。

## 示例

