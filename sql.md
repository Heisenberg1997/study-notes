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

  