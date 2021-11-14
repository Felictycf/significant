### /etc 

etc不是什么缩写，是and so on的意思 来源于 法语的 et cetera 翻译成中文就是 等等 的意思. 至于为什么在/etc下面存放配置文件， 按照原始的UNIX的说法([Linux](http://lib.csdn.net/base/linux)文件结构参考UNIX的教学实现MINIX) 这下面放的都是一堆零零碎碎的东西, 就叫etc, 这其实是个历史遗留.

```
这个是来源于拉丁语全称etcetera.
n.等等之人（或物），附加的人（或物）；加s：附加（或额外）的项目；零星杂物。
或者分开的et cetera, adv. 等等，以及其他等等（略作etc.或＆c.）但得注意，人名后不宜用，要用and others。

这个目录在LINUX里很重要哦，里面装的都是些杂七杂八的配置文件

发音很简单，这里打不了音标，到百度字典上看看音标就行了。
```

 

```
这个问题在历史上有几种不同的说法的。
一种说法，是et cetera。这个是老一点的说法，就是说，不管什么数据或文件，只要不属于其它目录的，就放在/etc目录下。
另一种说法，"Editable Text Configuration"：很多人也把/etc目录看成是一个放置我们系统程序的配置文件的地方。所以常看到这样的目录介绍
/etc - Usually contain the configuration files for all the programs that run on your Linux/Unix system.
```

 

 

这个目录一般用来存放程序所需的整个文件系统的配置文件.

 

/etc目录
　　包含很多文件.许多网络配置文件也在/etc 中. 

/etc/rc  or/etc/rc.d  or/etc/rc*.d 
　　启动、或改变运行级时运行的scripts或scripts的目录. 

/etc/passwd 
　　用户[数据库](http://lib.csdn.net/base/mysql)，其中的域给出了用户名、真实姓名、家目录、加密的口令和用户的其他信息. 

/etc/fdprm 
　　软盘参数表.说明不同的软盘格式.用setfdprm 设置.

/etc/fstab 
　　启动时mount -a命令(在/etc/rc 或等效的启动文件中)自动mount的文件系统列表.[linux](http://lib.csdn.net/base/linux)下，也包括用swapon -a启用的swap区的信息.

/etc/group 
　　类似/etc/passwd ，但说明的不是用户而是组. 

/etc/inittab 
　　init 的配置文件. 

/etc/issue 
　　getty在登录提示符前的输出信息.通常包括系统的一段短说明或欢迎信息.内容由系统管理员确定. 

/etc/magic 
　　file 的配置文件.包含不同文件格式的说明，file 基于它猜测文件类型.


/etc/motd 
　　Message Of TheDay，成功登录后自动输出.内容由系统管理员确定.经常用于通告信息，如计划关机时间的警告. 

/etc/mtab 
　　当前安装的文件系统列表.由scripts初始化，并由mount 命令自动更新.需要一个当前安装的文件系统的列表时使用，例如df命令. 

/etc/shadow 
　　在安装了影子口令软件的系统上的影子口令文件.影子口令文件将/etc/passwd 文件中的加密口令移动到/etc/shadow中，而后者只对root可读.这使破译口令更困难. 

/etc/login.defs 
　　login 命令的配置文件. 

/etc/printcap 
　　类似/etc/termcap ，但针对打印机.语法不同. 

/etc/profile , /etc/csh.login ,/etc/csh.cshrc 
　　登录或启动时Bourne或Cshells执行的文件.这允许系统管理员为所有用户建立全局缺省环境. 

/etc/securetty 
　　确认安全终端，即哪个终端允许root登录.一般只列出虚拟控制台，这样就不可能(至少很困难)通过modem或网络闯入系统并得到超级用户特权. 

/etc/shells 
　　列出可信任的shell.chsh 命令允许用户在本文件指定范围内改变登录shell.提供一台机器FTP服务的服务进程ftpd检查用户shell是否列在 /etc/shells 文件中，如果不是将不允许该用户登录. 

/etc/termcap 