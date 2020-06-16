---
title: 	SQL语句
weight: 10
---

---
命令行：mysql

```bash
mysql -u root -p -h 10.0.0.1 -P 3306

--host=host_name,    -h host_name：服务端地址；
--user=user_name,    -u user_name：用户名；
--port=port_num,     -P port_num：服务端端口；
--socket=path,       -S path
--database=db_name,  -D db_name：
--compress,          -C：数据压缩传输
--execute=statement, -e statement：非交互模式执行SQL语句；
--vertical,          -E：查询结果纵向显示；
--protocol={TCP|SOCKET|PIPE|MEMORY}：
```

## 授权

```sql
语法：
GRANT ALL [PRIVILEGES] ON db.tbl TO 'username'@'host' IDENTIFIED BY 'password';
上述语法中，db表示数据库的名字，可以使用*通配，tbl为表的名称，可以使用*通配

grant 权限 on 数据库名.表名 to 用户名@"本地回环地址或localhost" identified by "密码"
grant all privileges on zsythink.* to zsy@localhost identified by 'zsythink';
grant all privileges on zsythink.* to zsy@127.0.0.1 identified by 'zsythink';
注意：上述两条命令都表示对zsy用户开放zsythink数据库的所有权限，但是上述两条命令的针对的zsy用户是不一样的，一个是zsy@localhost用户，一个是zsy@127.0.0.1用户，mysql会认为这是两个用户。
权时privileges关键字可省，示例如下

MySQL
grant all on zsythink.* to zsy@127.0.0.1 identified  by  'zsythink';
1
grant all on zsythink.* to zsy@127.0.0.1 identified  by  'zsythink';

FLUSH PRIVILEGES;
```

select语句的基本用法，select语句的分组与聚合、多表查询等常用语法

```sql
作死语句，数据量大
select * from tb1;
表示从tb1表中查询出所有数据，但是只显示前3行。
select * from tb1 limit 3;
从tb1表中查询出name字段与age字段的数据，即使这样写，也没有比上例的语句好多少，它仍然是显示表中的所有行的指定字段，表中的数据量较大时，这样写也是非常不好的，除非必要，一般不要这样写。
select name,age from tb1;
从tb1表中查询出符合条件的数据，使用where字句给定条件，带有筛选条件的查询语句则会比上面两种查询语句好很多，如下示例中给出常用的条件表达式。

如下语句表示从tb1表中查询出age等于25的行的name和age字段。
select name,age from tb1 where age = 25;
查出tb1表中age不等于28的数据。
select * from tb1 where age != 28;
如下两条语句均表示从tb1表中查询出age大于等于25并且小于等于28的数据。
select * from tb1 where age >= 25 and age <=28;
select name,age from tb1 where age between 25 and 28;
如下语句表示从tb1表中查询出age等于25或者等于28的数据。
select * from tb1 where age = 25 or age = 28;
如下语句表示从tb1表中查询出age不在25到28区间中的数据。
select * from tb1 where age not between 25 and 28;
select * from tb1 where age < 25 or age > 28;
使用like结合通配符进行模糊查询，如下语句表示查询tb1表中name字段以j开头的数据，"%"在查询语句中表示"任意长度的任意字符".
select * from tb1 where name like 'j%';
如下语句表示查询tb1表中name字段以t开头，并且只有三个字符的数据，"_"在查询语句中表示"任意单个字符"，下例中的语句中，在t后面跟随了两个"_",表示t后面的两个字符可以是任意字符。
select * from tb1 where name like 't__';
也许你觉得还不够灵活，或许你更习惯使用正则表达式作为匹配条件，没有关系，满足你，我们可以使用rlike结合正则表达式，对字符数据进行模糊查询，所以，查询语句能有多强大的功能，就看你的正则表达式运用的有多熟练了，示例语句如下。

如下语句表示查询出tb1表中name字段以t开头的所有数据，正则表达式的含义此处不再赘述。
select * from tb1 where name rlike '^t.*';
我们还可以从指定的列表中匹配对应的条件，使用in关键字指定列表，示例如下，如下语句表示从tb1表中查找出age等于22、23、24或25中的任意一个的行的所有数据。
select * from tb1 where age in (22,23,24,25);
除了使用in，我们还可以使用not in,聪明如你一定秒懂，not in就是in的对立面，比如，查询出tb1表中age不等于28、43、33的数据。
select * from tb1 where age not in (28,33,43);
我们可以对查询出的数据进行排序，如下示例表示查询tb1表中的所有数据，并且按照age的值从小到大进行升序排序，asc表示升序排序，asc可省，默认使用升序排序。
select * from tb1 order by age;
select * from tb1 order by age asc;
如下示例表示查询tb1表中的所有数据，并且按照age的值从大到小进行降序排序
select * from tb1 order by age desc;
查询tb1表中的所有数据，并且按照age的值从大到小进行降序排序，如果多行之间的age字段的值相同时，这些行再根据name字段进行升序排序。
select * from tb1 order by age desc,name asc;
我们可以在查询某字段的时候去重，使用DISTINCT关键字表示去重查询，比如，查询学生的年龄并去重显示年龄。
select distinct age from students;
我们也可以在查询时给字段添加别名，以便显示为我们指定的列名。
select name as StuName,age from tb1;
```



select语句中的分组与聚合

```sql
select语句中group by 的使用，见名知义，group by就是用来分组的。
之所以要对数据进行分组，往往是为了在分组以后，对分组后的数据进行聚合操作
```

![](../../content/MySQL/requirements/images/011217_0208_1.png)

students表中的数据如上图所示

现在，我们使用students表中的数据，进行分组，我们可以根据性别分组，也可以根据年龄分组。

比如，我们根据性别对上述数据进行分组，示例如下。

![mysql/mariadb知识点总结（15）：select语句总结之二：分组与聚合](http://www.zsythink.net/wp-content/uploads/2017/01/011217_0208_2.png)

可以看到，根据性别分组后，只分出了两组，因为性别只有男和女两种性别，所以只分出了两组。

而且每组只显示一条数据，也就是每组的第一条数据，注意，这可能与我们想象的不太一样，分组后每组只显示一条数据。

 

我们说过，分组的目的往往是对分组后的数据进行"聚合操作",设么意思呢？我们先看示例，看完示例再解释，示例如下

![mysql/mariadb知识点总结（15）：select语句总结之二：分组与聚合](http://www.zsythink.net/wp-content/uploads/2017/01/011217_0208_3.png)

上例中，我们通过性别对数据进行了分组，然后算出了每组中的人员数量，也就是说，我们算出了女性有10人，男性有15人。

而上例中的count(stuid)就是一种"聚合操作",count()是一种聚合函数，这个聚合函数能够算出对应数据的条目数量。

上例中的count(stuid)表示算出分组后的每组的stuid的数量，这就是所谓的"分组的目的往往是为了聚合操作"的含义。

 

那么，我们再举一个列子，比如，我们仍然按照性别分组，然后，算出男生与女生的平均年龄，可以使用如下语句。

![mysql/mariadb知识点总结（15）：select语句总结之二：分组与聚合](http://www.zsythink.net/wp-content/uploads/2017/01/011217_0208_4.png)

可以看到，根据性别分组，然后算出每组的平均年龄，男性平均年龄33（普遍成熟稳重），女性平均年龄19（花姑娘大大滴）

聪明如你一定猜到了，avg( )也是一种聚合函数，avg（age）就是求年龄的平均值。

 

那么，mysql中常用的聚合函数有哪些呢，如下？

min(col)返回指定列的最小值

max(col)返回指定列的最大值

avg（col）返回指定咧的平均值

count（col）返回指定列中非null值的个数

sum（col）返回指定列的所有值之和

group_concat(col)返回指定列的值，但是会分组显示，也就是说分组显示指定列组合后的结果，这样说不容易明白，我们来看个例子，比如，将学生表中的学生按性别分组，并且显示男生组有哪些学生，女生组有哪些学生，示例如下。

![mysql/mariadb知识点总结（15）：select语句总结之二：分组与聚合](http://www.zsythink.net/wp-content/uploads/2017/01/011217_0208_5.png)

 

那么，如果我们想要对分组后的信息再次过滤，该怎么办呢，举个例子，如下:

![mysql/mariadb知识点总结（15）：select语句总结之二：分组与聚合](http://www.zsythink.net/wp-content/uploads/2017/01/011217_0208_6.png)

从上例可以看出，如果想要对分组过后的信息再次过滤，可以使用having关键字。

 

好了，此处总结一些常用示例：

查询students表，以性别为分组，求出分组后的年龄之和。





MySQL

| 1    | select gender,sum(age) from students group by gender; |
| ---- | ----------------------------------------------------- |
|      |                                                       |



 

查询students表，以classid分组，显示平均年龄大于25的classid。





MySQL

| 1    | select classid,avg(age) as avgage from students group by classid having avgage > 25; |
| ---- | ------------------------------------------------------------ |
|      |                                                              |



 

查询students表，以性别字段gender分组，显示各组中年龄大于19的学员的年龄的总和。





MySQL

| 1    | select sum(age) from students where age > 19 group by gender; |
| ---- | ------------------------------------------------------------ |
|      |                                                              |



 

分组与聚合先总结到这里，下一篇文章中将会总结多表查询的select语句，直达链接 [select语句总结之多表查询](http://www.zsythink.net/archives/1105)



这篇文章将会详细的总结mysql中多表查询的相关语句，即mysql中的 交叉连接、内连接、外链接、左连接、右连接、联合查询、全连接。

 

在本博客中，"mysql"是一个系列文章，这些文章主要对mysql/mariadb的常用知识点进行了总结，每一篇博客总结的知识点有所不同，具体内容可参考mysql文章列表。

mysql文章列表直达链接：[mysql知识点总结](http://www.zsythink.net/archives/tag/mysql/)

 

多表查询顾名思义就是数据同时从多张表中获得，查询语句牵扯到多张表，多表查询有多种语法，多种使用场景，不同的场景需要不同的语法，我们先不考虑那么多，从头开始理解一下多表查询。

 

## 交叉连接：cross join

既然是多表查询，那么我们先来看看两张非常简单的表，我们就以这两张表为例，进行演示。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_1.png)

上图中，我们通过两条语句分别查询了表1与表2的内容，t1表中有3条数据，t2表中有2条数据，那么同时查两张表，会查询出什么内容呢？我们来实验一下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_2.png)

上图中，我们只是单纯的将两张表使用同一条select语句查询了出来，并没有添加任何额外的过滤条件，仔细观察查询出的数据，可以发现，当使用上图中的语句时，t1表中的每一行记录，都与t2表中的任意一条记录相关联，同样，t2表中的每一行记录，都与t1表中的任意一条记录相关联。

换句话说，两张表中的数据会以下图中的方式被"交叉连接"在一起，然后展示出来。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_3.png)

当然，上述示例中,t1表中有3条数据，t2表中有2条数据,所以"交叉连接"后如上图，如果t1表中有3条记录，t2表中也有3条记录，那么交叉连接后的结果如下图。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_4.png)

我们把上述"没有任何限制条件的连接方式"称之为"交叉连接"，"交叉连接"后得到的结果跟线性代数中的"笛卡尔乘积"一样。

可以看到，使用交叉连接时，任意一张表中的记录多出一行，"交叉连接"的数量都会增长很多。

上述示例中，我们只使用了两张表，而且两张表中的数据非常少，如果我们同时将多张表使用上述语句查询，而且每张表中的数据又比较多，那么可以想象，我们得到结果的时间可能会非常长，而且得到结果以后，可能也没有太大的意义，所以，通过交叉连接的方式进行多表查询的这种方法，我们并不常用，而且我们应该尽量避免这种查询。

 

"交叉连接"的英文原文为"cross join",被咱们翻译为交叉连接，其实，上述示例中的语句我们可以换一种写法，两种写法能够获取到相同的结果，示例如下

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_5.png)

其实，上图中的第一种写法才是官方建议的最标准的写法，即为使用"cross join"将多张表使用"交叉连接"连接起来，当然，上述实例中，我们只使用了t1与t2两张表作为示例，我们也可以将多张表使用"cross join"连接起来，比如将t1,t2,t3三张表使用"cross join"连接起来，示例语句如下：





MySQL

| 1    | select * from t1 cross join t2 cross join t3; |
| ---- | --------------------------------------------- |
|      |                                               |



在mysql中，上述查询语句查询出的结果与如下语句相同。





MySQL

| 1    | select * from t1,t2,t3; |
| ---- | ----------------------- |
|      |                         |



  

## 内连接：inner join

既然"交叉连接"不常用，那么肯定有其他的常用的"多表查询方式"。

我们来看看另一种常用的多表查询的方式：内连接

仍然拿刚才的t1表与t2表为例，此处回顾一下这两张表的内容。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_6.png)

那么什么是"内连接"呢？我们可以把"内连接"理解成"两张表中同时符合某种条件的数据记录的组合"，这样说不容易理解，我们来动手做一个小例子，示例如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_7.png)

上图中的sql语句就使用了"内连接"，上图中的sql语句查询出了t1表与t2表中id号相同的记录，并把两表中id号相同的记录连接在了一起，我们对比着"内连接"的概念，来理解上图中的sql语句，我们说过内连接就是"两张表中同时符合某种条件的数据记录的组合"，那么上图中，"where t1.t1id=t2.t2id"就是所谓的"符合某种条件"，上图中查询出的结果就是"两张表中同时符合某种条件的数据记录的组合"，这其实就是所谓的"内连接"。

聪明如你一定发现了，在mysql中，"内连接"的语句与"交叉连接"的语句的不同之处就是"内连接"语句比"交叉连接"有更多的限制条件，这样理解"内连接"，会不会容易一点呢？

"内连接"的英文原文为"inner join"，所以，刚才的内连接sql语句还能换成另一种写法，两种写法得到的结果是相同的，示例如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_8.png)

上图中的第一种语法才是官方建议的标准写法，所以，我们在使用"内连接"类型的sql语句时，应该尽量采用上图中的第一种写法。内连接的两张表用"inner join"连接在一起，使用"on"指明"条件"。

我们刚才说过，在mysql中，"内连接"与"交叉连接"的不同之处就是"内连接"语句比"交叉连接"语句有更多的限制条件，那么如果我们把"内连接"的"限制条件"去掉，得出的结果会与"交叉连接"得出的结果相同吗？我们来做一个"实验"。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_9.png)

从实验结果可以看出，当不附加任何条件时，内连接与交叉连接查询出的结果并没有什么不同，那么反过来想，如果"交叉连接"加上"连接条件"，是否与"内连接"查询得到的结果相同呢？我们来试试。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_10.png)

好了，实验证明，在mysql中，"cross join"与"inner join"似乎可以互相替代，但是在通用的sql标准中，这两者是不同的。

同时我们得出了一个结论，在通常情况下，使用内连接时需要指定连接条件，换句话说，就是使用"inner join"时一定不要忘记使用"on"指明连接条件。

 

此刻，你可能还是没有理解什么是内连接，那么我们换一种解释方式，我们用图示的方法描述一遍什么是内连接。

我们把t1表与t2表当做两个集合，把t1id与t2id分别当这做两个集合中的元素，可以理解为下图。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_11.png)

还记得我们刚才使用的"内连接"查询语句吗，"内连接"查询语句如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_12.png)

即t1id与t2id相同的记录被查询了出来，从结果来看，由于t2表中并不存在id号为1的记录，所以，只查询出了两张表中id号同为2和3的两条记录，用图表示如下

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_13.png)

这就是所谓的"内连接"。

但是，"内连接"还能够分为多种，比如"等值连接"和"不等连接"，刚才示例中使用的内连接就属于"等值连接"，聪明如你一定想到了，内连接是否属于"等值连接"取决于"连接条件"中有没有使用"等号"。

那么我们给出一个"不等连接"的示例，如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_14.png)

上图中的"内连接"就属于"不等连接"，同样，下图中的"内连接"也属于"不等连接"，只要"连接条件"中没有使用"="作为连接条件的都为"不等连接"

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_15.png)

那么，用"图示"的方法表示上图中的"内连接"语句，可以参考下图。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_16.png)

 

从上图中可以发现，使用"内连接"语句查询出的结果集是两个集合中"同时满足条件的数据"的"组合",所以我们并不能单纯的用"交集"去表示这个组合，就以上图为例，按照"交集"的定义，属于集合A且同时属于集合B的元素所组成的集合被称为交集，但是上图中，id号为1的元素只属于t1表，在t2表中并不存在id号为1的元素，但是，上图中"中间"的结果集就是"内连接"查询出的结果，所以，我们不能单纯的用"交集"表示"内连接"，但是，我们可以从另一个角度定义"交集"，我们定义，"交集"为"两个集合中同时满足条件的数据的组合",那么，我们可以把"内连接"查询出的结果集用下图表示。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_17.png)

通过上图去理解"内连接"，可能更容易理解一点。

 

其实，"内连接"除了"等值连接"与"不等连接"，还有一种分类，被称作"自连接"，自连接可以理解为比较特殊的"内连接",刚才说到的"等值连接"与"不等连接"所连接的表为两张不同的表，而"自连接"连接的表为一张表，也就是自己连接自己，所以被称为"自连接"，什么意思呢，我们来动手做个例子。

有一张students表，数据如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_18.png)

这张表中存放了"学生"的名字，同时也存放了"老师"的名字,因为这张表里面的学生有可能是其他"学生"的"老师",他们之间互相学习，所以，上表中tid对应的就是学生的id，那么，我们可以通过"自连接",查出每个"学生"的"老师"的名字，示例语句如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_19.png)

上图中的两个sql语句就属于"自连接"，自连接把同一张表当做两张表连接了起来，这就是"自连接"，很容易理解吧。

 

其实在mysql中，"inner join"还可以缩写为"join"，他们是等效的，示例如下：

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_20.png)

 

  

## 外连接：left join ， right join

"外连接"分为两种，"左外连接"和"右外连接"，我们只要搞明白其中的任意一个，就能明白另一个是什么意思，有了之前的"交叉连接"和"内连接"的基础，再看"外连接"，就容易多了。

那么，我们先来了解了解"左外连接"，"左外链接"的英文原文为"left outer join"，我们可以使用"left outer join"将两张表进行左外链接，我们先来动手做个小例子。

仍然以t1表与t2表为例，老规矩，先回顾一下两张表中的数据。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_21.png)

t1表与t2表中的数据如上图所示，现在，我们将两张表使用"左外链接"连接起来，示例如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_22.png)

可以看到，上图中查询出的数据似乎跟之前的"内连接"查询出的数据有一部分相同，但是又不是完全相同，我们来对比一下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_23.png)

通过对比，我们发现，在同样的连接条件下，"左外连接"查询出的数据更多一点，多出的一行记录由t1表中的id号为1的记录和一条"空记录"组成。

可是t2表中并不存在id号为1的记录啊，为什么不符合连接条件的记录也会出现在查询结果中呢？这就是左外连接的特性。

左外连接不仅会查询出两表中同时符合条件的记录的组合，同时还会将"left outer join"左侧的表中的不符合条件的记录同时展示出来，由于左侧表中的这一部分记录并不符合连接条件，所以这一部分记录使用"空记录"进行连接。

换句话说，左外连接"左侧的表"中的所有记录都会被展示出来，左侧表中符合条件的记录将会与右侧表中符合条件的记录相互连接组合，左侧表中不符合条件的记录将会与右侧表中的"空记录"进行连接。

上述示例中的t1表就是"left outer join"左侧的表，t2表就是"left outer join"右侧的表，连接条件就是t1id=t2id,虽然t1表中id号为1的记录不满足连接条件，但是仍然会被展示出来，t2表中会使用"空记录"与其进行连接，表示t1表中对应的记录是不满足连接条件的记录。

如果刚才的描述还是不能让你理解左外连接，那么，我们来画个图看看，仍然使用类似之前"内连接"中的"示意图"进行示意。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_24.png)

上图中，两个彩色的集合组成了左外连接查询出的结果集，看到这里，我想你应该已经明白什么是"左外链接"了，既然明白了"左外连接"，那么"右外连接"就更容易理解了，左外连接是以连接左侧的表为准，不管左侧表中的记录是否符合连接条件，都会被显示出来并且右侧的表中会使用空记录与之连接，那么"右外连接"就是以连接右侧的表为准，不管右侧表中的记录是否符合连接条件，都会被显示出来并且左侧的表中会使用空记录与之连接，我们来看一个小例子。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_25.png)

上述例子中，t2表为"右外连接"右侧的表，t1表为"右外连接"左侧的表，虽然t2表中id号为3的记录并不满足连接条件，但是仍然被展示了出来，t1表中使用空记录与之相连接，那么，用图示的方法表示"右外连接"如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_26.png)

好了，我想我已经把"左外连接"和"右外连接"说明白了。

使用"左外连接"或者"右外连接"时，有可能所有记录都符合连接条件，这时就不会出现使用"空记录"连接的情况，比如如下情况。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_27.png)

虽然我们使用了右外链接，但是t2表中的所有记录都满足连接条件，所以，t1表中并不会出现"空记录"与t2表中的记录进行连接。

其实，"左外连接"可以简称为"左连接"，"右外连接"可以简称为"右连接"，"left outer join"可以简写为 "left join" ，同理，"right outer join"可以简写为"right join"，示例如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_28.png)

 



 

其实，我们还可以将左连接与右连接扩展一下，在左连接或者右连接的基础上添加更多的过滤条件，示例如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_29.png)

我们在之前左连接语句的基础上添加了更多限制条件，使用where子句过滤出了t2表中使用"空记录"连接的记录，那么，所查询出的结果一定是t1表中不符合连接条件的记录。这个结果用图表示可能更容易理解，图示如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_30.png)

上述示例中的左连接语句查询出了存在于左侧表中，但是不满足连接条件的数据记录，如上图中的集合所示。

同理，我们也可以在右连接语句上使用同样的方法，示例如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_31.png)

上图中的右连接语句查询出的结果可以用如下示意图表示。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_32.png)

似乎，我们能够适应的场景越来越多了，不过我还没有说完，咱们继续聊。

  

## 联合查询：union 与 union all

联合查询比较容易理解，我们可以把联合查询理解成把多个查询语句的查询结果集中在一起显示，语法示例如下。

select column_name(s) from table_name1 **UNION** select column_name(s) from table_name2

 

我们来动手做一个小例子，仍然以t1表与t2表为例。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_33.png)

 

此处，我们将上图中的两条语句使用union连接起来。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_34.png)

可以看到，使用union将两条sql语句连接起来以后，两个sql对应的结果集也被集中显示了，是不是很简单。

从上图可以看出，默认情况下，结果集的字段名以t1表中的为准，如果我们想要以t2表中的为准，可以将t2表对应sql放在union之前，如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_35.png)

当然，我们也可以使用别名，示例如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_36.png)

当使用union连接两个查询语句时，两个语句查询出的字段数量必须相同，否则无法使用union进行联合查询，示例如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_37.png)

上图中的t3表有3个字段，而t2表有两个字段，如果想要使用union将上图中的语句连接，必须使得两个sql的结果集查询出的字段数量相同。

 

使用union将两个结果集集中显示时，重复的数据会被合并为一条，示例如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_38.png)

上图中，t2表中的两条记录与t4表中的两条记录完全相同，所以，使用union查询出的重复结果被合并为一条。

我们能不能让重复的记录都显示出来呢？必须能啊，union all的作用就在于此，示例如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_39.png)

使用union all进行联合查询时，如果两条sql语句存在重复的数据，重复的记录会被展示出来。

  

## 全连接：full join

在之前，我们已经总结了mysql中的"交叉连接"、"内连接"、"左连接"、"右连接"以及"联合查询"的多表查询方式，其实在sql标准中，还有一种被称为"全连接"的多表查询方式，"全连接"的英文原文为full join，但是在mysql中并不支持"全连接"，更准确的说，mysql中不能直接使用"full join"实现全连接，不过，我们可以变相的实现"全连接",在mysql中，我们可以使用"left join"、"union"、"right join"的组合实现所谓的"全连接"。

什么意思呢？空口白话的描述实在费劲，我们动手做个小例子，我们用两张简单的表进行示例，t1表与t5表，数据如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_40.png)

 

我们先使用左连接查询出对应的数据，如下：

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_41.png)

 

使用同样的连接条件，再使用右连接查询出对应的数据。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_42.png)

 

最后，使用union将两条语句连接在一起，即可以在mysql中实现"全连接"所实现的查询功能，示例如下。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_43.png)

由于union会将"左连接"与"右连接"查询出的结果集中的重复数据合并，所以，查询出的结果如上图所示。

如果用图示的方法表示上图中的语句，可以参考如下示意图。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_44.png)

"全连接"可以使用上图中的彩色集合进行示意。

与"左连接"或者"右连接"一样，"全连接"也可以添加更多的连接条件，没错，聪明如你一定想到了，语句如下

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_45.png)

那么上图中的语句用图示表示如下，下图中的绿色集合与紫色集合组成了上图中的"全连接语句"查询出的结果集。

![mysql/mariadb知识点总结（16）：select语句总结之三：多表查询](http://www.zsythink.net/wp-content/uploads/2017/01/011317_0113_46.png)

 

好了，mysql中的各种常用的多表查询方式我们已经总结完毕，不知道这篇博文对你有没有帮助呢？写博不易，希望大家多多支持，评论和点赞都是免费的哦~嘿嘿嘿~~~！