# Shell

## 1 Shell 程序和参数

**执行Shell程序的方法**

1. `sh < Shell_program`
2. `sh Shell_program`
   * `sh -v Shell_program`：显示读入的每个命令行
   * `sh -x Shell_program`：显示执行的每个命令和变量的取值
   * `sh -n Shell_program`：检查命令的语法
   * `sh -u Shell_program`：显示引用了未设置变量的错误
3. 首先为命令文件建立执行许可：`chmod a+x Shell_program`，以后执行时只需输入`Shell_program`即可

**位置参数**

`Shell_program pram1 pram2...`：在程序中，`$1`对应`pram1`，`$2`对应`pram2`，最多9个位置参数

## 2 Shell变量

### 2.1 用户定义变量

用户定义变量必须以字母或下划线开始，可以包括字母、下划线、数字

能用赋值语句置初值、重置值，`=`两边不能有空格

本质上，用户定义变量都是字符串

赋给变量的值包括了空格、制表符、换行符时，需要将值放在一对引号内，引号本身不是值的一部分，`OS="Operating System"`

通过变量名前加`$`来引用变量值，`echo $OS`

目录名也能存放在Shell变量中，`workdir=/usr/local/work`

Shell变量与其它字母、下划线、数字之间必须有空格、制表符、”.“、”;“等非字母、非下划线、非数字符号作分隔符。如果希望在显示的格式中不存在分隔符，就要用{}标记Shell变量，以使系统能够识别

可以用`readonly`命令来标记Shell变量的值是不能改变的：`readonly OS`

### 2.2 系统定义变量

在用户登录时，Shell对一些变量进行说明和初始化，这些变量在整个的用户工作环境中都起作用，包括任何一层子Shell，故也称作环境变量

* `HOME`：存放用户主目录
* `PATH`：Shell查找命令时在文件系统中的查找路径
  * 如果用户输入一个不含有路径名的命令时，Shell要按照`PATH`变量所确定的目录顺序查找一个同名的可执行文件
  * `PATH`中一般包含用`:`分开的如下几个查找路径：`:/bin:/usr/bin`，第一个`:`之前的空串(或`.`)表示当前目录
  * 很多用户习惯将自己编写的可执行程序放在主目录下的`bin`目录中，那么可重置`PATH`变量：`PATH=$HOME/bin:$PATH`
  * 存放最常用命令的目录放在靠前面的位置上
* `PS1`：Shell的系统主提示符，一般为`$`
* `PS2`：Shell的辅助提示符，假设用户输入的一个命令在一行内还没有结束，即使按了回车键，Shell也不会执行，Shell将在下一行头显示辅助提示符(一般为`>`)，以要求另外的输入
* `LD_LIBRARY_PATH`：连接动态库时的搜索路径
* `CDPATH`：`cd`命令要查找的目录表，其格式类同`PATH`
* `LOGNAME`：用户的注册名
* `SHELL`：Shell程序的路径名
* `IFS`：内部字段分隔符，通常是空格、制表符、换行符，这个分隔符将命令行按字段分隔开来
* `MANPATH`：man命令的搜索路径

可以用`set`命令显示所有用户定义变量和系统定义变量

### 2.3 Shell定义变量

这类变量一般是只读的

**Shell参数变量**

* `$0`：命令名，在Shell程序内可以用`$0`获得调用该程序的名字
* `$1, $2...`：Shell程序的位置参量
* `$#`：位置参量的个数，不包括命令名
* `$*`：所有位置参量，即相当于`$1, $2...`
* `$@`：与`$*`基本相同，用双引号转义时，`"$@"`能分解成多个参数，而`"$*"`合并成一个参数

如果命令行上带有多于9个的参量，可以通过Shell内部命令`shift`访问9个以上的参数。`shift n`命令左移参数表，即将最左边的位置参数丢弃，对剩下的位置参数重新从1开始编号

可以通过内部命令`set`对位置参数置值，如`set abc def ghi`，使Shell程序的位置参量`$1`、`$2`、`$3`分别置值为abc、def、ghi

**Shell状态变量**

* `$?`：上一命令的返回代码，如命令执行成功返回真值(0)，否则返回假值(1)
* `$$`：当前命令的进程标识数
* `$!`：Shell执行的最近后台进程标识数
* `$-`：Shell标志位组成的字符串，可由Shell传递来，或由`set`命令设置，如正在跟踪输出，则值可能为xv

### 2.4 参数替换

参数替换是指Shell变量的值取决于另一个Shell变量的值

* `${parameter-word}`：如变量`parameter`已置值，则取该值，否则取值为`word`
  * `DIR=${1-$HOME}`：当存在位置参数时，变量取值为`$1`，否则取值为用户的主目录
* `${var=word}`：如果变量`var`没置值，则`var`置为`word`，如已置值，则不变
  * 用这种方法不能为位置参数置值
* `${parameter?message}`：如变量`parameter`已置值，则取该值，否则在标准错误输出中印出信息`message`，返回FALSE代码(1)。如`message`默认，则印出标准信息
* `${parameter+word}`：如变量`parameter`已设置，置换值取`word`，否则置换值为空，两种情况都不影响原`parameter`的值

### 2.5 引号机制

单引号：单引号中的任何字符(单引号本身除外)就是这个字符本身

双引号：

* 作用与单引号相似，但有几个字符还存在特殊含义，它们是`$`、`\`、单双引号、用于命令替换的反撇号
* 命令替换里所使用的特殊字符都不受外层双引号的影响，如`*`、`?`、`[`、`]`等

反撇号：

* 任何一个命令行都可放在反撇号里，包含在反撇号中的命令由Shell执行，使得命令的输出替换括在反撇号里的命令行包括反撇号本身
* 反撇号中特殊字符的含义不受影响

## 3 测试和求值

**测试**

* `test`是一个最常用、最重要的测试命令，测试一个表达式，返回一个条件码以供控制语句选择一个执行分支
* 所有测试如满足测试条件就返回真值(返回代码为0)，否则返回假值(返回代码为1)
* 文件测试
  * `-r file`：用户可读的文件file存在
  * `-w file`：用户可写的文件file存在
  * `-x file`：用户可执行的文件file存在
  * `-f file`：普通文件file存在
  * `-s file`：文件file存在，且长度大于0
  * `-d dir`：目录dir存在
  * `-c file`：字符设备文件file存在
  * `-b file`：块设备文件file存在
  * `-p file`：有名管道文件file存在
  * `-l file`：符号链接文件file存在
* 串测试
  * `-z string`：字符串string长度等于0
  * `-n string`：字符串string长度大于0
  * `string`：string非空
  * `str1=str2`：两者相等
  * `str1!=str2`：两者不等
* 数值比较
  * `-eq`：等于
  * `-ne`：不等于
  * `-gt`：大于
  * `-ge`：大于等于
  * `-lt`：小于
  * `-le`：小于等于
  * 进行数值比较时，数字前面的空格和0被忽略，系统只处理整数部分，而不管小数部分，能识别负数
* 逻辑运算
  * `!`：非
  * `-a`：与
  * `-o`：或
* 当比较两个Shell串变量或一个Shell串变量与一个字符串时，最好把串变量括在双引号内
* `test`与`[   ]`是同一Shell内部程序，后者主要用于控制语句。`[`与`]`是程序名，与相邻符号名之间要留出空格

**求值**

* `expr`也能用于参数比较
* `expr`主要用于对数值求值
* 在`expr`中可以使用的操作符有`+`、`-`、`*`、`/`、`%`
* 每个操作数、操作符之间必须用空格或制表符隔开
* 乘法`*`需转义，写为`\*`

## 4 控制结构

### 4.1 顺序控制结构

用`;`分隔命令，形成顺次执行的命令表，如`cd doc;ls -l`

用`&`分隔前后两个命令，并指示前一个命令放在后台执行

用`|`连接两个命令，这两个命令可以并发执行

用`&&`分隔两个命令，只有在前一个命令执行成功后才执行后一个命令

用`||`分隔两个命令，只有在前一个命令执行失败后才执行后一个命令

命令表可用花括号括起来，构成复合命令，复合命令在结构上可看成一个简单命令

用花括号括起来的命令组将组内各条命令的stdout和stderr合并成一个流

用`()`括起来的命令表，在子进程中执行，能合并命令表中各条命令的stdout和stderr，在子进程中命令对环境的改变对返回到父进程的环境没有影响

### 4.2 if语句

```
if command
then
	命令组1
else              (可省略)
	命令组2
fi
```

上述`if`后的测试部分相当于

```
command
if test $? -eq 0       可写成 if [$? -eq 0]
then
...
```

一个管道的出口状态是它最后一个命令的出口状态

如要将`then`写在与`if`同一行，`if`后必须用界符`;`分隔

下列关键字只有作为命令行的首字时才被Shell识别：`if`、`then`、`else`、`elif`、`fi`、`case`、`esac`、`for`、`while`、`until`、`do`、`done`、`{`、`}`

B Shell提供了`if-then-elif...-else-fi`结构

### 4.3 case语句

```
case $var in
模式1)
	命令组1
	;;
模式2)
	命令组2
	;;
...
*)
	默认命令组
	;;
esac
```

`case`命令执行与变量匹配的那个模式相联系的命令组

可用Shell元字符构成各种模式

当有多个模式对应于相同的命令组时，可用竖线组合这些模式，每经过一次`case`结构，只能执行一个模式所对应的命令组

### 4.4 for语句

```
for 变量 [in <值表>]
do
	命令组
done
```

`for`语句执行时，依次用`in`后面值表的每项对变量赋值，再执行命令组，直至值表中的所有值都取过一次为止

若`in`部分默认，值表默认值为调用该Shell过程的参数表，即变量依次取Shell位置参数变量的值各1次，相当于`for 变量 in $*`

```
Example

大多数Shell程序需要在命令行上有选择项和参数表，如Shell_program -a -k file1 file2 file3 ...
要检验处理这些选择项和参量中的每一个，可以使用循环

status=TRUE
for arg in $*
do
	case $arg in
		-a)
			process option -a
			;;
		-k)
			process option -k
			;;
		*)
			if test `echo $arg | cut -c1`='-'
			then
				echo "Option: $arg invalid"
				status=FALSE
			elif test -f $arg
				then
					process $arg
				else
					echo "Invalid File Name $arg"
					status=FALSE
			fi
			;;
	esac
done
exit $status
```

### 4.5 while语句

```
while 测试命令
do
	命令组
done
```

如果在命令组中一条命令也没执行，`while`命令则退出循环，其状态值为0

与`if`语句一样，在`while`后的测试命令中最常见的也是`test`命令

```
Example

逆序印出Shell参数

count=$#
cmd=echo
while test $count -gt 0
do
	cmd="$cmd \$$count"
	count=`expr $count - 1`
done                           cmd的值为echo ... $3 $2 $1
eval $cmd											 对自变量进行多重替换，使echo命令重新置值并执行
```

无限循环：

* `while true`，`true`不是一个参数，它是UNIX中最简单的没有任何执行动作的命令，该命令的返回值总是为真(0)
* 也可写成`while [1]`或`while [0]`，因为`test`把任何值的存在作为真

### 4.6 until语句

```
until 测试命令
do
	命令组
done
```

`until`语句在测试命令返回非零状态值时执行命令组

可以用while语句表示

```
while [!测试命令]
do
	命令组
done
```

### 4.7 break、continue、exit、return语句

`break [n]`：从for或while循环中退出，参数n说明要退出n层循环

`continue`：重新转到循环的开始

`exit [n]`：以状态值n退出Shell，若n默认，最后一条命令执行的状态就是退出状态值

`return [n]`：以返回值n退出一个函数，若n默认，返回值是最后一条命令执行的状态值

## 5 递归和Shell函数

**递归**

```
Example

lstrees采用了递归技术，列出参数表中指定的所有目录子树下的全部目录，当lstrees不带参数时，则显示当前目录子树下的所有目录

if test $# -eq 0; then
	lstrees .
else
	for i in $*; do
		if test -d $i; then
			echo $i
			(	cd $i
				for j in *; do
					lstrees $j
				done	)
    fi
  done
fi
```

**Shell函数**

* Shell程序是存储在文件中的命令集合，Shell函数存储在内存中，而不是存储在一个文件中，因此对Shell函数的存取比对Shell程序的存取快得多

* Shell要对Shell函数进行分析和预处理，因此执行Shell函数比执行Shell过程快

* Shell是在本进程中直接执行Shell函数，而不是通过生成一个子进程执行Shell程序，降低了系统开销

* 用户可以在`.profile`或Shell程序中定义Shell函数，或直接从Shell命令行输入

  ```
  up(){
  	who
  	ps -u $user
  }
  ```

* 在Shell函数中可以引用Shell变量和位置参数，函数体位于一对`{}`内，用户输入`}`后，就退出了输入状态

* Shell函数作用范围是从函数定义之后到当前Shell终止

* Shell函数也能递归调用

* 可以用`set`命令看到所有已定义的Shell函数

* 可以用命令`unset 函数名`去除一个已定义的Shell函数

* 当用户退出系统后，Shell不保留所有的Shell函数

## 6 Shell内部命令

**从标准输入中读入一行**

* `read`命令常用于从标准输入或通过I/O转向从文件中输入一行
* `read`命令将输入内容存入Shell变量中
* 当读到文件底，或从键盘读入一个Ctrl+D时，`read`返回假值(1)，否则返回真值(0)，这个返回结果可用于控制语句中

```
Example

if [$# -eq 0]; then
	echo -n "Enter filename:"
	read filename
else
	file=$1
fi

read命令由管道读入标准输入
for file in chapter? chapter??
do
	cat $file |
		while read fline
		do
			process $fline
			...
		done
done
```

**求值和执行参数**

* `eval`命令对其参数进行代换和求值，然后按它们就像是Shell程序一部分那样执行代换后的命令串

```
Example

inputfilter="cmd1 | cmd2"
outcmd="cmd3 > $result"
eval "$inputfilter | command | $outcmd"
等价于执行
cmd1 | cmd2 | command | cmd3 > $result
```

* 在某些条件下，`eval`是执行某些命令串的唯一方法

**点命令与`exec`命令**

* `.`是Shell的一个内部命令，使当前Shell直接执行参数所指定的Shell程序，而不是产生子Shell执行该程序
* 当多个Shell命令共享一组Shell变量时，用`.`命令非常有效
* 格式为`. Shell_program`
* `.`命令执行的Shell程序不必具有执行权限
* `exec`也是内部命令，格式为`exec program`
* 将被调用的程序覆盖当前执行的程序，从而执行参数指定的程序
* 用`exec`命令执行，可以减少程序运行时所占的空间，当控制不需要返回到原执行程序时可用`exec`命令
* `exec`执行的程序可以是Shell程序、UNIX命令、二进制可执行程序，都必须具有执行权限

**信号自陷**

* `trap`命令可以处理由信号引起的软中断
* `trap`命令可以使用的信号由信号表定义
* 当一个Shell进程收到信号后能执行信号表中预定义的处理动作，也能执行由`trap`命令指定的处理动作
* `trap`命令的格式：`trap "命令表" 信号表`

```
Example

trap "rm temp*; exit 1" 1 2 3 15
当Shell收到了一个信号1(电话挂断)、信号2(中断Break)、信号3(退出Quit)或信号15(终止进程kill)后，删除临时文件temp*，并以一个假值返回码主动地退出
```

* 为了忽略或禁止信号，可执行命令表为空的`trap`命令：`trap "" 信号表`
* 引入`trap`的另一个目的是在中断信号发生时删除该程序所用的临时文件，然后终止程序的执行，以免系统被”弄脏“

**here文件**

* here文件用于将标准输入暂时重定向到后面的Shell程序行

```
Example

cat << eof
	This is an example of
	here file
eof
```

* here文件开头由`<<`指示，其后面的符号可任意选择，用于指示here文件的结束
* 在here文件中允许出现Shell变量和参数

## 7 Shell环境

命令开始执行时，所有的Shell变量和相应的值构成了命令的环境，又称为当前Shell环境

这个环境包括本命令从父进程继承来的变量和命令行上的命令名和位置参数

Shell进程传给子进程的Shell变量仅是那些作为`export`命令参数的变量

`export`把这些变量放在当前Shell和所有子进程的环境中

子进程可以存取其环境中任何变量的值，但是重置变量值并不影响环境，而仅仅影响正在运行的进程

要得到当前Shell的所有环境变量表，可输入不带参数的`export`命令

为了得到当前环境下Shell变量的名字-值，可执行命令`env`

当用户在UNIX系统登录时，几个文件被用于为用户和Shell建立环境

* `/etc/profile`：系统范围的环境设置，首次被执行

* `$HOME/.profile`：sh或ksh用户的环境设置，其次执行(定义Shell函数的好地方)

  ```
  Example
  
  PATH=$PATH:$HOME/bin
  PS1="/UNIX>
  PS2="More Input!>
  export PATH PS1 PS2
  dir(){
  	ls -l
  }
  ```

  

































