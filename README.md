数据库相关操作

* 数据库登录 mysql -uroot -p(有密码的，没有密码的把-p去掉)
* 清除数据库表 TRUNCATE TABLE Lagou1;
* 数据库语句
```
sql = '''insert into Lagou1 (work_name, work_address, work_sendtime, work_money, work_experience, company_name, compay_industry, work_logo, nice_str, work_info) values(%s, %s, %s, %s, %s, %s, %s, %s, %s, %s)'''

cur.execute(sql,  (work_name, work_address, work_sendtime, work_money, work_experience, company_name, compay_industry, work_logo, nice_str, work_info))
```

# 数据库插入中文失败的结局办法
```
MySQL ERROR 1366(HY000):Incorrect string value，在往数据库中插入中文的时候会出现。
这也就是编码问题，网上大部分都是说设置下配置文件中的设置，而可悲的是在我的环境中配置文件是不允许修改，或者说和其他版本的不同。

大家都知道中文常用的编码方式是gbk或者utf-8。我建议是使用utf-8这种编码方式，因为大势所趋。
我们有时候设置了mysql的配置文件，而创建出来的 database ，table 的character 任然为默认的 latin1。
我们可以通过 show create database/table database_name/table_name；来查看所创的库和表的character。会出现ERROR 1366错误的，编码上就可能存在问题。如果编码问题，那一下内容就不用看了，我的这个笔记帮不了你。

解决方法有好几个，我也是百度了很久，把两个成功的方法罗列在下面，方便自己方便他人。
方法一：在创建数据的时候设置好character ，这样再创建 table的时候会和database的编码方式相同。
CREATE DATABASE <DATABASE_NAME> CHARACTER SET <CODE>;
当然如果database创建的时候忘了设置，在创建表的时候任然可以设置character来补救。
CREATE TABLE <TABLE_NAME> (.......) CHARACTER SET <CODE>;

方法二：如果你很不辛的在创建database和table的时候都忘了设置character，那就可以使用方法二
alter table <tbname> convert to charset gbk或utf8;
```

# 如何在MySQl数据库中给已有的数据表添加自增ID？
1、给某一张表先增加一个字段,这里我们就以node_table这张表来举例，在数据库命令行输入下面指令 ：
```
alter table node_table add id int
```
2、更改id字段属性为自增属性，在数据库命令行输入下面指令 ：
```
alter table node_table change id id int not null auto_increment primary key; 
```

