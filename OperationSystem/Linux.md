# Linux 常用指令
## 关于关机
1. ```who```用于查看有没有其他用户在线
2. ```sync```加快磁盘文件的读写速度.内存的文件数据不会立即同步到磁盘上,所以在关机前需要使用sync进行同步
3. ```shutdown [-krhc] 时间[信息]```
    - -k:不会关机,会发出警告,通知所有在线用户
    - -r:将系统的服务停止后重新启动
    - -h:将系统的服务停止后立刻关机
    - -c:取消已经在进行的shutdown指令内容

## 包管理工具
|工具名|公司|系统|
|:--:|:--:|:------|
|RPM(Redhat Package Manager)|Redhat|Fedora/Centos|
|DPKG(Debian Package)|Ubuntu|Debian|
## 磁盘
- IDE磁盘: /dev/hd[a-d]
- SATA/SCSI/SAS磁盘:  /dev/sd[a-p]

## 文件
### 文件属性
|```drwxr-xr-x```|```3```|```root```|```root```|```17```|```May 6 00:14```|```.config```|
|:---:|--:|--:|--:|--:|--:|--:|
|文件类型及权限|链接数|文件所有者|所属群组|文件大小|文件最后修改时间|文件名|
### 文件及目录的基本操作
1. ```ls [-aAdfFhilnrRSt]``` **列出文件或目录的信息,目录信息指其中包含的文件**
    - -a: 列出全部的文件
    - -d: 列出目录本身
    - -l: 以长数据列出,包含文件属性与权限数据
2. ```mkdir [-mp] 目录名称``` **创建目录**
    - -m:配置目录权限
    - -p:递归创建目录
3. ```rmdir [-p] 目录名称``` **删除目录,目录内必须为空**
    - -p:递归删除目录
4. ```touch [-acmdt] 文件名``` **更新文件时间或创建新文件**
    - -a:更新atime
    - -c:更新ctime
    - -m:更新mtime
    - -d:后面可以接更新日期而不是用当前日期,亦可用```--date="时间"```
    - -t:后面可以接更新时间而不是用当前时间,格式```[YYYYMMDDhhmm]```
5. ```cp [-adfilprsu] source destination```**复制文件,如果源文件有两个以上.则目的文件一定要是目录**
    - -a:相当于```-dr --preserve=all```
    - -d:若来源文件为链接的文件,则复制链接文件属性而非文件本身
    - -i:若目标文件已存在,覆盖前会先询问
    - -p:连同文件属性一块复制
    - -r:递归持续复制
    - -u:destination比source旧或destination不存在时才复制
    - -preserve=all:处理```-p```权限相关参数外,还加入SELinux属性,links,xattr也复制
6. ```rm [-rif] 文件或目录``` **删除文件**
    - -r:递归删除
7. ```mv``` **移动文件**
    - ```mv [-fiu] souce destination```
    - ``` mv [option] source1 source2 source3 destination```
### 修改权限
每个权限对应的数字权值为r:4  w:2  x:1

例 ```chmod 154 .bashrc```  将.bashrc文件权限改为```-rwxr-xr--```

```chmod [ugoa] [+-=] [rwx] dirname/filename```
- u:拥有着
- g:所属群组
- o:其他人
- a:所有人
- +:加权限
- -:除权限
- =:设定权限
### 链接
```ln [-sf] source_filename dest_filename```
- -s:默认是hard link加s转化为symbolic link
- f:如果目标文件存在,先删除目标文件
#### 实体链接
在目录下创建一个条目,记录文件名与inode编号.inode为源文件inode
#### 符号链接
在目录下创建一个条目,保存源文件的绝对路径,可理解为快捷方式.
### 文件内容
1. ```cat [-AbEnTv] filename```
    - -n: 打印行号,连同空白行也会有行号,-b空白行没有行号
2. ```tac``` cat的反向操作,从最后一行打印.
3. ```more```  和cat不同,可以一页页查看文件内容
4. ```less```  和more类似,多了向前翻页的功能
5. ```head```  取得文件前几行 -n表示显示行数
6. ```tail```  取得文件最后几行 -n表示显示行数
7. ```od``` 以字符或十六进制的形式显示二进制文件
### 文件搜索
1. ```which [-a] command``` **指令搜索**
    - a: 列出所有指令而非只列出第一个
2. ```whereis [-bmsu] dirname/filename``` **文件搜索,速度快,只搜几个特定的目录**
3. ```locate [-ir] keyword```**文件搜索,可用关键字或正则表达式进行搜索**
    - -r:使用正则表达式
    - locate使用了/var/lib/mlocate/这个数据库进行搜索,存储在内存中,每天更新一次,无法使用locate搜索新建的文件,可用updatedb来更新数据库.
4. ```find [basedir] [option]```**文件搜索,可用文件属性和权限进行搜索**
### 压缩
|扩展名|压缩程序|
|:---:|:--------:|
|*.z|compress|
|*.zip|zip|
|*.gz|gzip|
|*.bz2|bzip2|
|*.xz|xz|
|*.tar|tar打包的数据,未压缩|
|*.tar.gz|tar打包文件,经gzip压缩|
|*.tar.bz2|tar打包文件,经bzip2压缩|
|*.tar.xz|tar打包文件,经xz压缩|
### gzip
```gzip [-cdtv#] filename```
- -c: 将压缩的数据输出到屏幕上
- -d: 解压缩
- -t: 检验压缩文件是否出错
- -v: 显示压缩比等信息
- -#: 数字,压缩等级,默认6

可解compress,zip和gzip压缩的文件.

经gzip压缩的文件,源文件就不存在了.

有9个不同的压缩等级可以使用

可以用zcat,zmore,zless来读取压缩文件的内容
### bzip2
```bzip2 [-cdkzv#] filename```
- -k: 保留文件

提供比gzip更高的压缩比

查看命令: bzcat, bzmore, bzless, bzgrep
### xz
```xz [-dtlkc#] filename```

提供比bzip2更佳的压缩比(但压缩比越高,压缩时间也越久)

查看命令: xzcat, xzmore, xzless, xzgrep
### tar
```
tar [-z|-j|-J] [cv] [-f 新建tar文件] filename  #打包压缩
tar [-z|-j|-J] [tv] [-f 已有tar文件] filename  #查看
tar [-z|-j|-J] [xv] [-f 已有tar文件] filename  #解压缩
```
- -z: 使用zip
- -j: 使用bzip2
- -J: 使用xz
- -c: 新建打包文件
- -t: 查看打包文件内有哪些文件
- -x: 解打包或解压缩功能
- -v: 在压缩|解压缩的过程中,显示正在处理的文件名
- -f: filename,要处理的文件
- -C 目录: 在特定目录解压缩

## 变量操作
```declear [-aixr] variable```
- -a: 定义为数组类型
- -i: 定义为整型
- -x: 定义为环境变量
- -r: 定义为readonly类型
## 数据流重定向
||代码|运算符|
|:----:|:--:|:---------:|
|标准输入(stdin)|0|<或<<|
|标准输出(stdout)|1|>或>>|
|标准错误输出(stderr)|2|2>或2>>|

可以将不需要的标准输出及标准错误输出重定向到/dev/null,相当于扔进垃圾箱.

如果需要将标准输出以及标准错误输出同时定向到一个文件,需要将某个输出转化为另一个输出,例 2>&1 表示将标准错误输出转化为标准输出

## 管理通道指令
### 提取指令
1. cut
    - ```cut [-d]``` **分隔符**
    - ```cut [-f]``` **经-d分隔后,用-f n取出第n个区间**
    - ```cut [-c]``` **以字符为单位取出区间**
2. ```sort [-fbMntuk] [file or stdin]``` **用于排序**
    - -f  忽略大小写
    - -b  忽略最前面的空格
    - -M  以月份的名字来排序
    - -n  使用数字排序
    - -r  反向排序
    - -u  相当于unique,重复的内容只出现一次
    - -t  分隔符,默认为tab
    - -k  指定排序的区间
3. ```tr [-ds] SET1``` **用来删除一行中的字符,或者对字符进行替换**
    -- d: 删除行中SET1这个字符串
4. ```col [-xb]``` **将tab字符转为对等的空格字符**
5. ```expand [-t] file``` **将tab字符转换一定数量的空格,默认8个**
    - -t: tab专为空格的数量
6. ```join [-ti12] file1 file2``` **将有相同数据的那行合并在一起**
    - -t: 分隔符,默认为空格
    - -i: 忽略大小写
    - -1: 第1个文件所用的比较字符
    - -2: 第2个文件所用的比较字符
7. ```paste [-0d] file1 file2``` **直接将两行粘贴在一起**
    - -d: 分隔符,默认为tab
8. ```grep [-acinv] [--color=auto] 搜索字符串 filename``` **用正则表达式进行全局查找并打印**
    - -c: 统计个数
    - -i: 忽略大小写
    - -n: 输出行号
    - -v: 反向选择,显示没有搜索到字符串的那行
    - --color=auto: 找到关键字加颜色显示
9. ```printf``` **用格式化输出,不属于管道命令,传参用$()形式**
    - 例: ```printf '%10s %5i %5i %5i %8.2f \n' $(cat printf.txt)
10. awk
每次处理一行.最小处理单位为字段,每个字段命名方式为$n,n为字段号,从1开始,$0表示整行

可以根据字段的某些条件进行匹配

```awk '条件类型 | {动作1} 条件类型2 {动作2} ...' filename```

|awk变量|变量名称|代表意义|
|:--:|:--:|:--:|
||NF|每行拥有字段数总数|
||NR|目前所处理的第几行数据|
||FS|目前的分隔符,默认空格|
## 进程&线程
1. ps **查看某个时间点的进程信息**
    - ```ps -l```看自己的进程
    - ```ps aux```看系统中所有的进程
2. pstree **查看进程树**
    - ```pstree -A```查看所有的进程树
3. top **实时显示进程信息, -d 2 两秒刷新一次**
4. netstat **查看占用端口的进程**
    - ```netstat -anp|grep port``` 查看特定端口的进程

### 进程状态
|进程状态|状态|说明|
|:----:|:----:|:----:|
||R|running or runnable (on run queue)|
||D|uninterruptible sleep (usually I/O)|
||S|interruptible sleep (waiting for an event to complete)|
||Z|zombie (terminated but not reaped by its parent)|
||T| stopped (either by a job control signal or because it is being treated)|

### 孤儿进程
一个父进程退出,而它的一个或多个子进程还在运行,那么这些子进程将成为孤儿进程.孤儿进程将被init进程(进程号为1)所收养,并由init进程对他们完成状态收集工作.由于孤儿进程会被init进程收养,所以孤儿进程不会对系统造成危害.

### 僵尸进程
一个子进程的进程描述符在子进程退出时不会释放,只有当父进程通过wait()或waitpid()获取子进程信息后才会释放.如果子进程退出,而父进程没有调用wait()或waitpdi(),那么子进程的进程描述符仍然保存在系统中,这种进程称为僵尸进程.

系统可用的进程号有限,大量的僵尸进程会导致系统可能因为没有可用的进程号而不能产生新的进程.

消灭将是进程,只要消灭它的父进程,init会收养这些僵尸进程,释放其所占用资源,结束僵尸进程.
