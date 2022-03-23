<h1 align="center">handy scripts</h1>

## 介绍
主要记录shell命令的一些常用功能，和替代一些日常工作中流程已经确定的重复性工作。

## 目录
* [常用命令](#常用命令)
    * [文件](#文件)
      * [基本命令](基本命令)
      * [tail](tail)
      * [find](find)
    * [网络](#网络)
    * [磁盘](#磁盘)
    * [内存](#内存)
* [其它](#其它)
    * [管道](#管道)
    * [xargs](#xargs)
    * [alias](#alias)
    * [crontab定时任务](#crontab定时任务)
* [示例](#示例)
* [Shell脚本](#Shell脚本)
    * [变量](#变量)
    * [字符串](#字符串)
    * [流程控制语句](#流程控制语句)
    * [使用案例](#使用案例)
      * [查询代码仓库源码行数](查询代码仓库源码行数)
      * [指标监控](指标监控)
      * [~~番茄时钟~~](番茄时钟)
      * [简单的ci/cd工具]()
      * [用sql分析csv]()
      * [好用插件分享]()
    





## 常用命令

### 文件
#### 基本命令
```shell
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
touch 

# 移动文件；重命名
mv a.txt b.txt

rm -rf .
```

#### tail

#### find
```shell
# 找到当前目录及子目录下所有以.java为后缀的文件
find . -name "*.java"

# 找到当前目录及子目录下两天之内修改的文件
find . -mtime 2d
```

#### grep
```shell
```

#### 全局寻找某个后缀的文件
```shell
# 寻找所有csv后缀的文件
sudo find / -name "*.csv" 2>/dev/null
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
