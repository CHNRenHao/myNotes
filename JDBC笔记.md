

![jdbc原理](E:\笔记\JDBC笔记.assets\jdbc原理.png)



# 编程6步

1. 注册驱动        告诉Java程序，即将要连接的是哪个品牌的数据库
2. 获取连接        表示JVM的进程和数据库进程之间的通信打开了，这属于进程之间的通信，使用完之后一定要关闭通道
3. 获取数据库操作对象       专门执行sql语句的对象
4. 执行sql语句       DQL  DML
5. 处理查询结果集         只有当第四步执行的是select语句的时候，才有第五步处理查询结果集
6. 释放资源        使用完资源之后一定要关闭资源，java和数据库属于进程间的通信，开启之后一定要关闭



# 程序

## 初始方法

```java
import java.sql.*;

public class JDBCTest {
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try {
            //1、注册驱动
            Driver driver = new com.mysql.jdbc.driver();        //多态：父类型对象指向子类型引用
            DriverManager.registerDriver(driver);
            //2、获取连接
            String url = "jdbc:mysql://127.0.0.1:3306/bjpowernode";
            String user = "root";
            String password = "333";
            conn = DriverManager.getConnection(url,user,password);
            //3、获取数据库操作对象   Statement专门执行sql语句
            stmt = conn.createStatement();
            //4、 执行sql
            String sql = "insert into dept(deptno,dname,loc) values(50,'人事部','北京')";
            int count = stmt.executeUpdate(sql);        //专门执行DML语句，返回值是影响数据库记录条数
            //5、 处理查询结果集
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //6、 释放资源
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }


    }
}
```

## 进阶方法：

###### 

注册驱动的代码在驱动的静态代码块中，只要类加载即可，利用反射机制可以类加载

1. DML语句

```java
import java.sql.*;

public class JDBCTest {
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try {
            //1、注册驱动
            Class.forName("com.sql.jdbc.Driver");
            //2、获取连接
            String url = "jdbc:mysql://127.0.0.1:3306/bjpowernode";
            String user = "root";
            String password = "333";
            conn = DriverManager.getConnection(url,user,password);
            //3、获取数据库操作对象   Statement专门执行sql语句
            stmt = conn.createStatement();
            //4、 执行sql
            String sql = "insert into dept(deptno,dname,loc) values(50,'人事部','北京')";
            int count = stmt.executeUpdate(sql);        //专门执行DML语句，返回值是影响数据库记录条数
            //5、 处理查询结果集
        } catch (SQLException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e){
            e.printStackTrace();
        }
        finally {
            //6、 释放资源
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }


    }
}
```

2. 查询语句

```java
import java.sql.*;
import java.util.ResourceBundle;

public class JDBCTest {
    public static void main(String[] args) {
        ResourceBundle bundle = ResourceBundle.getBundle("info");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            Class.forName(driver);
            conn = DriverManager.getConnection(url,user,password);
            stmt = conn.createStatement();
            String sql = "select empno,ename,sal from emp";
            rs = stmt.executeQuery(sql);
            while(rs.next()){
                String empno = rs.getString("empno");
                String ename = rs.getString("ename");
                String sal = rs.getString("sal");
                System.out.println(empno + "," + ename + "," + sal);
            }

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            if(rs != null){
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
                ;
            }
        }
    }
}
```

## 解决SQL注入问题

```java
import java.sql.*;
import java.util.ResourceBundle;

public class JDBCTest {
    public static void main(String[] args) {
        ResourceBundle bundle = ResourceBundle.getBundle("info");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try {
            //1、注册驱动
            Class.forName(driver);
            //2、获取连接
            conn = DriverManager.getConnection(url,user,password);
            String sql = "select empno,ename,sal from emp e where e.deptno = ? ";
            //3、获取预编译的数据库操作对象
            ps = conn.prepareStatement(sql);
            ps.setString(1,"10");
            //4、执行SQL
            rs = ps.executeQuery();
            //5、处理结果集
            while(rs.next()){
                String empno = rs.getString("empno");
                String ename = rs.getString("ename");
                String sal = rs.getString("sal");
                System.out.println(empno + "," + ename + "," + sal);
            }

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //6、释放资源
            if(rs != null){
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(ps != null){
                try {
                    ps.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
                ;
            }
        }
    }
}
```

# Statement和PreparedStatement区别

1. Statement存在SQL注入问题，PreparedStatement解决了SQL注入问题
2. Statement是编译一次执行一次，PreparedStatement是编译一次，可执行N次，PreparedStatement效率高一些
3. PreparedStatement会在编译阶段做类型的安全检查
4. 凡是业务要求进行SQL语句拼接的，必须使用Statement；凡是为了防止SQL注入问题的，必须使用PreparedStatement

# JDBC中的事务

1. JDBC中只要执行任意一条DML语句，就提交一次
2. 改自动提交为手动提交

```java
conn.setAutoCommit(false);		//将自动提交机制改为手动提交，开启事务
conn.commit();					//提交事务
conn.rollback();				//事务回滚
```

# 行级锁

1. 行级锁也叫悲观锁
2. 在语句后面加入 for update，对应的记录(该记录的所有字段)会被锁住，其他事务修改不了，不允许并发

```mysql
select ename,job,sal from emp where job='MANAGER' for update
```

3. 
4. 乐观锁：支持并发，事务也不需要排队，只不过需要一个版本号

