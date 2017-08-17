---
layout: post
title: 开发技能-shell面试整理
date: 2017-07-28
tags: shell 
description: "开发技能-shell面试整理"
---

### 1、常规命令
```
cd 切换目录 
ls 查看当前目录下的内容 
cp 复制 
head、tail 显示文件头、尾内容 
cat 查看文件内容 
more、less 分页展示文件内容 
rm 删除 
tar 解、压缩 
touch 创建文本 
mv 移动或重命名 
find 在文件系统中搜索某文件 如find . -name filename 支持正则 
wc 统计 
grep 查找某个字符串 
pwd 显示当前目录
```

### 2、系统管理命令
```
su 切换用户 
ps 查看进程 
kill 根据pid号杀进程 
who 显示在线账号 
whoami 显示当前操作账号 
hostname 显示主机名 
uname 显示系统信息 
top 动态显示当前资源消耗 
du 查看目录大小 
df 查看磁盘大小 
ping 测试网络是否连通，丢包 
ifocnfig 查看网卡 
man 帮助 
clear 清屏 
alias 对命令重命名 如：alias showmeit=”ps -aux” ，另外解除使用unaliax showmeit 
netstat 显示网络状态 
free 显示内存使用情况 
sar 1 20 以20次显示当前系统资源占用情况，最终显示这20次的平均值 
export 设置环境变量
```

### 3、Linux管道
```
将一个命令的标准输出作为另一个命令的标准输入。也就是把几个命令组合起来使用，后一个命令除以前一个命令的结果。 
例：grep -r “close” /home/* | more 在home目录下所有文件中查找，包括close的文件，并分页输出。
```

### 4、用户及用户组管理
```
/etc/passwd 存储用户账号 
/etc/group 存储组账号 
/etc/shadow 存储用户账号的密码 
/etc/gshadow 存储用户组账号的密码 
useradd 用户名 
userdel 用户名 
groupadd 组名 
groupdel 组名 
passwd root 给root设置密码 
/etc/profile 系统环境变量 
bash_profile 用户环境变量 
.bashrc 用户环境变量 
su user 切换用户，加载配置文件.bashrc 
su - user 切换用户，加载配置文件/etc/profile ，加载bash_profile
```

### 5、更改文件的用户及用户组
```
chown [-R] owner[:group] {File|Directory}
```

### 6、文件权限管理
```
三种基本权限 
R 读 数值表示为4 
W 写 数值表示为2 
X 可执行 数值表示为1
```

### 7、更改权限
```
chmod [u所属用户 g所属组 o其他用户 a所有用户] [+增加权限 -减少权限] [r w x] 目录/文件名
```

### 8、vim使用
vim三种模式：命令模式、插入模式、编辑模式。使用ESC或i或：来切换模式。 
命令模式下： 
```
:q 退出 
:q! 强制退出 
:wq 保存并退出 
:set number 显示行号 
:set nonumber 隐藏行号 
/apache 在文档中查找apache 按n跳到下一个，shift+n上一个 
yyp 复制光标所在行，并粘贴 
h(左移一个字符←)、j(下一行↓)、k(上一行↑)、l(右移一个字符→)
```

### 9、shell script使用
```
# 注释 
$? 前一个命令执行结果，0成功 1失败 
$# 统计传递的参数个数 
$0 在脚本中获取当前脚本名称 
$$ shell本身的processid 
$1 ~ n 获取第1 ~ n个参数 
$@ 所有参数 
sh -n 检测脚本是否有错，语法错或者字符错 
sh -x 调测模式 
tee 将数据输出到标准输出设备(屏幕) 和文件比如：command | tee outfile 
> 覆盖输出 
>> 追加输出 
expr 进行数学运算 如：expr $n + 1 
read 输入，如read a 
basename file 返回不包含路径的文件名比如： basename /bin/tux将返回 tux 
dirname file 返回文件所在路径比如：dirname /bin/tux将返回 /bin 
sort 默认以第一个数据来排序，而且默认是以字符串形式来排序,所以由字母 a 开始升序排序 
uniq 可以去除排序过的文件中的重复行，因此uniq经常和sort合用。也就是说，为了使uniq起作用，所有的重复行必须是相邻的 
awk 用来从文本文件中提取字段。缺省地，字段分割符是空格，可以使用-F指定其他分割符 
sed 一个基本的查找替换程序 
怎么让脚本后台执行 ./test.sh & 
nohup 永久执行 
EOF 通常与”<<”结合使用，”<<EOF”表示后续的输入作为子命令或子shell的输入，直到遇到”EOF”，再次返回到主调shell 
如： 
sqlplus testuser/testpwd <<EOF 
select sysdate from dual; 
EOF 

$SHELL 查看当前的shell语法类型 如：bash ksh csh 
$home 当前用户的家目录，就是创建用户时制定的根目录，如：root的是~ 
$PATH 环境变量，每个目录中间以“：”分割 
./test.sh、test.sh、sh test.sh 3种方式执行脚本的区别，为什么会这样？ 
脚本中的交互： 
send 用于向进程发送字符串 
expect 从进程接收字符串 
spawn 启动新的进程 
interact 允许用户交互 
如： 
#!/usr/bin/expect 
spawn ssh sshuser@192.168.22.194 
expect "*password:" 
send "123\r" 
expect "*#" 
interact 
```

### 10、判断：

通常用” [ ] “来表示条件测试。注意这里的空格很重要。要确保方括号的空格。 
```
[ -f “somefile” ] ：判断是否是一个文件 
[ -x “/bin/ls” ] ：判断/bin/ls是否存在并有可执行权限 
[ -n “$var” ] ：判断var变量是否有值[“$a”=“$b”]：判断a和$b是否相等
```
#### 检查文件类型：
```
-e  文件是否存在   test -e filename
-f  文件是否存在，且为文件 file
-d  文件是否存在，且为目录 directory
-b  文件是否存在，且为block device设备
-c  文件是否存在，且为character device设备
-S  文件是否存在，且为Socket文件
-p  文件是否存在，且为FIFO(pipe)文件
-L  文件是否存在，且为连接文件
```
#### 检查文件权限：
```
-r  文件是否存在，且可读权限
-w  文件是否存在，且可写权限
-x  文件是否存在，且可执行权限
-u  文件是否存在，且具有SUID属性
-g  文件是否存在，且具有SGID属性
-k  文件是否存在，且具有 Sticky bit 属性
-s  文件是否存在，且为 非空白文件
```
#### 两个文件比较：
```
-nt  newer than 判断file1是否比file2新     如：test file1 -nt file2
-ot  older than 判断file1是否比file2旧
-ef  判断是否为同一文件，可用在判断hard link上，判定两个文件是否指向同一个inode 
```
#### 两个整数的判断：
```
-eq  equal 相等，  test n1 -eq n2
-ne  not equal 不相等
-gt  greater than 大于
-lt  less than 小于
-ge  greater than or equal 大于等于
-le  less than or equal 小于等于
```
#### 判定字符串：
```
test -z string  判断字符串是否为0， string为空，返回true，  test -z string
test -n string  判断字符串是否非为0， string 为空， 返回false， -n可省略
test str1 = str2 是否相等
test str1 != str2 是否不相等
```
#### 多重条件：
```
-a 同时成立，and  ， test -r file -a -x file : file同时具有rx权限时，返回true
-o 任意一个成立， or
!  取反
```
#### 流程控制：
if
“if” 表达式 如果条件为真则执行then后面的部分： 
```
if ….; then 
…. 
elif ….; then 
…. 
else 
…. 
fi
```

case
case表达式可以用来匹配一个给定的字符串，而不是数字。 
```
case … in 
…) do something here ;; 
esac
```

select
select结构从技术角度看不能算是循环结构，只是相似而已，它是bash的扩展结构用于交互式菜单显示，功能类似于case结构比case的交互性要好 
```
select color in “red” “blue” “green” “white” “black” 
do 
break 
done 
echo “You have selected $color”
```

loop
while-loop 将运行直到表达式测试为真。will run while the expression that we test for is true. 
关键字”break” 用来跳出循环。而关键字”continue”用来不执行余下的部分而直接跳到下一个循环。 
```
while …; do 
…. 
done

进入循环：满足 
退出循环：不满足
``` 

```
until …; do 
…. 
done

进入循环：不满足 
退出循环：满足 
退出值不为0
``` 

for-loop表达式查看一个字符串列表 (字符串用空格分隔) 然后将其赋给一个变量： 
```
for var in ….; do 
…. 
done
```

#### 抓包：

Linux： 
tcpdump
``` 
-i 监听的网口，比如eth0 
-w 写文件，带文件名 
-s 设置缓存字节数 
-c 抓包次数 
```
windows: 
fiddler wireshark(同时可以看包的报文)