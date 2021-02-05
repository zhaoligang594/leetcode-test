### 工作笔记

> 在工作的过程中，有可能会遇到各种各样的问题。记录一下这么多奇奇怪怪的问题。

#### 1. 如何配置并解决OOM的问题

在运行项目，开发代码的同时，或许项目刚刚开始的时候，是没有任何问题的，但是随着项目的运行，一些奇葩的问题就会暴露出来，在此时，我们应当找出问题并且解决这个问题是关键。那如何进行呢？

实际上，有一些的问题复现是很困难的，所以，在我们运行项目的时候，一定要插眼；一定要留有余地的配置运行的项目，只有这样，在发生意想不到的问题的时候，才会不慌不忙的解决这个问题。

> 任何程序的运行实际上都是使用 java  -jar  这种，采用命令行的形式 

遇到启动的时候，没有任何的问题，但是运行一段时间后，发现了一些奇奇怪怪的问题。这个时候，可以给项目的启动参数上加上一些配置，完美的解决了这问题。当下一次出现什么问题的时候，就可以针对性的定位解决代码里面隐藏的问题。

```shell
# Enables the dumping of the Java heap to a file in the current 
# directory by using the heap profiler (HPROF) when a java.lang.OutOfMemoryError exception is thrown.
# 开启 出现 java.lang.OutOfMemoryError 这个异常的时候 保存上下文的 堆栈 信息
-XX:+HeapDumpOnOutOfMemoryError
# 设置保存的堆栈信息的目录
-XX:HeapDumpPath=path
# 打印 每一个 GC 的详细信息 默认是 不开启的状态
-XX:+PrintGCDetails
# 打印 每一次 垃圾回收的 事件
-verbose:gc
# 设置新生代的大小
-Xmn200M
	#-Xmn256m
	#-Xmn262144k
	#-Xmn268435456
-Xmssize # 设置 堆 的初始的大小 This value must be a multiple of 1024 and greater than 1 MB
	#-Xms6291456
	#-Xms6144k
	#-Xms6m
# 设置内存分配池的最大空间 这个值必须是 1024 的整数倍 而且 必须 大于 2MB 的大小
-Xmxsize
# 设置线程 栈 的大小 单位的匹配
# Append the letter k or K to indicate KB, m or M to indicate MB, g or G to indicate GB
-Xsssize 
```

上面的是基本的经常用的配置，平时启动项目，加上上面的配置，就可以优化项目，精准的定位问题。

```shell
nohup java  -server -Xms2G -Xmx4G -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=path  -jar  XX.jar &
```

#### 2. linux 创建软连接 ln -s 命令

这是linux中一个非常重要命令，请大家一定要熟悉。它的功能是为某一个文件在另外一个位置建立一个同不的链接，这个命令最常用的参数是-s,具体用法是：ln -s 源文件 目标文件：

```shell
不论是硬连结或软链结都不会将原本的档案复制一份，只会占用非常少量的磁碟空间。
　　-f : 链结时先将与 dist 同档名的档案删除
　　-d : 允许系统管理者硬链结自己的目录
　　-i : 在删除与 dist 同档名的档案时先进行询问
　　-n : 在进行软连结时，将 dist 视为一般的档案
　　-s : 进行软链结(symbolic link)
　　-v : 在连结之前显示其档名
　　-b : 将在链结时会被覆写或删除的档案进行备份
　　-S SUFFIX : 将备份的档案都加上 SUFFIX 的字尾
　　-V METHOD : 指定备份的方式
```

>  如果要删除软连接，直接删除刚刚新建的目标文件就好了。

#### 3. 配置 maven 的基本操作

> 平时开发的过程中，我们基本上管理我们的项目一般都是使用的是maven来进行管理，这个时候，maven 的作用就会凸显出来。

为了防止内存出现不足的问题，这时候要修改maven的具体的参数：

```shell
test -r ~/.bash_profile && echo 'export MAVEN_OPTS="$MAVEN_OPTS -Xmx4g"' >>~/.bash_profile
test -r ~/.profile && echo 'export MAVEN_OPTS="$MAVEN_OPTS -Xmx4g"' >>~/.profile
test -r ~/.zshrc && echo 'export MAVEN_OPTS="$MAVEN_OPTS -Xmx4g"' >>~/.zshrc
```

经历过上面配置的参数，这样我们编译我们的软件的时候，就不会出现奇奇怪怪的问题了。

