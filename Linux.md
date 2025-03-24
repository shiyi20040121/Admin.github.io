# Linux磁盘分区、挂载

### 一、Linux分区与文件系统的关系

1.Linux来说无论有几个分区，分给哪一目录使用，它归根结底就只有一个根目录，一个独立且唯一的
文件结构，Linux中每个分区都是用来组成整个文件系统的一部分。
2.Linux采用了一种叫”载入“的处理方法，它的整个文件系统中包含了一整套的文件和目录，且将**一
个分区和一个目录联系起来。这时要载入的一个分区将使它的存储空间在一个目录下获得。**

例如：sda是硬盘，每个硬盘分区都关联了一个目录

![image-20250226151332955](img/image-20250226151332955.png)

总结：目录就只是目录，真正存放数据的是分区。

### 二、硬盘说明

Linux硬盘分IDE硬盘和SCSI硬盘，目前基本上是SCSI硬盘。

1.对于IDE硬盘，驱动器标识符为“hdx~”，其中“hd”表明分区所在设备的类型，这里是指IDE硬盘
了。”x”为盘号（a为基本盘，b为基本从属盘，c为辅助主盘，d为辅助从属盘），”~”代表分区，前四个分区用数字1到4表示，它们是主分区或扩展分区，从5开始就是逻辑分区。例，hda3表示为第一个IDE硬盘上的第三个主分区或扩展分区，hdb2表示为第二个IDE硬盘上的第二个主分区或扩展分区。

2.对于SCSI硬盘则标识为“sdx~”，SCSI硬盘是用“sd”来表示分区所在设备的类型的，其余则和
IDE硬盘的表示方法一样。

### 三、查看所有设备挂载情况

```
lsblk 或者 lsblk-f
```

### 四、增加磁盘案例

[059_韩顺平Linux_增加磁盘应用实例_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Sv411r7vd?spm_id_from=333.788.player.switch&vd_source=b73f51d5b05559ddcd960d68192fd224&p=59)

### 五、磁盘情况查询

查询系统整体磁盘使用情况

```
df -h
```

查询指定目录的磁盘占用情况

```
du -h /目录
-s 指定目录占用大小汇总
-h 带计量单位
-a 含文件
--max-depth=1 子目录深度
-c 列出明细的同时，增加汇总值

#案例
du -ha --max-depth=1 /opt
du -hac --max-depth=1 /opt
```

磁盘情况-工作实用指令

1、统计/opt文件夹下文件的个数

```
ls -l /opt | grep "^-" | wc -l
```

"^-" 只看文件

wc -l 统计个数

2、统计/opt文件夹下目录的个数

```
ls -l /opt | grep "^d" | wc -l
```

3、统计/opt文件夹下文件的个数，包括子文件夹里的

```
ls -lR /opt | grep "^-" | wc -l
```

4、以树状显示目录结构

```
tree /home
```

<img src="img/image-20250226154619090.png" alt="image-20250226154619090"  />

# 网络配置







## Linux 组、权限

### 所有者、所在组、其它组

在linux中每个用户必须属于一个组。每个文件都有所有者、所在组、其它组。

谁创建文件，谁就是这个文件的所有者。文件的所有者所在组可以与文件所在组不一样。

假设有两个组，Tom在组1，Bob在组2，Tom创建文件a.txt，Tom就是这个文件的所有者，组1是这个文件的所在组，组2是这个文件的其他组。

### 权限

```
-rwxr--r--  1  root root   94 Feb 22 18:31 aaa.sh
```

第0位：确定文件类型（d，-，l，c，b)

I 是链接(windows的快捷方式) ，d 是目录(windows的文件夹)，- 是普通文件，**c 是字符设备文件(鼠标，键盘)，b 是块设备(硬盘)**

第1-3位：所有者对该文件的权限---User

第4-6位：所在组（同用户组）对该文件的权限---Group

第7-9位：其他用户对该文件的权限---Other

rwx作用到文件

1.[r]代表可读（read）：可以读取，查看

2.[w]代表可写（write）：**可以修改，但是不代表可以删除该文件**，删除一个文件的前提条件是对该文件所在的目录有写权限，才能删除该文件

3.[x]代表可执行（execute)：可以被执行

wx作用到目录

1.[r]代表可读（read）：可以读取，Is查看目录内容

2.[w]代表可写（write）：可以修改，对目录内创建+删除+重命名目录

3.[x]代表可执行（execute）：可以进入该目录

注意文件w权限就行

1  文件：**硬连接数**或目录：**子目录数**

root用户

root组

1213文件大小（字节），**如果是文件夹，显示4096字节**

Feb 2 09:39最后修改日期

abc文件名

### 命令

```
#查看文件所有者
ls -ahl
-----------------------------------------
#创建用户
useradd 用户名

#创建组
groupadd 组名

#创建用户和用户所在组
useradd -g 组名 用户名
-----------------------------------------
#修改文件所有者
chown 用户名 文件名
#修改文件所在组
chgrp 组名 文件名

#修改目录所有者（加-R）
chown -R 用户名 目录路径
chown -R Tom /home/test
#修改目录所在组
chgrp -R 组名 目录路径

#修改文件所有者和所在组
chown 用户名:组名 文件/目录

#改变用户所在组
usermod -g 组名 用户名
#改变该用户登录的初始目录
usermod -d 目录名 用户名 
-----------------------------------------
#删除用户
userdel 用户名


-----------------------------------------
#u所有者、g所在组、o其他人、a所有人（u、g、o的总和）

#修改权限
chmod u=rwx,g=rx,o=x 文件/目录

chmod o+w,a-x 文件/目录

chmod 751 文件/目录
```





多行注释：`:<<! 内容 !`（内容两边要有空格）

## Shell变量

在Linux中，Shell的变量分为系统变量、环境变量和用户自定义变量。

### 系统变量

> $0：当前脚本名称
>
> $n：$1-$9代表第一到第九个参数，十以上的参数需要用大括号包含，如 ${10}
>
> $*：代表令行中所有的参数，所有的参数看成一个整体
>
> $@：代表命令行中所有的参数，把每个参数区分对待
>
> $#：代表命令行中所有参数的个数

```shell
vim var.sh
#!/bin/bash
echo "0=$0 1=$1 2=$2"
echo "所有的参数=$*"
echo $@
echo "参数的个数=$#"

chmod u+x var.sh

./var.sh 100 200
->输出
0=./var.sh 1=100 2=200
所有的参数=100 200
100 200
参数的个数=2
```

> $$：当前进程的进程号（PID）
>
> $!：后台运行的最后一个进程的进程号（PID）
>
> $?：最后一次执行的命令的返回状态。如果这个变量的值为0，证明上一个命令正确执行；非0，不正确

```shell
#!/bin/bash
echo "当前执行的进程id=$$"
#以后台的方式运行一个脚本,并获取他的进程号(加&，就是在后台运行)
/root/wz/var.sh & 
echo "最后一个后台方式运行的进程id=$!"
echo "执行的结果是=$?"

当前执行的进程id=37986
最后一个后台方式运行的进程id=37987
执行的结果是=0
A=100
A=100
A=
B=2
/root/wz/var.sh: line 13: unset: B: cannot unset: readonly variable
C=Sat Feb 22 18:42:35 PST 2025
D=Sat Feb 22 18:42:35 PST 2025
```



### 环境变量

`$HOME`、`$PWD`、`$SHELL`、`$USER`

全局都可以使用

```shell
1.在/etc/profile文件中定义TOMCAT_HOME环境变量
export TOMCAT_HOME=/opt/tomcat

2.让修改后的配置信息立即生效
source /etc/profile

3.查看环境变量TOMCAT_HOME的值
echo $TOMCAT_HOME
->/opt/tomcat

4.在另外一个shell脚本中使用TOMCAT_HOME
echo "tomcat_home=$TOMCAT_HOME"
->/opt/tomcat
```



显示当前shell中所有变量：set

vi显示行号：set -nu

### 自定义变量

> 变量名称可以由字母、数字和下划线组成，但是不能以数字开
>
> 等号两侧不能有空格
>
> 变量名称一般习惯为大写

```shell
#!/bin/bash
#1.定义变量（等号两边不能有空格）
A=100

#2.输出变量
echo A=$A
echo "A=$A"

#3.撤销变量---unset
unset A
echo "A=$A"

#4.声明静态/全局变量---readonly(不能unset)
readonly B=2
echo "B=$B"
unset B

#5.将命令的返回值赋给变量---反引号/$(data)
C=`date`
D=$(date)
echo "C=$C"
echo "D=$D"

#输出
A=100
A=100
A=
B=2
var.sh: line 13: unset: B: cannot unset: readonly variable（报错）
C=Sat Feb 22 04:48:52 PST 2025
D=Sat Feb 22 04:48:52 PST 2025

```



## 运算符

> $((运算式))
>
> $[运算式]
>
> expr m + n （expr中乘法是`\*`；运算符之间要有空格。没有空格当成一个整体输出；expr的结果赋给一个变量，用反引号``）

```shell
#!/bin/bash
echo $(((2+3)*4))
echo $[(2+3)*4]
A=`expr 2 \* 3`
echo $A

20
20
6
```

案例：位置参数变量与运算符的结合

```shell
vim bbb.sh
#!/bin/bash
SUM=$[$1+$2]
echo $SUM

./bbb.sh 1 2

3
```

## 条件判断

> 1.字符串比较=
>
> 2.数的比较：
>
> -lt 小于
>
> -le 小于等于
>
> -eq 等于
>
> -gt大于
>
> -ge大于等于
>
> 3.按照文件权限进行判断
>
> -r有读的权限
>
> -w有写的权限
>
> -x有执行的权限
>
> 4.按照文件类型进行判断
>
> -f文件存在并且是一个常规的文件
>
> -e文件存在
>
> -d文件存在并是一个目录

```shell
#!/bin/bash
#例1：“a"是否等于"a"
if [ "a" = "a" ]
then
	echo "equal"
fi
#相等输出equal，不相等无输出
#例2：23是否大于等于22
if [ 23 -ge 22 ]
then 
	echo "大于"
fi
#例3：/root/wz/aaa.txt目录中的文件是否存在
if [ -f /root/wz/aaa.txt ]
then
	echo "存在"
fi

equal
大于
```

### if语句

案例：eee.sh

```shell
#!/bin/bash
if [ $1 -ge 60 ]
then
	echo "及格"
elif [ $1 -lt 60 ]
then	 
	echo "不及格"
fi
```

```
./eee.sh 70
及格
```

### case语句

> 
>
> ```
> case $变量名 in
> “值1"）
> 执行1
> ;;
> *)
> 都不是，执行
> ;;
> esac
> ```

案例：fff.sh

```shell
#!/bin/bash
case $1 in
"1")
echo "上午"
;;
"2")
echo "下午"
;;
*)
echo "晚上"
;;
esac
```

```
./fff.sh  1

上午
```

### for循环

> 
>
> ```
> for 变量 in 值1 值2...
> do
> 程序
> done
> ```
>
> ```
> for (( 初始值;循环控制条件;变量变化 ))
> do
> 程序
> done
> ```
>
> for

案例1：打印命令行输入的参数（$*和$@的区别）

```shell
#!/bin/bash
for i in "$*"
do 
	echo "num is $i"
done
echo "-----------------------"
for j in "$@"
do 
	echo "num is $j"
done
```

```
./ggg.sh 10 20 30

num is 10 20 30
-----------------------
num is 10
num is 20
num is 30
```

如果在for语句中，$*不加双引号，输出和$@一样。

案例2：从1加到100的值输出显示

```shell
#!/bin/bash
SUM=0
for (( i=1;i<=100;i++ ))
do
	SUM=$[$sum+$i]
done
echo "SUM的值:$SUM"
```

```
./hhh.sh 

SUM的值:100
```

### while循环

> ```
> while [ 条件判断式 ]
> do 
> 程序
> done
> ```

案例：从命令行输入一个数n，统计从1+...+n的值是多少

```shell
#!/bin/bash
SUM=0
i=0
while [ $i -le $1 ]
do
	SUM=$[$SUM+$i]
	i=$[$i+1]
done
echo "SUM=$SUM"
```

```
./iii.sh 100

SUM=5050
```

## read读取控制台输入

> 基本语法
>
> read (选项)（参数)
>
> -p：指定读取值时的提示符
>
> -t：指定读取值时等待的时间（秒），如果没有在指定的时间内输入，就不再等待

```shell
#!/bin/bash
#案例1：读取控制台输入一个NUM值
read -p "请输入一个数NUM1：" NUM1
echo "你输入的NUM1为$NUM1"

#案例2：读取控制台输入一个NUM2值，在10秒内输入
read -t 5 -p "请输入一个数NUM2：" NUM2
echo "你输入的NUM2为$NUM2"
```

```
请输入一个数NUM1：10
你输入的NUM1为10
请输入一个数NUM2：./jjj.sh: line 7: unexpected EOF while looking for matching `"'
./jjj.sh: line 8: syntax error: unexpected end of file
```

## 函数

shell有系统函数，也有自定义函数

### 系统函数

1、basename

```
basename [路径] [后缀]
```

返回完整路径最后/的部分，常用于获取文件名

案例1：请返回/home/aaa/test.txt的“test.txt”部分

```shell
# basename /home/aaa/test.txt
test.txt
# basename /home/aaa/test.txt .txt
test
```

2、dirname

```
dirname [文件绝对路径]
```

返回完整路径最后/的前面的部分，常用于返回路径部分

案例1：请返回/home/aaa/test.txt的/home/aaa

```shell
# dirname /home/aaa/test.txt
/home/aaa
```

### 自定义函数

案例1：计算输入两个参数的和，getSum

```shell
#!/bin/bash
#定义函数
function getSum(){
	SUM=$[$n1+$n2]
	echo "SUM=$SUM"
}
#输入
read -p "请输入一个数n1：" n1
read -p "请输入一个数n2：" n2
#调用自定义函数
getSum $n1 $n2
```

```
请输入一个数n1：10
请输入一个数n2：20
SUM=30
```

## 定时任务调度

## crond任务调度

#### 一、概述

1.任务调度：是指系统在某个时间执行的特定的命令或程序。

2.任务调度分类：

（1）系统工作：有些重要的工作必须周而复始地执行，如病毒扫猫等

（2）个别用户工作：个别用户可能希望执行某些程序，比如对mysql数据库的备份。

#### 二、基本语法

```
#编辑crontab定时任务
crontab -e 

#终止任务调度
conrtab -r

#列出当前有那些任务调度
crontab -l

```

1.参数说明

> `* * * * * `
>
> **分钟(0-59)  小时(0-23)  天 (1-31) 月(1-12)  星期(0-7)0、7都是星期日**
>
> `*`：代表任何时间。比如第一个“*”就代表一小时中每分钟都执行一次的意思。
>
> `,`：代表不连续的时间。比如“0 8,12,16 * * *命令”，就代表在每天的8点0分，12点0分，16点0分都执行一次命令
>
> `-`：代表连续的时间范围。比如“0 5 * * 1-6命令”，代表在周一到周六的凌晨5点0分执行命令
>
> `*/n`：代表**每隔多久执行一次**。比如“*/10 * * * *命令”，代表每隔10分钟就执行一遍命令

特定时间执行任务案例

![image-20250223221204534](img/image-20250223221204534.png)

#### 三、应用实例

案例1：

```
#设置任务调度文件
/etc/crontab

#设置个人任务调度
crontab -e

#输入任务到调度文件
##每小时的每分钟执行ls -1 /etc/ > /tmp/to.txt命令
*/1 * * * * ls -l /etc/ > /tmp/to.txt
##每隔1分钟，就将当前的日期信息，追加到/tmp/mydate文件中
*/1 * * * * date >> /tmp/mydate
```

案例2：每隔1分钟，将当前日期和日历都追加到/home/mycal文件中

```shell
##先写一个脚本，并且有执行权限
vim /home/aaa.sh
date >> /home/mycal
cal >> /home/mycal
##输入任务到调度文件
crontab -e
*/1 * * * * /home/aaa.sh
##访问mycal文件
```

## at定时任务

#### 一、基本介绍

1.at命令是一次性定时计划任务，at的守护进程atd会以后台模式运行，检查作业队列来运行。

2.默认情况下，atd守护进程每60秒检查作业队列，有作业时，会检查作业运行时间，如果时间与当前

时间匹配，则运行此作业。

3.**at命令是一次性定时计划任务，执行完一个任务后不再执行此任务了。**

4.在使用at命令的时候，一定要保证atd进程的启动，可以使用相关指令来查看。

ps -ef | grep atd      //可以检测atd是否在运行

#### 二、at命令格式

```
at [选项] [时间]

Ctrl+D结束at命令的输入
```

1.at命令选项

![image-20250226144427536](img/image-20250226144427536.png)

2.at指定时间：

（1）接受在当天的hh:mm（小时：分钟）式的时间指定。假如该时间已过去，那么就放在第二天执行。例如：04:00

（2）使用midnight（深夜），noon（中午），teatime（饮茶时间，一般是下午4点）等比较模糊的词语来指定时间。

（3）采用12小时计时制，即在时间后面加上AM（上午）或PM（下午）来说明是上午还是下午。例如：12pm

（4）指定命令执行的具体日期，指定格式为 `month day（月日）` 或 `mm/dd/yy（月/日/年）` 或`dd.mm.yy（日.月.年）`；指定的日期必须跟在指定时间的后面，例如：04:00 2021-03-1

（5）使用相对计时法。指定格式为：now+count time-units，now就是当前时间，time-units是时间单位minutes（分钟）、hours（小时）、days（天）、weeks（星期），count是时间的数量几天、几小时。例如：now + 5 minutes

（6）直接使用today（今天）、tomorrow（明天）来指定完成命令的时间。

#### 三、应用实例

```shell
#案例1：2天后的下午5点执行 /bin/ls /home
at 5pm + 2 days
/bin/ls /home
#案例2：明天17点钟，输出时间到指定文件内比如/root/date100.log
at 5pm tomorrow
date > /root/date100.log
#案例3：2分钟后，输出时间到指定文件内比如/root/date200.log
at now + 2 minutes
date > /root/date200.log
Ctrl+D
Ctrl+D

#查看任务
atq

#删除已经设置的任务
atrm 编号
```

# python开发平台Ubuntu



















