# shell

shell脚本直接在命令行中执行，也可以组成文件当作脚本文件实现复用。



常见shell脚本种类：

* Bourne Shell(/usr/bin/sh或者/bin/sh)
* Bourne Again Shell(/bin/bash)
* C shell(/usr/bin/csh)
* ...



# bash

Linux一般默认使用bash



```bash
#! /bin/bash
```

文件开头需要知名bash为脚本解释器



# 关于注释

```bash
# 这是一行注释

:<<EOF
第一行注释
第二行注释
第三行注释
EOF
```



# 变量

* 定义以及使用变量

for example:

```bash
name=zxw
echo ${name} # 输出变量，可以去掉{}，但如果是变量和字符串混合输出最好加上{}
```



### 只读变量

* 使用readonly或者declare可以将变量变为只读

```bash
readonly name
declare -r name
```

以上两种写法都可以



### 删除变量

* unset可以删除变量

```bash
unset name
echo $name #这时会输出空行
```



### 变量类型

* 自定义变量(局部变量)：子进程不能访问的变量
* 环境变量(全局变量)：子进程可以访问的变量



自定义变量改为环境变量

```bash
export value # 第一种方法
declare -x value # 第二种方法
```



环境变量改为自定义变量

```bash
declare +x value # 改为自定义变量
```



### 字符串

单引号与双引号的区别：

* 单引号中的内容会原样输出，不会执行，不会取变量
* 双引号中的内容可以执行、可以取变量



获取字符串长度

```bash
echo ${#string} # 输出string的长度
```



提取子串

```bash
echo ${string:0:7} # 提取0到7的子字符串
```





### 默认变量

#### 文件参数变量

```bash
echo $0 # 文件名(包含路径)
echo $1 # 第一个参数
echo $2 # 第二个参数
...
echo ${10} # 第十个参数，多位数需要用{}包起来
...
```



| 参数        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| $#          | 代表文件传入的参数个数，如上例中值为4                        |
| $*          | 由所有参数构成的用空格隔开的字符串，如上例中值为"$1 $2 $3 $4" |
| $@          | 每个参数分别用双引号括起来的字符串，如上例中值为"\$1" "\$2" "\$3" "\$4" |
| $$          | 脚本当前运行的进程ID                                         |
| $?          | 上一条命令的退出状态（注意不是stdout，而是exit code）。0表示正常退出，其他值表示错误 |
| $(command)  | 返回command这条命令的stdout（可嵌套）                        |
| \`command\` | 返回command这条命令的stdout（不可嵌套）                      |







# 数组

只支持一维数组，可以存放多个不同类型的值，初始化不需要指明数组大小



#### 定义

* 数组用小括号表示，元素之间用空格隔开

```bash
array=(1 abc "def" zxw)
```

* 可以直接定义数组中某个元素的值

```bash
array[0]=1
array[1]=abc
array[2]="def"
array[3]=zxw
```



#### 读取数组中某个元素的值

* 格式

```bash
${array[index]} # "${#array[*(或者@)]}"可以获得数组长度
```







# expr

```bash
expr 表达式 # 该命令求表达式的值
```

* 用空格隔开每一项
* 用防线杠放在shell特定的字符面前(发现表达式运行错误是，可以试试转义)
* 对包含空格和其他特殊字符的字符串要用引号括起来
* expr会在stdout中输出结果，如果为逻辑关系表达式，则结果为真，`stdout`为1，否则为0

* expr的`exit code`：如果为逻辑关系表达式，则结果为真，`exit code`为0，否则为1



#### 字符串表达式

* ```bash
  legth STRING
  # 返回STRING的长度
  ```

* ```bash
  index STRING CHARSET
  # CHARSET中任意单个字符在STRING中最前面的字符位置，下标从1开始。如果在STRING中完全不存在CHARSET中的字符，则返回0
  ```

* ```bash
  substr STRING POSITION LENGTH
  # 返回STRING字符串中从POSITION开始，长度最大为LENGTH的子串。如果POSITION或LENGTH为负数，0或非数值，则返回空字符串。
  ```

example

```bash
str="Hello World!"

echo `expr length "$str"` # ``不是单引号，表示执行该命令，输出12
echo `expr index "$str" aWd` # 输出7，下表从1开始
echo `expr substr "$str" 2 3` # 输出ell
```



#### 整数表达式

expr支持普通的算术操作，算术表达式优先级低于字符串表达式，高于逻辑关系表达式

* ```bash
  + - # 加减运算。两端参数会转换为整数，如果转换失败则报错
  ```

* ```bash
  * / % # 乘、除、取模运算。两端参数会转换为整数，弱国转换失败则报错
  ```

* ```bash
  () #可以该表优先级，但需要用反斜杠转义
  ```



#### 逻辑表达式

```bash
1. |
2. & # 如果两个参数都非空且非0，则返回第一个参数，否则返回0
3. < <= = == != >= >
3. () # 可以表示该表优先级，但需要用反斜杠转义
```







# read

用于从标准输入中读取单行数据。当读到文件结束符时，`exit_code`为1，否则为0

参数说明

* ```-p```:后面可以接提示信息
* ```-t```:后面跟秒数，定义输入字符的等待时间，超过等待时间后自动忽略此命令(单位是秒？{可能})







# echo

用于输出字符串



#### 显示普通字符串

```bash
echo Hello World # 引号可以省略
```



#### 显示转义字符

```bash
echo \"Hello World\" # 可以省略双引号
```



#### 显示变量

```bash
name=zxw
echo "My name is $zxw" # 输出My name is zxw
```



#### 显示换行

```bash
echo -e "Hi\n" # -e 开启转义
echo "zxw"

:<<EOF
OUTPUT:
Hi

zxw
EOF
```



#### 显示不换行

```bash
echo -e "Hi \c"
echo "zxw"

:<<EOF
OUTPUT:
Hi zxw
EOF
```



#### 显示结果定向至文件

```bash
echo "Hello World" > output.txt # 将内容以覆盖的方式输出到output.txt中
```



#### 原样输出，不进行转义或取变量(用单引号)

```bash
name=zxw
echo `$name\"`

:<<EOF
OUTPUT:
$name\"
EOF
```



#### 显示命令的执行结果

```bash
echo `date`

:<<EOF
OUTPUT:
Sat May 6 20:59:12 CST 2023
EOF
```







# printf

用于格式化输出，类似于c/c++中的printf函数

默认不会再字符串末尾添加换行符

命令格式

```bash
printf format-string [arguments...]
```







# 逻辑运算符&&和||

表达式的exit code为0，表示为真；为非零，表示为假。（与c/c++中的定义相反）







# test

* 用于判断文件类型，以及对变量作比较
* 用exit code(表达式的exit code为0，表示为真；为非零，表示为假。)返回结果，而不是使用stdout



#### 文件类型判断

命令格式

```bash
test -e filename # 判断文件是否存在
```

| 测试参数 | 代表意义     |
| -------- | ------------ |
| -e       | 文件是否存在 |
| -f       | 是否为文件   |
| -d       | 是否为目录   |

 

#### 文件权限判断

命令格式

```bash
test -r filename # 判断文件是否可读
```

| 测试参数 | 代表意义       |
| -------- | -------------- |
| -f       | 文件是否可读   |
| -w       | 文件是否可写   |
| -x       | 文件是否可执行 |
| -s       | 是否为非空文件 |



#### 整数间的比较

命令格式

```bash
test $a -eq $b # a是否等于b
```

| 测试参数 | 代表意义       |
| -------- | -------------- |
| -eq      | a是否等于b     |
| -ne      | a是否不等于b   |
| -gt      | a是否大于b     |
| -lt      | a是否小于b     |
| -ge      | a是否大于等于b |
| -le      | a是否小于等于b |



#### 字符串比较

| 测试参数          | 代表意义                                               |
| ----------------- | ------------------------------------------------------ |
| test -z STRING    | 判断STRING是否为空，如果为空，则返回true               |
| test -n STRING    | 判断STRING是否为非空，如果非空，则返回true(-n可以省略) |
| test str1 == str2 | 判断str1是否等于str2                                   |
| test str1 != str2 | 判断str1是否不等于str2                                 |



#### 多重条件判断

命令格式

```bash
test -r filename -a -x filename
```

| 测试参数 | 代表意义                                           |
| -------- | -------------------------------------------------- |
| -a       | 两条件是否同时成立                                 |
| -o       | 两条件是否至少一个成立                             |
| !        | 取反。如test ! -x file, 当file不可执行时，返回true |



# 判断符号[]

`[]`与`test`用法几乎一摸一样，更常用与`if`语句中。另外`[[]]`是`[]`的加强版，支持的特性更多。

example:

```bash
[ 2 -lt 3 ] # 为真，返回值为0
echo $? # 输出上个命令的返回值，输出0
```

* `[]`内的每一项都要用空格隔开
* 中括号内的变量，最好用双引号括起来
* 中括号内的常熟，最好用单或双引号括起来

```bash
name="hello zxw"
[ $name == "hello zxw" ] # 
[ "$name" == "hello zxw" ]
```







# 判断语句

#### if...then形式

类似于`c/c++`中的`if-else`语句



#### 单层if

命令格式

```bash
if condition
then
	语句1
	语句2
	...
fi
```



#### 单层if-else

命令格式

```bash
if condition
then
	语句1
	语句2
	...
else
	语句1
	语句2
	...
fi
```



#### 多层if-elif-elif-else

命令格式

```bash
if condition
then
	语句1
	语句2
	...
elif condition
then
	语句1
	语句2
	...
elif condition
then
	语句1
	语句2
	...
else
	语句1
	语句2
	...
fi
```



#### case...esac形式

类似于`c/c++`中的`switch`语句

命令格式

```bash
case $变量名称 in
	值1)
		语句1
		语句2
		...
		;; # 类似于c/c++中的break
	值2)
		语句1
		语句2
		...
		;;
	*) # 类似于c/c++中的default
		语句1
		语句2
		...
		;;
esac
```







# 循环语句



#### for...in...do...done

命令格式

```bash
for var in val1 val2 val3
do
	语句1
	语句2
	...
done
```

example

```bash
#OUTPUT
:<<EOF
a
2
cc
EOF
for i in a 2 cc
do
	echo $i
done



#输出档期那路径下的所有文件名，每个文件名一行
for file in `ls`
do
	echo $file
done



#输出1-10
for i in $(seq 1 10) # 可以用 {1..10} (字母也可以如{a..z})
do
	echo $i
done

```



#### for((...;...;...)) do...done

命令格式

```bash
for ((expression; condition; expression))
do
	语句1
	语句2
	...
done
```

example

```bash
for ((i=1; i<=10; i++))
do
	echo $i
done
```



#### while...do...done循环

命令格式

```bash
while condition
do
	语句1
	语句2
	...
done
```

example

```bash
while read name
do
	echo $name
done
```



#### until...do...done循环

当条件为真时结束

命令格式

```bash
until condition
do
	语句1
	语句2
	...
done
```

example

```bash
until [ "${word}" == "yes" ] || [ "$word" == "YES" ]
do
	read -p "Please input yes/YES to stop this program " word
done
```



#### break

跳出当前一层循环，注意与`c/c++`不同的时：`break`不能跳出`case`语句

example

```bash
while read name
do
	for ((i=1;i<=10;i++))
	do
		case $i in
			8)
				break
				;;
			*)
				echo $i
				;;
		esac
	done
done
```

该示例每读入非EOF的字符串，会输出一遍1-7。

该程序可以输入`Ctrl+d`文件结束符来结束，也可以直接用`Ctrl+c`杀掉该进程



#### continue命令

跳出当前循环

example

```bash
for ((i=1;i<=10;i++))
do
	if [ `expr $i % 2` -eq 0 ]
	then
		continue
	fi
	echo $i
done
```

给程序输出1-10所有的



#### 死循环的处理方式

直接关闭进程

* 使用`top`命令找到进程的PID
* 输入`kill -9 PID`即可关掉此进程







# 函数

* `return`的返回值与`c/c++`不同，返回的是`exit code`，取值为0-255，0表示正常结束。

* 如果想获取函数的输出结果，可以通过`echo`输出到`stdout`中，然后通过`$(function_name)`来获取`stdout`中的结果。
* 函数的`return`值可以通过`$?`来获取。

命令格式

```bash
[function] func_name(){
	语句1
	语句2
	...
}
```



#### 函数的传入参数

在函数内，`$1`表示第一个输入参数，`$2`表示第二个输入参数，依次类推。

注意：函数内的`$0`仍然时文件名，而不是函数名。







# 文件重定向

每个进程默认打开3个文件描述符

* `stdin`标准输入，从命令行读取数据，文件描述符为0
* `stdout`标准输出，向命令行输出数据，文件描述符为1
* `stderr`标准错误输出，向命令行输出数据，文件描述符为2

可以用文件重定向将这三个文件重定向到其他文件中。



#### 命令列表

| 命令             | 说明                                      |
| ---------------- | ----------------------------------------- |
| command >  file  | 将`stdout`重定向到`file`中                |
| command < file   | 将`stdin`重定向到`file`中                 |
| command >> file  | 将`stdout`以追加方式重定向到`file`中      |
| command n> file  | 将文件描述符`n`重定向到`file`中           |
| command n>> file | 将文件描述符`n`以追加方式重定向到`file`中 |







# 引入外部脚本

类似于`c/c++`中的`include`操作， `bash`也可以引入其他文件中的代码

语法格式

```bash
. filename # 注意点和文件名之间有一个空格
或
source filename
```

