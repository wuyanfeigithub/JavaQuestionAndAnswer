#### About database Interview Questions and Answers .
   
 1. A、B两表，找出ID字段中，存在A表，但是不存在B表的数据。A表总共13w数据，去重后大约3W条数据，B表有2W条数据，且B表的ID字段有索引。

  方法一 使用 not in ,容易理解,效率低  ~执行时间为：1.395秒~
  select distinct A.ID from  A where A.ID not in (select ID from B)

  方法二 使用 left join...on... , "B.ID isnull" 表示左连接之后在B.ID 字段为 null的记录  ~执行时间：0.739秒~
  select A.ID from A left join B on A.ID=B.ID where B.ID is null

  方法三 逻辑相对复杂,但是速度最快  ~执行时间: 0.570秒~
  select * from  B 
  where (select count(1) as num from A where A.ID = B.ID) = 0 

 2. 各种数据库连接池对比
 
<table border=1>
		<thead>
		<tr>
		<th style="text-align:left"></th>
		<th style="text-align:left">Druid</th>
		<th style="text-align:left">BoneCP</th>
		<th style="text-align:left">DBCP</th>
		<th style="text-align:left">C3P0</th>
		<th style="text-align:left">Proxool</th>
		<th style="text-align:left">JBoss</th>
		<th style="text-align:left">Tomcat-Jdbc</th>
		</tr>
		</thead>
		<tbody>
		<tr>
		<td style="text-align:left">LRU</td>
		<td style="text-align:left">是</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">是</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">是</td>
		<td style="text-align:left">是</td>
		<td style="text-align:left">?</td>
		</tr>
		<tr>
		<td style="text-align:left">PSCache</td>
		<td style="text-align:left">是</td>
		<td style="text-align:left">是</td>
		<td style="text-align:left">是</td>
		<td style="text-align:left">是</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">是</td>
		</tr>
		<tr>
		<td style="text-align:left">PSCache-Oracle-Optimized</td>
		<td style="text-align:left">是</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left"></td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">否</td>
		</tr>
		<tr>
		<td style="text-align:left">ExceptionSorter</td>
		<td style="text-align:left">是</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">是</td>
		<td style="text-align:left">否</td>
		</tr>
		<tr>
		<td style="text-align:left">更新维护</td>
		<td style="text-align:left">是</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">否</td>
		<td style="text-align:left">?</td>
		<td style="text-align:left">是</td>
		</tr>
		</tbody>
</table>

    LRU

    LRU是一个性能关键指标，特别Oracle，每个Connection对应数据库端的一个进程，如果数据库连接池遵从LRU，有助于数据库服务器优化，这是重要的指标。在测试中，Druid、DBCP、Proxool是遵守LRU的。BoneCP、C3P0则不是。BoneCP在mock环境下性能可能好，但在真实环境中则就不好了。

    PSCache

    PSCache是数据库连接池的关键指标。在Oracle中，类似SELECT NAME FROM USER WHERE ID = ?这样的SQL，启用PSCache和不启用PSCache的性能可能是相差一个数量级的。Proxool是不支持PSCache的数据库连接池，如果你使用Oracle、SQL Server、DB2、Sybase这样支持游标的数据库，那你就完全不用考虑Proxool。

    PSCache-Oracle-Optimized

    Oracle 10系列的Driver，如果开启PSCache，会占用大量的内存，必须做特别的处理，启用内部的EnterImplicitCache等方法优化才能够减少内存的占用。这个功能只有DruidDataSource有。如果你使用的是Oracle Jdbc，你应该毫不犹豫采用DruidDataSource。

    ExceptionSorter

    ExceptionSorter是一个很重要的容错特性，如果一个连接产生了一个不可恢复的错误，必须立刻从连接池中去掉，否则会连续产生大量错误。这个特性，目前只有JBossDataSource和Druid实现。Druid的实现参考自JBossDataSource，经过长期生产反馈补充。
    

 3 数据库优化问题
 
     mysql的优化
     1.数据库（表）设计合理
     2.sql语句的优化（索引，常用小技巧）
     3.数据库的配置（把缓冲设置大）
      对于innodb
      innodb_additinal_mem_pool_size = 64 m 
      innodb_buffer_pool_size = 1G 
      对于myisam,需要调整key_buffer_size
     4.适当硬件配置和操作系统
      a.如果机器内存大于4G，则使用64为操作系统
      b.读写分离
       如果数据库压力很大，一台机器支撑不了，那么可以用mysql复制实现多台机器同步，将数据库的压力分散。
       
 
 4 数据库设计的三范式
 
     1NF： 原子性
     2NF： 唯一性 （在满足1NF的基础上）
     3NF： 字段冗余性的约束，他要求字段没有冗余（在满足2NF的基础上）
	逆范式：为什么需要逆范式
   
   （相册功能对应数据库设计）
	冗余规则： 1：N 应当尽可能把逆范式的内容放在少的这边
 
 5 SQL优化
  
  面试题
  SQL语句有几类
  ddl 数据库定义语句  [cretae alter drop]
  dml(数据库操作语句) [insert delete update]
  select
  dtl(数据库事务语句) [commit rollback savepoint]
  dcl（数据库控制语句）[grant revoke]
  1.show status 了解各种SQL的执行频率
  该命令可以显示你的MYSQL数据库的当前状态，我们主要关心的是“COM”
 开头的指令 [Com_select ,Com_deltete,Com_insert,Com_update]
 show status like 'Com%' <=> show  session status like 'Com%'
 show global status  like 'Com%'; // 显示数据库从启动到查询的次数
 
 显示链接数据库次数
 show status like 'Connections';
 服务器工作的时间
 show status like 'Uptime';(单位秒)
 慢查询的次数（默认是10秒）
 show status like 'Slow_queries';
 显示查看慢查询的情况
 show variables like 'long_query_time';

         select * from emp where emp.empno=123456;
        /* 如何在一个项目中，找到慢查询的select,mysql数据库支持把慢查询语句，记录到日志中，程序员分析
        （但是注意，默认情况下不启动）
        1.要怎样启动mysql

        2. 如果你的数据库的存储引擎是MyISAM 的，当创建一个表，后三个文件 *.frm记录表结构，*.myd 数据信息 *.myi 记录表索引
         启动 xx>bin\mysqld.exe -slow-query-log 
        3.测试 
          select * from emp where empno=123456;

        快速体验 在emp表的empno建立索引

        alter table emp add PRIMARY KEY(empno) ;
        然后在查速度明显变快
        小结：
        1.可以加快查询速度，但是是以牺牲 删除修改添加为代价。（牺牲dml为代价）
        2.index信息是存在*.myi文件中 磁盘占用

        介绍一款非常重要的工具

        explan 这个工具可以对一个sql语句进行分析，可以预测你的sql语句执行效率

        删除主键索引 alter table emp drop primary key ;

        那些裂伤合适添加索引
        1.较频繁的作为查询条件字段应该创建索引
        2.唯一性太差的字段不适合单独创建索引，即使频繁的作为查询条件
        3.更新非常频繁的字段不适合创建索引
        4.不会出现在where子语中的字段不适合创建索引

        如何查询某表的索引
        show indexes from 表名；

        索引的类型
        1.主索引 （把某列设为主键，则就是主键索引）
        2.唯一索引（unique）（即该列具有唯一性同时又是索引）
        3.Index (普通索引)
        4.全文索引(FULLTEXT) （只有MYISAM支持）sphinx+中文分词 coreseek 综合使用==>复合索引
        5.复合索引 多列合在一起
        create index myind on 表名(列1,列2) （索引只有用到了最左边的，后面的才会生效）

        如何创建索引
        1.建立索引
        如果创建的是 unique/index/fulltext 
         a.create [UNIQUE|FULLTEXT] index 索引名 on 表名(列名...)
         b.alter table 表名 add index 索引名(列名);
        //如果要添加主键索引
        alter table 表名 add PRIMARY KEY (列。。。);
        2.删除索引
         a.drop index 索引名 on 表名
         b.alter table 表名 drop index index_name ;
         c.alter table 表名 drop primary key ;
        3.显示索引
          show INDEXes from 表名
          show KEYS from 表名 
          desc 表名 

        使用索引的注意事项
        使用like语句，%打头的不会生效。
        1.如果条件中有or,即使条件中有索引也不会生效。
        2.对于多列索引，不是使用的第一部分，则不会使用索引。
        3.like 查询是以%打头
        4.如果列数据中是字符串类型，如果没有用双引号括起来，则不会使用索引。
        5.如果mysql估计使用全表扫描比索引扫描快，则不使用索引。

        查看索引的使用情况
        如何检测索引是否有效
        show status like 'Handler_read%' ;
        注意点：
        handler_read_key: 值越高说明使用的越好
        handler_read_rnd_next; 值越高说明使用的越差

        常用SQL的优化
        对于MYISAM
        alter table table_name disable keys;
        loading data ;
        alter table table_name enable key l
        对于Innodb;
        a.将要导入的数据按照主键排序
        b.set unique_checks=0,关闭唯一性校验
        3.set autocommit=0 ,关闭自动提交

        MyISAM 和Innodb区别是什么
        a.MyISAM 不支持外键  INNODB 支持
        b.MyISAM 不支持事务，不支持外键
        c.对数据的存储处理方式不同（如果存储引擎是MyISAM,则创建一张表，对应着三个文件，如果Innodb,则只有一个文件*.frm，数据
        存储到ibdata1）
        对于MyISAM 引擎的需要定时清理
        OPTIMIZE table 表名 

        优化group by 语句
        常见的SQL优化手法
        1.使用order by null 禁用排序。
          比如select * from dept GROUP BY dname ORDER BY NULL
        2.有些情况下，可以使用连接来替代子查询
          因为使用join,mysql需要在内存中创建临时表
          如果想要含有or的查询语句中利用索引，则or之间的每个列都需要加索引
          如果没有索引，则应该考虑增加索引
        select * from 表名 where 条件1 = ？ or 条件2 =  ？
        3. 在精度要求高的应用中，建议使用定点数(DECIMAL)来存储数值，以保证结果的准确性,
           100000000.32 万
           create table salary(t1 FLOAT(10,2)) ; //会丢失精度
           create table salary(t1 DECIMAL(10,2)) ; //不会丢失精度
        4.日期类型要根据实际需要的选择能够满足应用的最小存储的早期类型。
        5.如果对于存储引擎是MyISAM 的数据库，如果经常做删除和修改记录的操作，要定时执行 OPTIMIZE table table_name；功能对表进行碎片整理


         mysql优化 表的垂直分割和水平分割
         1.水平分割 横切记录
         2.垂直分割  横切字段
         选择字段 保小不保大
         保存图片或则文件时系统存储
         数据库只存储路径。图片和文件存放在文件系统，甚至单独的放在一台服务器（图床）

         数据库参数配置
         1. 

        */

        explain select * from emp where empno = 1 \G; 
 
 浅谈读写分离的原理
 
 目的： 给大型网站缓解查询压力
 
 过程： 
   
 request->sql语句->服务器amoeba(服务器)->dml->master mysql->（独立服务器）>同步->读数据库（另一台服务器）->client
 
 request->sql语句->服务器amoeba(服务器)->select->->读数据库（另一台服务器）->client
 