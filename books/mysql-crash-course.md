---
title: MySQL必知必会
---
# 1.了解SQL

参见 SQL必知必会 第1章. 

# 2.MySQL简介

DBMS可以分为两类:
- 基于共享文件系统的DBMS(如: Microsoft Access和FileMaker)
- 基于客户机-服务器(如: MySQL)

MySQL相关的3个工具: 
- mysql命令行程序
- MySQL Administrator(MySQL管理器)是图形交互客户机, 用来简化MySQL服务器的管理.
- MySQL Query Browser是图形交互客户机, 用来编写和执行MySQL命令.

# 3.使用MySQL

连接上数据库后, 必须使用USE打开数据库, 才能读取其中的数据.
SHOW DATABASES;返回可用数据库的列表.
SHOW TABLES;返回当前选择数据库内的表的列表.
SHOW COLUMNS FROM table;返回table表的列的列表. (DESCRIBE table;语句起到同样的效果)

支持的其他SHOW语句:
- SHOW STATUS;显示广泛的服务器状态信息.
- SHOW CREATE DATABASE database;和SHOW CREATE TABLE table;分别用来显示创建特定数据库或表的MySQL语句.
- SHOW GRANTS;用来显示授予用户(所有/特定用户)的安全权限.
- SHOW ERRORS;和SHOW WARNINGS;用来显示服务器错误或警告消息.

# 4.检索数据

MySQL中多条SQL语句必须以分号(;)分隔. 而单条SQL语句后和多数DBMS一样不需要语句后加分号.

注: 不能部分使用DISTINCT. DISTINCT关键字应用于所有列而不仅是前置它的列. 

LIMIT 5,6指示MySQL返回从第5行开始的6行. 第一个数字为开始位置(从第0行开始), 第二个数为要检索的行数.
由于上面的容易搞糊涂, 所以MySQL5支持另一种替代语法:

~~~ sql
LIMIT 4 OFFSET 3 -- 从行3开始取4行 也就是LIMIT 3, 4
~~~

# 5.排序检索数据

使用ORDER BY和LIMIT的组合, 能够找出一个列中最高或最低的值.

注意: ORDER BY子句应该保证它位于FROM子句之后, 如果使用LIMIT, 它必须位于ORDER BY之后. 使用子句次序不对将产生错误信息.

# 6.过滤数据

MySQL在匹配字符串时默认不区分大小写.

# 7.数据过滤

参见 SQL必知必会 第5章.

# 8.用通配符进行过滤

参见 SQL必知必会 第6章.

# 9.用正则表达式进行搜索

REGEXP关键字, MySQL用WHERE子句对正则表达式提供了初步的支持.
MySQL中的正则表达式匹配(自版本3.23.4后)不区分大小写. 为区分小写可以使用BINARY关键字. 如WHERE prod_name REGEXP BINARY 'JetPack .000'.

注: MySQL中使用正则表达式. 多数正则表达式实现使用单个反斜杠转义特殊字符, 以便能使用这些字符本身. 但MySQL要求两个反斜杠, 因为MySQL自己解释一个, 正则表达式库解释另一个.

这一章主要说正则表达式, 还是以后用到再看看吧.

# 10.创建计算字段

- 拼接(concatenate)MySQL的SELECT语句中可以用Concat()函数来拼接两个列.Concat()需要一个或多个指定的串, 各个串之间用逗号分隔.

~~~ sql
Concat(vend_name, ' (', vend_country, ')')
~~~

# 11.使用数据处理函数

使用函数的SQL可移植性没这么强, 如果决定使用函数, 应该保证做好代码注释, 以便以后能确切地知道所编写SQL代码的含义.

大多数SQL实现支持以下类型的函数:
- 用于处理文本串
- 用于在数值数据上进行算术操作
- 用于处理日期和时间值并从这些值中提取特定成分
- 返回DBMS正使用的特殊信息的系统函数

# 12.汇总数据

参见 SQL必知必会 第9章. 差不多的.

# 13.分组数据

参见 SQL必知必会 第10章. 差不多的.

# 14.使用子查询

参见 SQL必知必会 第11章. 差不多的.

# 15.联结表

参见 SQL必知必会 第12章. 差不多的.

# 16.创建高级联结

参见 SQL必知必会 第13章. 差不多的.

# 17.组合查询

参见 SQL必知必会 第14章. 差不多的.

# 18.全文本搜索

注意: 并非所有引擎都支持全文本搜索, MySQL中最常用的引擎为MyISAM和InnoDB, 前者支持全文本搜索, 而后者不支持.

使用ENGINE=MyISAM指定引擎, 使用FULLTEXT()指定全文本搜索的索引列.

~~~ sql
CREATE TABLE productnotes
(
    note_id int NOT NULL AUTO_INCREMENT,
    prod_id char(10) NOT NULL,
    note_date datetime NOT NULL,
    note_text text NULL,
    PRIMARY KEY(note_id),
    FULLTEXT(note_text)
) ENGINE=MyISAM;
-- 作者提供的数据 来源: http://www.forta.com/books/0672327120/
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(101, 'TNT2', '2005-08-17',
'Customer complaint:
Sticks not individually wrapped, too easy to mistakenly detonate all at once.
Recommend individual wrapping.'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(102, 'OL1', '2005-08-18',
'Can shipped full, refills not available.
Need to order new can if refill needed.'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(103, 'SAFE', '2005-08-18',
'Safe is combination locked, combination not provided with safe.
This is rarely a problem as safes are typically blown up or dropped by customers.'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(104, 'FC', '2005-08-19',
'Quantity varies, sold by the sack load.
All guaranteed to be bright and orange, and suitable for use as rabbit bait.'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(105, 'TNT2', '2005-08-20',
'Included fuses are short and have been known to detonate too quickly for some customers.
Longer fuses are available (item FU1) and should be recommended.'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(106, 'TNT2', '2005-08-22',
'Matches not included, recommend purchase of matches or detonator (item DTNTR).'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(107, 'SAFE', '2005-08-23',
'Please note that no returns will be accepted if safe opened using explosives.'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(108, 'ANV01', '2005-08-25',
'Multiple customer returns, anvils failing to drop fast enough or falling backwards on purchaser. Recommend that customer considers using heavier anvils.'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(109, 'ANV03', '2005-09-01',
'Item is extremely heavy. Designed for dropping, not recommended for use with slings, ropes, pulleys, or tightropes.'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(110, 'FC', '2005-09-01',
'Customer complaint: rabbit has been able to detect trap, food apparently less effective now.'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(111, 'SLING', '2005-09-02',
'Shipped unassembled, requires common tools (including oversized hammer).'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(112, 'SAFE', '2005-09-02',
'Customer complaint:
Circular hole in safe floor can apparently be easily cut with handsaw.'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(113, 'ANV01', '2005-09-05',
'Customer complaint:
Not heavy enough to generate flying stars around head of victim. If being purchased for dropping, recommend ANV02 or ANV03 instead.'
);
INSERT INTO productnotes(note_id, prod_id, note_date, note_text)
VALUES(114, 'SAFE', '2005-09-07',
'Call from individual trapped in safe plummeting to the ground, suggests an escape hatch be added.
Comment forwarded to vendor.'
);
~~~

注意: 不要在导入数据时使用FULLTEXT, 建立索引需要时间, 建议先导入完数据再修改表定义FULLTEXT.

进行全文本搜索:

~~~
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('rabbit');
~~~

Match(note_text)指示MySQL针对指定的列进行搜索(传递给Match()的值必须与FULLTEXT()定义中的相同, 如果指定多个列, 则必须根据次序列出他们), Against('rabbit')指定词rabbit作为搜索文本(除非用BINARY否则不区分大小写).

虽然以上的搜索用LIKE同样可以实现, 但是使用全文本搜索返回的是匹配和排序良好的数据. 全文本搜索会将具有高等级的行先返回(比如rabbit排在第3个词要优于排在20个词).

演示排序如何工作:

~~~ sql
SELECT note_text, Match(note_text) Against('rabbit') AS rank
FROM productnotes;
~~~

以上通过Match()和Against()建立了一个计算列rank, 这一列包含全文本搜索计算出的等级值(由行中词数目, 唯一词的数目, 整个索引中词的总数以及包含该词的的行的数目计算出来). rank值靠前的行等级值比词靠后的行等级值高.

所以, 全文本搜索提供了简单LIKE搜索不能提供的功能. 而且因为数据是索引的, 所以全文本搜索还相当快.

使用查询扩展:

~~~ sql
SELECT note_text
FROM productnotes
WHERE Match(note_text) Against('anvils' WITH QUERY EXPANSION);
~~~

第二行查询的结果与anvils无关, 但因为它包含第一行中的两个词(customer和recommend), 所以也被检索出来. 查询扩展极大地增加了返回的行数.

布尔文本搜索:

这是一种高级的搜索, 可以选择包含或不包含的词, 类似于正则表达式, 设定一些高级匹配规则过滤数据等.

全文本搜索还有一些说明:
比如有一些词在进行全文本搜索时会被忽略(3个字符以下, 内建非用词, 出现频率高的词, 不具有词分隔符等...), 有一些是可以配置的.

# 19.插入数据

参见 SQL必知必会 第15章.

如果数据检索是重要的, 那么可以通过给INSERT和INTO之间添加LOW_PRIORITY关键字, 指示MySQL降低INSERT语句的优先级. 同样也适用于UPDATE和DELETE语句.

可以用单条语句插入多行以提高INSERT性能:

~~~ sql
INSERT INTO table(col1, 
    col2, 
    col3)
VALUES(
        val11,
        val12,
        val13
    ),
    (
        val21,
        val22,
        val23
    );
~~~

# 20.更新和删除数据

参见 SQL必知必会 第16章.

IGNORE关键字: 在UPDATE更新多行时, 如果出现一个错误整个UPDATE操作都会被取消(恢复到更新前的值). 如果希望发生错误也继续更新, 可以使用IGNORE关键字.

~~~ sql
UPDATE IGNORE table ...
~~~

小心使用: MySQL没有撤销(undo)按钮, 应该要非常小心的使用UPDATE和DELETE.

# 21.创建和操纵表

参见 SQL必知必会 第17章.

如果你仅想在一个表不存在时创建它, 应该在表明后给出IF NOT EXISTS. 这只检查表明是否存在, 不存在则创建该表, 而不是检查表模式是否相匹配.

覆盖AUTO_INCREMENT: 你可以在INSERT语句中指定一个唯一的至今未使用过的值, 那么这个值会被用来替代自动生成的值, 后续的增量也会从这个值开始.

确定AUTO_INCREMENT值: 可以使用last_insert_id()函数获得这个值, 具体用法还是看文档吧.

与大多数DBMS不一样, MySQL不允许使用函数作为默认值, 它只支持常量.

MySQL与其他DBMS不一样, 它有很多种引擎, 为不同的任务选择正确的引擎能获得良好的功能和灵活性. 下面是需要知道的引擎:
- InnoDB是一个可靠的事务处理引擎, 不支持全文本搜索.
- MEMORY功能上等同于MyISAM, 但数据存储在内存而不是磁盘, 所以速度很快, 特别适合用于临时表.
- MyISAM是高性能引擎, 支持全文本搜索, 不支持事务.

不同表可以有不同的引擎, 也就是引擎混用, 但是外键不能跨引擎.

删除表(删除整个表而不是其内容)使用DROP TABLE语句. 删除表不能撤销.

~~~ sql
DROP TABLE table;
~~~

重命名表:

~~~
RENAME TABLE table1 TO table2;
~~~

# 22.使用视图


参见 SQL必知必会 第18章.

性能问题: 如果使用多个联结和过滤创建了复杂的视图或嵌套了视图, 可能会发现性能下降得很厉害, 因此在部署使用了大量视图的应用前, 应该进行测试.

- CREATE VIEW创建视图.
- SHOW CREATE VIEW viewname;来查看创建视图的语句.
- DROP VIEW viewname;删除视图.
- 更新时可以先删除再创建, 也可也直接用CREATE OR REPLACE VIEW.

# 23.使用存储过程


参见 SQL必知必会 第19章.

MySQL称存储过程的执行为调用, 因此执行语句为CALL.

~~~ sql
CALL productpricing(@pricelow, 
                    @pricehigh, 
                    @priceaverage);
~~~

执行名为productpricing的存储过程, 计算并返回产品的最低, 最高和平均价格.

创建存储过程:

~~~ sql
CREATE PROCEDURE productpricing()
BEGIN
    SELECT Avg(prod_price) AS priceaverage
    FROM products;
END;
~~~

此sql语句创建一个名为productpricing的存储过程, 接收参数在括号()中列出来. 这里只是创建一个新的存储过程, 并没有调用执行所以没有返回数据.

注意: 如果你使用的是mysql命令行实用程序, 因为MySQL默认的语句分隔符为分号;. 所以如果创建存储过程体中包含了分号;, 那么它就会以为语句结束了, 因而会出现语法错误. 解决办法是临时更改命令行实用程序的分隔符, 如下所示:

~~~ sql
DELIMITER // -- 使用//作为语句分隔符

CREATE PROCEDURE productpricing()
BEGIN
    SELECT Avg(prod_price) AS priceaverage
    FROM products;
END //

DELIMITER ; -- 还原本来的分隔符;
~~~

使用上述存储过程:

~~~ sql
CALL productpricing()
~~~

删除存储过程:

~~~ sql
DROP PROCEDURE productpricing
~~~

使用参数:

~~~ sql
CREATE PROCEDURE productpring(
    OUT pl DECIMAL(8, 2),
    OUT ph DECIMAL(8, 2),
    OUT pa DECIMAL(8, 2)
)
BEGIN
    SELECT Min(prod_price)
    INTO pl
    FROM products;
    SELECT Max(prod_price)
    INTO ph
    FROM products;
    SELECT Avg(prod_price)
    INTO pa
    FROM products;
END;
~~~

此存储过程接受3个参数, pl存储产品最低价格, ph存储产品最高价格, pa存储产品平均价格. MySQL支持IN(传递给存储过程), OUT(从存储过程传出), INOUT(对存储过程传入和传出)类型的参数. 在BEGIN和END语句之间是SELECT语句来检索值然后保存到相应的变量(通过指定INTO关键字).

为了调用这个带参数的存储过程, 需要指定三个变量名(MySQL变量都必须以@开始):

~~~ sql
CALL productpring(@pricelow, @pricehigh, @priceaverage);
~~~

在调用时, 这条语句不会显示任何数据. 它返回以后可以使用以下语句获得3个值:

~~~ sql
SELECT @pricelow, @pricehigh, @priceaverage;
~~~

检查存储过程:

~~~ sql
SHOW CREATE PROCEDURE productpring;
~~~

# 24.使用游标

创建游标:

~~~ sql
CREATE PROCEDURE processorders()
BEGIN
    DECLARE ordernumbers CURSOR
    FOR
    SELECT order_num FROM orders;
END;
~~~

这个存储过程没有做啥事, 就是定义和命名了游标ordernumbers. 存储过程处理完后游标消失(因为它局限于存储过程, 隐含关闭). 逻辑应该写在OPEN cursor和ENDcursor之间, 作者举了个例子我就不摘了.

# 25.使用触发器

