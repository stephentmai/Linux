# 查看是否安装java
```shell
JAVA -version
``` 
# 安装java
```shell
wget https://download.oracle.com/java/21/latest/jdk-21_linux-x64_bin.tar.gz
```
# 查看当前目录
```shell
ls
jdk-21_linux-x64_bin.tar.gz
```
# 解压缩tar
```shell
tar -zxvf jdk-21_linux-x64_bin.tar.gz
```
## tar命令
<span style="background:black;color:white">语法</span>

tar (选项)（参数）

<span style="background:black;color:white">选项</span>

-A或--catenate：新增文件到以存在的备份文件；<br>
-B：设置区块大小；<br>
-c或--create：建立新的备份文件；<br>
-C <目录>：这个选项用在解压缩，若要在特定目录解压缩，可以使用这个选项。<br>
-d：记录文件的差别；<br>
-x或--extract或--get：从备份文件中还原文件；<br>
-t或--list：列出备份文件的内容；<br>
-z或--gzip或--ungzip：通过gzip指令处理备份文件；<br>
-Z或--compress或--uncompress：通过compress指令处理备份文件；<br>
-f<备份文件>或--file=<备份文件>：指定备份文件；<br>
-v或--verbose：显示指令执行过程；<br>
-r：添加文件到已经压缩的文件；<br>
-u：添加改变了和现有的文件到已经存在的压缩文件；<br>
-j：支持bzip2解压文件；<br>
-v：显示操作过程；<br>
-l：文件系统边界设置；<br>
-k：保留原有文件不覆盖；<br>
-m：保留文件不被覆盖；<br>
-w：确认压缩文件的正确性；<br>
-p或--same-permissions：用原来的文件权限还原文件；<br>
-P或--absolute-names：文件名使用绝对名称，不移除文件名称前的“/”号；<br>
-N <日期格式> 或 --newer=<日期时间>：只将较指定日期更新的文件保存到备份文件里；<br>
--exclude=<范本样式>：排除符合范本样式的文件。

<span style="background:black;color:white">参数</span>

文件或目录：指定要打包的文件或目录列表

<span style="background:black;color:white">实例</span>

**将文件全部打包成tar包：**

```shell
tar -cvf log.tar log2012.log    仅打包，不压缩！
tar -zcvf log.tar.gz log2012.log   打包后，以 gzip 压缩
tar -jcvf log.tar.bz2 log2012.log  打包后，以 bzip2 压缩
```
在选项<kbd>f</kbd>之后的文件档名是自己取的，我们习惯上都用.tar来作为辨识。如果加<kbd>z</kbd>选项，则以.tar.gz或.tgz来代表gzip压缩过的tar包；如果加<kbd>j</kbd>选项，则以.tar.bz2来作为tar包名。

**查阅上述tar包内有哪些文件：**

```shell
tar -ztvf log.tar.gz
```
由于我们使用gzip压缩的log.tar.gz，所以要查阅log.tar.gz包内的文件时，就得要加上<kbd>z</kbd>这个选项了。

**将tar包解压缩：**

```shell
tar -zxvf /opt/soft/test/log.tar.gz
```
在预设的情况下，我们可以将压缩档在任何地方解开的

**只将tar内的部分文件解压出来：**

```shell
tar -zxvf /opt/soft/test/log30.tar.gz log2013.log
```
我可以透过<kbd>tar -ztvf</kbd>来查阅tar包内的文件名称，如果单只要一个文件，就可以透过这个方式来解压部分文件！

**文件备份下来，并且保存其权限：**

```shell
tar -zcvpf log31.tar.gz log2014.log log2015.log log2016.log
```
这个<kbd>-p</kbd>的属性是很重要的，尤其是当您要保留原本文件的属性时。

**在文件夹当中，比某个日期新的文件才备份：**

```shell
tar -N "2012/11/13" -zcvf log17.tar.gz test
```

**备份文件夹内容是排除部分文件：**

```shell
tar --exclude scf/service -zcvf scf.tar.gz scf/*
```

**其实最简单的使用 tar 就只要记忆底下的方式即可：**

压　缩：tar -jcv -f filename.tar.bz2 要被压缩的文件或目录名称<br>
查　询：tar -jtv -f filename.tar.bz2<br>
解压缩：tar -jxv -f filename.tar.bz2 -C 欲解压缩的目录<br>