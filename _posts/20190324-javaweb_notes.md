---
title: JavaWeb笔记
date: 2019-03-24
updated: 2021-01-21
tags:
  - Java
  - Java Web
  - Note
urlname: javaweb_notes
original: true
---
老旧的javaweb笔记
<!--more-->

# Cues

包名规范, PreparedStatement, JDBC结果集操作, BLOB, CLOB, jsp注释, GET和POST

# Notes

## 包名的规范:

工具类:cn.edu.school.dao
一般放对数据的操作类,数据库增删改查.
数据传输类:cn.edu.school.dto
定义一些数据类,生成get,set方法供外部使用.
测试类:cn.edu.school.test
测试都在这里做,主函数入口了.
常用的独立出一个类:cn.edu.school.util
比如:连接数据库的操作可以独立出来.直接丢:

~~~ java
public class DataAccess{
	static Connection conn = null;
	public static Connection getConnection(){
		try {
			Class.forName("com.mysql.jdbc.Driver");
			conn = DriverManager.getConnection
					("jdbc:mysql://localhost:3306/school", "root", "root");
		} catch (ClassNotFoundException e) {
			System.out.println("数据库链接获取失败,连接用的jar包找不到");
			e.printStackTrace();
		}catch (SQLException e) {
			System.out.println("请检查数据库连接参数设置");
			e.printStackTrace();
		}
		return conn;
	}
}
~~~

每次DAO类里需要连接的时候,try-with-resource里直接调用:Connection conn = DataAccess.getConnection();

## PreparedStatement使用

和Statement比缺点就是只能提交一次.

~~~ java
PreparedStatement prep1 = conn.prepareStatement
		("select * from student where sName = ? and sPassword = ?");
	prep1.setString(1, _name);
	prep1.setString(2, _password);
	rs = prep1.executeQuery();//执行查询返回结果
	
PreparedStatement prep2 = conn.prepareStatement
		("update Course set cName = ? where cId = ?");
	prep2.setString(1, cName);
	prep2.setString(2, cId);
	prep2.executeUpdate();//执行更新
	
---//使用批处理---
PreparedStatement prep3 = conn.prepareStatement
		("update student set sName=? where sId=?");
	for(int i=0; i<10000; i++){
        prep3.setString(1, "Name"+i);
        prep3.setInt(2, i);
        prep3.addBatch();//添加到同一个批处理中
        }
	prep3.executeBatch();//执行批处理
~~~

## JDBC结果集(ResultSet接口)的移动和更新

~~~ java
Statement stat = conn.createStatement
		(ResultSet.TYPE_SCROLL_INSENSITIVE, ResultSet.CONCUR_UPDATABLE);//允许结果集动
ResultSet rs = stat.executeQuery
		("select * from student");
//---常用结果集命令---
rs.beforeFirst();
rs.first();
rs.last();
rs.absolute(4);//定位到第四个元素
rs.getRow();//获取当前行号
//---更改---
rs.updateString ("sId","s04");//更改当前结果集sId列值
//---插入---
rs.moveToInsertRow();//移动到待插入的行
rs.updateString("sId","s04");
rs.insertRow();//执行插入
~~~

## BLOB数据的存取

先在mysql创建含有id列和列名为binaryfile的blob类型(最大存储限制65k)数据的blobtable表
简单存取图片的示例:

~~~ java
//---BLOB存入图片p1.jpg---
File f = new File("c:\\p1.jpg");
FileInputStream fis = new FileInputStream(f);
prep = conn.prepareStatement("insert into blobtable values(?,?)");
prep.setInt(1, 1);
prep.setBinaryStream(2, fis, (int)f.length());
prep.executeUpdate();
//---BLOB取出图片p1.jpg---
prep1 = conn.prepareStatement("select * from blobtable where id = 1");
rs = prep1.executeQuery();
rs.next();
InputStream is = rs.getBinaryStream("binaryfile");
File f = new File(System.getProperty("user.home")+System.getProperty("file.separator")+"p1.jpg");
FileOutputStream fos = new FileOutputStream(f);
int i;
while((i = is.read())!=-1){
	fos.write(i);
}
~~~

## CLOB数据的存取

先在mysql创建含有id列和列名为clobfile的text类型(最大存储限制65k)数据的clobtable表
简单存取文件的示例:

~~~ java
//---CLOB存---
File f = new File(System.getProperty("user.home")+System.getProperty("file.separator")+"a.txt");
FileInputStream fis = new FileInputStream(f);
prep = conn.prepareStatement("insert into clobtable values(?,?)");
prep.setInt(1, 1);
prep.setAsciiStream(2, fis, (int)f.length());
prep.executeUpdate();
//---CLOB取---
stat = conn.createStatement();
rs = stat.executeQuery("select * from clobtable where id = 1");
rs.next();
InputStream is = rs.getAsciiStream("clobfile");
File f = new File(System.getProperty("user.home")+System.getProperty("file.separator")+"a2.txt");
FileOutputStream fos = new FileOutputStream(f);
int i = 0;
while((i = is.read())!=-1){
	fos.write(i);
}
~~~

## jsp里6种注释

客户端可见

~~~
<!-- html注释 -->
~~~

客户端不可见, 服务端可见

~~~
<%-- jsp注释 --%>
~~~

嵌入代码的注释

~~~
<!-- <%= 输出new Data() %> -->
~~~

java中的三种注释

~~~
// 单行注释
/* 多行注释 */
/** 文档注释 */
~~~

## get和post区别

### get

是form默认的提交方式
如果通过一个超链访问某个地址, 是get方式
如果在地址栏直接输入某个地址, 是get方式
提交数据会在浏览器显示出来
不可以用于提交二进制数据, 比如上传文件

### post

必须在form上通过 method="post" 显示指定
提交数据不会在浏览器显示出来
可以用于提交二进制数据，比如上传文件

# Summary

没有总结哈哈哈









