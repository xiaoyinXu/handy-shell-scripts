<h1 align="center">handy scripts</h1>

## 介绍
主要记录shell命令的一些常用功能，和替代一些日常工作中流程已经确定的重复性工作。

## 目录
* [常用命令](#常用命令)
    * [文件](#文件)
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





## 常用命令
echo cat cd ls mkdir touch find grep sed awk xargs

## 示例
### 文件操作

#### 全局寻找某个后缀的文件
```shell
# 寻找所有csv后缀的文件
sudo find / -name "*.csv" 2>/dev/null
```

#### 查询仓库的代码行数
```shell
# 过滤空行、import依赖行、单行注释
git ls-files | grep -iE "\.java|\.scala" | xargs cat |grep -v "^$|^import|\S+//" | wc -l
```
#### 找到包含某个字符串的文件
```shell
grep -iER "System.out.println" *
```

#### 词频统计
```shell
cat README.md | tr ' ' '\n' | grep -iEv "^$" | grep -iE "^\w+$"  | sort | uniq -c | sort -k1nr
```
