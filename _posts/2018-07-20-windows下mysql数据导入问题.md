---
layout: post
title: Windows下mysql数据导入问题
date: 2018-07-20 
tag: 博客
---

## windows下mysql数据的导入问题

#### 许多初学者在学习将本地数据导入到mysql时使用命令"LOAD DATA INFILE '本地数据文件路径' INTO TABLE 表名 FIELDS TERMINATED BY '::' ENCLOSED BY '"' LINES TERMINATED BY '\n';"但是经常遇到两个坑,分别是：

```
1. "ERROR 1290 (HY000): The MySQL server is running with the –secure-file-priv option so it cannot execute this statement."

2. "errno:13-Permissiondenied"

```

####  我们先来说第一个问题，由于mysql在安装的时候默认指定了一个数据导入和导出目录，所以当我们想从自定义的路径下导入数据是受到限制的，我们可以在mysql中使用命令"show variables like '%secure%';"查看详情，如下图所示：

![](/images/posts/mysql/455.png)

#### 我们可以看到，secure_file_priv所对应的value即为mysql默认指定的目录，那么，我们应该从哪里去修改呢？

#### 首先我们需要到mysql安装的磁盘目录下,找到ProgramData文件夹（注：由于该文件是隐藏文件，所以需要到‘组织’选项卡一栏中设置显示所有隐藏文件）：

![](/images/posts/mysql/234.png)

#### 接下来我们来到my.ini配置文件处，如下图所示：

![](/images/posts/mysql/123.png)

#### 进入到该配置文件中，找到"secure-file-priv"一栏，并将其默认路径设定为‘’,如下图所示：

![](/images/posts/mysql/456.png)

#### 此后，重启mysql ，运行“LOAD DATA INFILE 'C:\Users\Administrator\Desktop\datasets\movielens\ratings.dat' INTO TABLE ratings FIELDS TERMINATED BY '::' ENCLOSED BY '"' LINES TERMINATED BY '\n';”你会发现，新的问题又出现了，那就是第二个坑--“errno:13-Permissiondenied”其实这个问题归根结底还是权限问题，这时我们需要右键点击计算机-->点击管理，出现如图：

![](/images/posts/mysql/000.png)





#### 依次点击服务和应用程序下的服务，选中mysql一栏，点击MySQL下的更多操作，出现mysql的基本属性设置如图：

![](/images/posts/mysql/2324.png)

#### 走到这步就快成功了，点击登录选项卡，将登录身份切换至本地系统账户。返回上一个界面，重启mysql.

#### 最后，从cmd进入到mysql再执行命令"LOAD DATA INFILE '本地数据文件路径' INTO TABLE 表名 FIELDS TERMINATED BY '::' ENCLOSED BY '"' LINES TERMINATED BY '\n';"就可以将本地文件中的数据传入到mysql中了。

#### 哈哈，是不是挺麻烦的呢！