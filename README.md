<h1 align="center">handy shell scripts</h1>

## 介绍
Shell是一种操作系统预先装好的应用程序，人们常说的小黑框、命令行、终端、Terminal等都可以简单粗暴的理解为Shell。
传统的Web应用或桌面应用使用可视化界面作为用户接口，而Shell则让用户通过输入命令的方式去和操作系统交互。
Shell内置了许多系统命令，使用户可以高效、方便地去访问和分析计算机的文件、磁盘、内存、网络等信息, 同时很多第三方命令行软件也可以通过Shell去调用。
Shell也是一种解释型的程序设计语言，用户可以通过编写Shell脚本来完成从简单到复杂的工作。目前Shell主要应用领域之一是自动化运维，
比如通过Shell脚本的方式去自动化软件的构建、部署，比如大数据领域里的很多分布式软件（如Flink、Kafka、Elasticsearch）通常都是通过"大篇幅"脚本部署。 
简单来说，很多重复且流程固定的工作可以尝试用Shell脚本来解决，只需要编写一次便能反复利用。

对于我们很多人而言，Shell也可以在日常工作中发挥很大的功能。
本文主要讲述Shell的一些命令的基本使用，并谈谈Shell可以做哪些有趣/有用的事情。

## 目录
* [Mac终端美化](#Mac终端美化)
  * [iTerm2](#iTerm2)
  * [oh-my-zsh+agnoster](#oh-my-zsh+agnoster)
* [常用命令](#常用命令) 
    * [最关键的命令-man](#最关键的命令-man)   
    * [文件/文本分析相关命令](#文件/文本分析相关命令)
      * [基本命令](#基本命令)
      * [Linux四剑客](#Linux四剑客)
        * [find](#find)
        * [sed](#sed)
        * [grep](#grep)
        * [awk](#awk)
      * [其它命令]()
        * [tail]()
    * [进程]()
    * [网络](#网络)
    * [磁盘](#磁盘)
    * [内存](#内存)
* [命令组合](#命令组合)
    * [管道](#管道)
    * [xargs](#xargs)
    * [alias](#alias)
* [crontab定时任务](#crontab定时任务)
* [Shell可以做哪些有趣的事]()
* [日常案例]()




## Mac终端美化
大多数操作系统Shell默认的样式就是一个小黑框，对于一个长期使用的软件，外观和易用性是影响用户体验的最主要因素。这里提供一个Mac下比较流行的set-up: iTerm2 + oh-my-zsh + agnoster theme, 最终配置完后大致长下面这样：
![](https://cookie.cdn.bcebos.com/agnoster.png)
### iTerm2
iTerm2是mac终端的一个替代品，它的功能更多一些，比如我最常用到的两个功能: 终端下打开文件和分屏。当然最重要的功能还是它有solarized dark这个配色方案，比较好看。
[安装地址](https://iterm2.com/)
### oh-my-zsh+agnoster
![](https://cookie.cdn.bcebos.com/oh-my-zsh.png)
oh-my-zsh是一个github开源项目，它负责管理zsh里相关的一些配置, 简单来说就是维护~/.zshrc配置文件, 它内置了很多插件和主题，比如我们要用到的agnoster。
安装完oh-my-zsh后，我们只需要将~/.zshrc里的主题配置改成agnoster即可。 [仓库地址](https://github.com/agnoster/agnoster-zsh-theme)
为了能够正常显示agnoster主题，还需要安装Powerline Fonts，oh-my-zsh和agnoster的具体安装方式可参考[oh-my-zsh仓库地址](), 唯一要注意的是安装完Powerline Fonts字体后，
需要在iTerm2的偏好设置里修改字体。


## 常用命令
我日常工作中用到最多的命令主要还是和文件/文本分析相关的，下面重点讲解这方面的命令。

### 最关键的命令-man
man即manual(说明书)的缩写，如果想要详细了解一个命令的用法，man是最直接有效的方式。
![](https://cookie.cdn.bcebos.com/man-find.gif)

当通过man找不到相应命令的文档时，也可以尝试命令后面加 -h 或者 --help。
![](https://cookie.cdn.bcebos.com/python-help.gif)

### 文件/文本分析相关命令
#### 基本命令
```shell
# cd进入目录
cd dir
# 退回上一级目录
cd ..

# 打印文件
echo 123
# 打印嵌入变量的文本
echo "My name is ${name}, and I'm ${age} years old"

# 打印单个文件内容
cat 1.txt 
# 打印多个文件内容
cat 1.txt 2.txt 3.txt

# -a: 打印以"."开头的隐藏文件。 -l: 打印文件详细信息。 -t: 按照最新修改时间排序
ls -alt

# 创建目录
mkdir dir

# 创建文件
touch 1.txt

# 移动文件；重命名
mv a.txt b.txt

# 递归删除目录。   可以在Docker容器或虚拟机里尝试:"rm -rf /"
rm -rf dir
```
#### Linux四剑客
find，grep, sed, awk俗称Linux四剑客，其中find用来检索文件，而grep、sed、awk则是以行为单位去处理文本。它们单个命令本身就能完成很多事情，借助管道等方式组合使用则能发挥更强大的作用。
##### find
```shell
# 在当前目录下找到所有以.java为后缀的文件
find . -name "*.java"

# 找到两天之内修改过的.md后缀文件
sudo find / -name "*.md" -mtime -2d  2>/dev/null

#... TODO
```


##### grep
```shell
# 在当前目录递归寻找包含关键字"System.out"的文件
grep -nR "System.out" *
```
![](https://cookie.cdn.bcebos.com/grep-system-out.gif)

```shell
# 得到/etc/hosts中的所有ipv4地址
grep -ioE "(25[0-5]|2[0-4]\d|[0-1]?\d?\d)(\.(25[0-5]|2[0-4]\d|[0-1]?\d?\d)){3}" /etc/hosts
```
![](https://cookie.cdn.bcebos.com/grep-ipv4.gif)

##### sed
```shell
# 将1.sh所有行的前面加上#字符
sed s/^/#/g 1.sh

# 将test.txt中所有的a替换为A
sed -i 's/a/A/g' test.txt

# 将test.txt中第1到第5行删除
sed '1,5d' test.txt

# 删除test.txt的空行
sed '/^$/d' test.txt

# 打印test.txt中第4到第7行
sed -n '4,7p' test.txt
```

##### awk
```shell
# 打印csv文件的第2，3列
awk -F ',' '{print $2,$3}' 1.csv
```
![](https://cookie.cdn.bcebos.com/awk-csv.gif)

当然了，awk的定位其实是一种程序语言，它有很多强大的用法，这里就不展开讲了。


#### 其它命令

```

#### 查询仓库的代码行数
```shell
# 过滤空行、import依赖行、单行注释
git ls-files | grep -iE "\.java|\.scala" | xargs cat | grep -iEv "^$|^import|\s+//" | wc -l
```
#### 找到包含某个字符串的文件
```shell
grep -iER "System.out.println" *
```

#### 词频统计
```shell
cat README.md | tr ' ' '\n' | grep -iEv "^$" | grep -iE "^\w+$"  | sort | uniq -c | sort -k1nr
```