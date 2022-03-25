<h1 align="center">handy shell</h1>

## 介绍
Shell是一种操作系统预先装好的应用程序，人们常说的小黑框、命令行、终端、Terminal等、Windows里的cmd、Powershell都可以简单粗暴的理解为Shell。

传统的Web应用或桌面应用使用可视化界面作为用户接口，而Shell则让用户通过输入命令的方式去和操作系统交互。

Shell内置了许多系统命令，使用户可以高效、方便地去访问和分析计算机的文件、磁盘、内存、网络等信息, 同时很多第三方命令行软件也可以通过Shell去调用。

Shell也是一种解释型的程序设计语言，用户可以通过编写Shell脚本来完成从简单到复杂的工作。目前Shell主要应用领域之一是自动化运维， 通过Shell脚本可完成软件的自动化构建、部署。
比如大数据领域里的很多分布式软件（如Flink、Kafka、Elasticsearch等）通常都是通过"大篇幅"脚本部署。 

简单来说，很多重复且流程固定的工作可以尝试用Shell脚本来解决，只需要编写一次便能反复利用。
对于我们很多人而言，Shell也可以在日常工作中发挥很大的作用。
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
    * [其它命令](#其它命令)
* [命令组合](#命令组合)
    * [管道](#管道)
    * [xargs](#xargs)
    * [alias](#alias)
* [crontab定时任务](#crontab定时任务)
* [有趣的用例](#有趣的用例)
  * [cal查询日历](#cal查询日历)
  * [fortune随机名言](#fortune随机打印名言)
  * [ici查询单词](#ici查询单词)
  * [pandoc文档转换](#pandoc文档转换)
  * [autojump快速跳转目录](#autojump)
  * [彩蛋](#彩蛋)
    * [asciiquarium水族馆](#asciiquarium水族馆)
  * [小游戏](#小游戏)
    * [nsnake贪吃蛇](#nsnake贪吃蛇)
    * [bastet俄罗斯方块](#bastet俄罗斯方块)
* [日常用例](#日常用例)
  * [curl爬取数据](#curl爬取数据)
    * [chrome技巧](#chrome技巧)
    * [自动获取Cookie](#自动获取Cookie)
  * [预警监控](#预警监控)
  * [简易自动化部署](#简易自动化部署)
    * [编写Shell脚本](#编写Shell脚本)
    * [搭建一个轻量的Http服务](#搭建一个轻量的Http服务)
    * [增加Github Webhooks配置](#增加Github Webhooks配置)
* [总结](#总结)




## Mac终端美化
大多数操作系统Shell默认的样式就是一个小黑框，对于一个长期使用的软件，外观和易用性是影响用户体验的最主要因素。这里提供一个Mac下比较流行的set-up: iTerm2 + oh-my-zsh + agnoster theme, 最终配置完后大致长下面这样：
![](https://cookie.cdn.bcebos.com/agnoster.png)
### iTerm2
iTerm2是mac终端的一个替代品，它的功能更多一些，比如我最常用到的两个功能: 终端下打开文件和分屏。当然最重要的功能还是它有solarized dark这个配色方案，比较好看。
[安装地址](https://iterm2.com/)
![](https://cookie.cdn.bcebos.com/iTerm2.gif)
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

### 其它命令
 文件/文本相关的还有很多命令，比如`more`，`less`，`head`, `tail`等。与进程相关的命令：`ps`、`top`，与网络相关的命令: `netstat`、`route`、`traceroute`，
与磁盘相关的命令: `du`, `df`。 这些命令自己平时用的比较少，这里就不做讲解了。

## 命令组合
### 管道
管道即piplines, 是由很多命令和'|'字符组成的命令组合，'|'左边命令的标准输出会作为'|'右边命令的标准输入。

管道是我们组合命令的最常见的方式
```shell
# 查询文件的前三行
cat 1.txt | head -3

# 词频统计
cat README.md | tr ' ' '\n' | grep -iEv "^$" | grep -iE "^\w+$"  | sort | uniq -c | sort -k1nr

# 查询仓库的代码行数: 过滤空行、import依赖行、单行注释
git ls-files | grep -iE "\.java|\.scala" | xargs cat | grep -iEv "^$|^import|\s+//" | wc -l


```

### xargs
xargs也是组合命令的一种方式，它常搭配管道一起使用。上面已经提到，管道是上一个命令的标准输出作为下一个命令的标准输入，但是很多命令是不支持标准输入的，xargs就是用来解决这个问题的。
那么什么是标准输出、标准输入呢？直接打印在控制台就是标准输出，从控制台读取就是标准输入。Shell里面的echo, Python里面的print, java里面的System.out.println都是标准输出。
有一些Shell命令支持标准输入，比如cat、grep、awk、head、more、less等，直接的体现就是在Shell里面单独输入这些命令，Shell不会打印东西，而是等待用户的输入。
而有一些Shell命令不支持标准输入, 比如mv、rm。除了通过标准输入去使用命令，也可以通过`命令行参数`的方式去使用Shell命令，比如`cat 1.txt 2.txt 3.txt`。

同一个命令通过`命令行参数`和`标准输入`两种方式去使用，表现的功能一般不一样。

如`cat 1.txt`代表的是打印1.txt这个文件的文本，而通过标准输入，则是输入什么打印什么。
![](https://cookie.cdn.bcebos.com/cat-p.gif)

如`grep 1.txt`代表的是过滤1.txt文件的文本，而通过标准输入，则是输入一行过滤一行。
![](https://cookie.cdn.bcebos.com/grep-p.gif)

而回到xargs，xargs经常结合管道，将管道左边命令的标准输出，作为管道右边命令的`命令行参数`
```shell
# 打印当前目录下所有txt文件的文本信息
ls | grep '\.txt' | xargs cat

# 删除当前目录及子目录下所有txt文件。 由于rm不支持标准输入，如果没有xargs，命令将会失效。
find . -name "*.txt" | xargs rm -rf
```

### alias
alias即别名，类似于C/C++里的宏(Macro)。
```shell
alias python=python3
alias grep="grep --color=auto"
alias ga="git add ."

# docker删除所有容器
alias drm="docker rm -f `docker ps -aq`"

# alias满足不了的话，一般考虑用function
function cs () {
    cd $1
    ls
}
```

## crontab定时任务
crontab是一般类unix操作系统内置的单机定时任务程序，它帮助用户在指定的时间内执行相应的任务。
cron表达式是用来描述时间的表达式，由"5个元素"构成, 分别为minute(分钟)、hour(小时)、day of month(某月的第几天)、month、day of week（每周几）
对于一些其它的分布式调度框架，如quartz，它的cron表达式支持7个字符，比crontab的多一个second(秒), year(年), 其描述时间的功能更强大。
虽然crontab不支持秒粒度的调度任务，但是也足够应付很多工作了。下面是一些常见的表达式，
```shell
# 每分钟
* * * * *

# 每10分钟
0/10 * * * *

# 周三中午十二点
0 12 * * WED
```
对于一些支持"6个及以上元素"的cron表达式，有各种个样的写法，可以参考一些网上的cron表达式生成工具。值得注意的是，当描述整点的时候，`千万要记得`前面的元素用0而不是*,
比如用户要定义一个每天十二点整点发邮件的任务, 如果cron表达式写成了`* 12 * * *`那么代表12点的每一分钟(12:00、12:01、12:02...)都会去发送邮件，对于支持秒的调度框架，那么更是会形成邮件轰炸。


## 有趣的用例
### cal查询日历
cal用来打印日历，一般Shell都内置了这条命令。
```shell
# 当前月
cal

# 当前年
cal -y

# 某年
cal -y 2023

# 某月
cal -m 5
```
![](https://cookie.cdn.bcebos.com/cal.gif)

### fortune随机打印名言


### ici查询单词
一个使用金山磁盘api + python编写的快速查单词的工具，非常好用。安装方式`pip install ici`, [仓库地址](https://github.com/Flowerowl/ici)
如果有兴趣还可以扩充ici的功能，比如增加单词收藏、单词本等功能。
![](https://cookie.cdn.bcebos.com/ici.gif)

### pandoc文档转换
用于常用文档格式之间的转换。[仓库地址](https://github.com/jgm/pandoc)

```shell
# 将markdown文件转换为ppt
pandoc README.md -o README.html -s -t revealjs -V theme="white"
```
### autojump快速跳转目录
对已经访问过的目录，可以在Shell容错输入目录名来快速访问。[仓库地址](https://github.com/wting/autojump)
![](https://cookie.cdn.bcebos.com/autojump.gif)


### 彩蛋
有很多好看的彩蛋
##### asciiquarium水族馆
还挺治愈的 `brew install asciiquarium`
![](https://cookie.cdn.bcebos.com/asciiquarium.gif)

### 小游戏
[更多游戏](https://zhuanlan.zhihu.com/p/51904179)
#### nsnake贪吃蛇
`brew install nsnake` 
![](https://cookie.cdn.bcebos.com/nsnake.gif)
#### bastet俄罗斯方块
`brew install bastet`
![](https://cookie.cdn.bcebos.com/bastet.gif)



## 日常用例
### curl爬取数据
curl是一个操作系统内置的命令，常用来发送http请求。对于日常浏览器里访问的数据，如果我们想做整合分析，就可以考虑用curl来"试水"。
#### chrome技巧
chrome的控制台里，网络-xhr里可以追踪浏览器发送到后端的api http请求，右键请求: copy as curl即可复制一段能直接在Shell里执行的curl脚本。
![](https://cookie.cdn.bcebos.com/chrome.gif)
如果你想通过一些编程语言来更灵活地去分析数据，可以去[curl转换代码工具](https://curlconverter.com/)里将curl转换成代码，非常好用。
![](https://cookie.cdn.bcebos.com/curl-converter.gif)

#### 自动获取Cookie
很多数据获取是需要身份校验的，而Cookie则是身份校验里最常用的手段。当用户登陆完成某个网站后，服务器向用户浏览器返回set-cookie的response body, 
浏览器将Cookie储存起来，当下次访问该网站时，自动携带对应的Cookie信息，那么后端服务器就知道访问用户是谁了，一般也就通过身份校验了。
浏览器将Cookie持久化到了磁盘的某个文件里，这也就意味着我们一定能从本地的某个文件里获取到Cookie信息，并且我们可以将这个获取的方式自动化。
。Cookie由多个key-value键值对组成，但是往往用于身份校验的键值对就这么一两个，以bilibili.com为例， SESSDATA即为bilibili身份检验唯一需要的key。
下面提供一种基于python3自动获取Cookie的方案
```shell
# 安装browsercookie
pip install browsercookie
```

下面演示获取bilili.com的Cookie
```python
# ~/get_bilibili_cookie.py
import browsercookie
import re

# cookie名称
cookie_key="SESSDATA"
# 域名
domain="bilibili.com"

s = str(browsercookie.chrome())
regexp = "<Cookie {}=([^>]*?) for \.{}/>".format(cookie_key, domain)
sso_cookie_value = re.findall(regexp, s)[0]
print(sso_cookie_value)
```

将其更通用化
```shell
# ~/get_cookie_value.py
import browsercookie
import sys
import re

if __name__ == "__main__":
    if (len(sys.argv) != 3):
	    print("error")
    else:
        domain = sys.argv[1]
        cookie_key= sys.argv[2]
        s = str(browsercookie.chrome())
        regexp = "<Cookie {}=([^>]*?) for \.{}/>".format(cookie_key, domain)
        sso_cookie_value = re.findall(regexp, s)[0]
        msg = sso_cookie_value if sso_cookie_value != '' and sso_cookie_value != None else "error"
        print(msg)


# ~/.bash_rc or ~/.zshrc
get_cookie_value() {
	/usr/bin/python3 ~/get_cookie_value.py $*
}

```

### 预警监控
  公司对很多中间件都配备了强大的预警功能，但监控的对象往往是CPU、内存、心跳、磁盘等骗硬件信息，却很难对业务数据做预警。
比如现在想对Elasticsearch里某个索引的文档数量进行预警（当文档超过一定量时，ES查询性能会下降很多），那么我们就可以写一个简单的Shell脚本，
通过curl获取数据，对于一些需要身份验证才能访问的接口，可以利用上述方法自动获取Cookie, 最后再结合crontab工具，就完成了一个自动监控的脚本了。
```shell
# ~/alert.sh
count="curl https://abcd.com/elasticsearch/kibana/_count?index=test_index" -H "cookie: sso.cookie=`get_cookie_value abcd.com sso.cookie`"
if [ $count -ge 3000000000 ]; then
  alarm #报警
fi

# crontab
* * * * * ~/alert.sh
```

### 简易自动化部署
  软件的构建和发布是一件非常重复、耗时的事情，公司一般会使用强大的ci/cd工具来完成构建、测试、提测、测试验证、预发部署、预发验证、上线等完备的流水线。
市面上有很多常用的ci/cd工具，比如jenkins，它们比较重，使用成本较高，而对于自己的个人项目，我们往往只需要满足一个简单的功能：
当代码push到仓库后，服务器能够自动拉取代码 + 构建 + 发布，下面就此提供一种比较轻量的解决方案:
#### 编写Shell脚本
```shell
# deploy.sh
cd ~/Projects/my-app
git pull
mvn clean package
java -jar target/XXXX.jar
```

#### 搭建一个轻量的Http服务
使用python的flask框架
```shell
# ~/deploy.sh
import os
import sys
from flask import Flask

sys.stdout = open("info.log", 'a+')

app = Flask(__name__)

@app.route('/deploy', methods=['POST'])
def deploy():
    code = os.system("~/Shell/deploy.sh")
    return 'success' if code == 0 else 'fail'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)
```
这样通过POST访问 https://XXXX.com:8080/deploy时，就可以触发自动化部署脚本

#### 增加Github Webhooks配置
在Github对应的代码仓库里Settings - Webhooks增加上述的POST url, 触发事件为push。

至此，就完成了一个简单的自动化部署工具。

## 总结
  Shell非常灵活，大到自动化运维，小到帮助个人完成日常的琐事，还有更多有趣/有用的用法等着去挖掘～