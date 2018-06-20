### MySQL

***

数据库用来创建，访问，管理，搜索和复制所需要的数据，比如在前端代码里加入了注册的数据，而前端向后端传入的数据可以保存在数据库中。



可以数据mysql链接到MySQL的服务器上

关系数据库管理系统的特点：

​	1、数据都已表格的形式出现

​	2、每行为各种记录名称

​	3、每列为数据名称所对应的数据域

​	4、许多的行和列组成一个表

​	5、若干的表单组成database



冗余：存储两倍的数据，冗余降低了性能，但是提高的数据的安全性。

主键：逐渐唯一，用来差数据

外检：用来关联两个表

符合键：将多个列作为一个索引键



mysql密码：常用！！

如果忘记密码，可以修改my.cnf文件添加skip-grant-tables来重置密码。



进入mysql 

mysql -u root -p



```
root@host# mysql -u root -p
Enter password:*******
mysql> use mysql;
Database changed

mysql> INSERT INTO user 
          (host, user, password, 
           select_priv, insert_priv, update_priv) 
           VALUES ('localhost', 'guest', 
           PASSWORD('guest123'), 'Y', 'Y', 'Y');
Query OK, 1 row affected (0.20 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 1 row affected (0.01 sec)

mysql> SELECT host, user, password FROM user WHERE user = 'guest';
+-----------+---------+------------------+
| host      | user    | password         |
+-----------+---------+------------------+
| localhost | guest | 6f8c114b58f2ce9e |
+-----------+---------+------------------+
1 row in set (0.00 sec)
```

执行FLUSH PRIVILEGES语句的时候，会重新载入授权表



管理数据库的常用的命令：

- USE 数据库名称      选择需要操作的数据库
- SHOW DATABASES    列出数据库系统的数据库列表
- SHOW TABLES     显示制定数据库的所有的表
- SHOW COLUMNS FROM 线束数据表的属性等具体信息
- SHOW INDEX FROM 数据表  显示数据表详细的索引信息

一般先进入库之中，在对表进行操作。





------

进行数据库的连接：

PHP使用mysql_connect()进行连接数据库

```
mysqli_connect(host,username,password,dbname,port,socket);
```

成功返回连接标识符，失败返回FALSE

如果想要断开连接

```
bool mysqli_close ( mysqli $link )
```

一般不会使用close  已经打开的非持久的连接会在脚本执行完毕后自动关闭。





- PHP连接数据库：

```php
<?php
$dbhost = 'localhost:3306';  // mysql服务器主机地址
$dbuser = 'root';            // mysql用户名
$dbpass = '123456';          // mysql用户名密码
$conn = mysqli_connect($dbhost, $dbuser, $dbpass);
if(! $conn )
{
    die('Could not connect: ' . mysqli_error());
}
echo '数据库连接成功！';
mysqli_close($conn);
?>

```

- 创建数据库

  - 可以直接使用命令来进行创建

    ```create DATABASE RUNOOB;
    create DATABASE RUNOOB;
    ```

  - 可以使用PHP脚本

    ```
    mysqli_query(connection,query,resultmode);
    - 第一个是需要的数据库连接
    - 第二个是查询的字符串
    - 第三个是选择量：
        MYSQLI_USE_RESULT（如果需要检索大量数据，请使用这个）
        MYSQLI_STORE_RESULT（默认）
    ```

- 删除数据库

  - drop databse +数据库名字

JDBC库是一个规范的可移植的底层数据库程序

JDBC API提供应用程序对JDBC的管理连接

JDBC Driver API 支持JDBC管理到驱动的连接



![img](http://www.yiibai.com/uploads/images/201706/0206/392080659_56700.jpg)



构建JDBC应用程序六大步骤

- **导入包**：import java.sql.*

- **注册JDBC驱动程序**：需要初始化驱动程序，以便可以打开与数据库的通信通道。

- **打开一个连接** ：需要使用类型为DriverManager.getConnection() 方法创建一个Connection对象，表示与数据库的物理连接

- **执行查询** ：需要使用类型为Statement的对象来构建和提交SQL语句到数据库

- **从结果中提取数据** ：需要使用相应的`ResultSet.getXXX()`方法从结果集中检索数据。

- **清理环境**：需要明确地关闭所有数据库资源，而不依赖于JVM的垃圾收集。

  实例代码：

  ```java
  
  //STEP 1. Import required packages
  import java.sql.*;
  
  public class FirstExample {
     // JDBC driver name and database URL
     static final String JDBC_DRIVER = "com.mysql.jdbc.Driver";  
     static final String DB_URL = "jdbc:mysql://localhost/emp";
  
     //  Database credentials
     static final String USER = "root";
     static final String PASS = "123456";
  
     public static void main(String[] args) {
     Connection conn = null;
     Statement stmt = null;
     try{
        //STEP 2: Register JDBC driver
        Class.forName("com.mysql.jdbc.Driver");
  
        //STEP 3: Open a connection
        System.out.println("Connecting to database...");
        conn = DriverManager.getConnection(DB_URL,USER,PASS);
  
        //STEP 4: Execute a query
        System.out.println("Creating statement...");
        stmt = conn.createStatement();
        String sql;
        sql = "SELECT id, first, last, age FROM Employees";
        ResultSet rs = stmt.executeQuery(sql);
  
        //STEP 5: Extract data from result set
        while(rs.next()){
           //Retrieve by column name
           int id  = rs.getInt("id");
           int age = rs.getInt("age");
           String first = rs.getString("first");
           String last = rs.getString("last");
  
           //Display values
           System.out.print("ID: " + id);
           System.out.print(", Age: " + age);
           System.out.print(", First: " + first);
           System.out.println(", Last: " + last);
        }
        //STEP 6: Clean-up environment
        rs.close();
        stmt.close();
        conn.close();
     }catch(SQLException se){
        //Handle errors for JDBC
        se.printStackTrace();
     }catch(Exception e){
        //Handle errors for Class.forName
        e.printStackTrace();
     }finally{
        //finally block used to close resources
        try{
           if(stmt!=null)
              stmt.close();
        }catch(SQLException se2){
        }// nothing we can do
        try{
           if(conn!=null)
              conn.close();
        }catch(SQLException se){
           se.printStackTrace();
        }//end finally try
     }//end try
     System.out.println("There are so thing wrong!");
  }//end main
  }//end FirstExample
  
  ```

  注册方式有两个，第一个是forname（）进行驱动注册

  第二个是使用静态方法进行注册程序  **DriverManager.registerDriver()**

  



注册完之后要建立连接，可以使用DriverManager.getConnection()的方法建立连接，

三个重载的方法 

- getConnection（String url）

- getConnection（String url，Properties prop）

- getConnection （String url，String user， String password）

  三种方式的url的格式不相同

MySQL 驱动名称 com.mysql.jdbc .Driver url格式：jdbc:mysql://hostname/databaseName

与数据库建立连接之后就可以访问数据库中的内statement等 每次使用这个Statement时需要先对Connection进行函数的调用创建一个Statement  然后用Statement执行SQL语句，一共可以执行三个语句

- boolean execute （String SQL） 可以使用执行创建数据库，创建表
- int executeUpdate （String SQL）返回收SQL语句影响的行数
- ResultSet executeQuery （String SQL）返回一个RS对象。

同样当使用完之后应该使用close（）函数进行关闭



三个statement一样，或者说类似 都需要对connection进行create 之后获得state 对其中保存的数据进行操作，读取的函数都是 **getxxx** 类型的。结果集完全取决于statement

四种结果集：

- 最基本的RS

  ​	只有查询结果的存储功能，并且不能来回的滚动读取。

  ​	创建方法：

  ​		Statement st = conn.CreateStatement

  ​		ResultSet rs = Statemnt.executeQuery(sqlStr);//存疑，st.Statement.executeQuary?

  ​	如果获得了这样的数据集 只能用next()方法进行读取，**不能回滚！！**

- 可回滚的RS

   	支持next()、previous()、first()回到第一行、absolute（int）跳转到第几行、relative（int n）相对当前第几行

  ​	创建方式：

  ​		Statement st = conn.createStatement(int resultSetType,int resultSetConcurrency)

  ​		ResultSet rs = st.executeQuery(sqlStr)

  ​	statement 创建中的两个参数一个是设置对象是否可以滚动

  ​		- ResultSet.TYPE_FORWARD_ONLY 只能向前滚动 

  ​		- ResultSet.TYPE_SCROLL_INSENSITIVE 和 Result.TYPE_SCROLL_SENSITIVE 这两个方都				

  ​		  能够实现 任意的前后滚动，使用各种移动的 ResultSet  指针的方法。

  ​    		  二者的区别在于者对于修改不敏感，而后者对于修改敏感。 

- 可更新的RS

  Statement st = createstatement(Result.TYPE_SCROLL_INSENSITIVE,Result.CONCUR_UPDATABLE) 

- 可保持的RS

  ​	每一次的查询的时候只能保持一个RS为打开状态，而使用可保持状态的RS则可以在打开一个RS的时候同时打开另一个RS进行数据的查询。