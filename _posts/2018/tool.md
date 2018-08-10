---
title: grep sed awk
date: 2018/08/09
author: litiantian
tag: note
intro: My Way
---
# grep使用

```bash
grep -[acinv] '字符串' filename

  -a 以文本方式搜索

  -c 计算找到符合行的数目

  -n 顺便输出符合条件的行号

  -i 忽略大小写

  -v 反向查找（即找到不符合查找条件的行）

  [^g] 表示除g之外的其他字符
```
grep的一些案例使用，可参考：https://blog.csdn.net/tenfyguo/article/details/6387786

```bash
#grep -v ^$ regular_express.txt | grep -v '^#'  等同 egrep -v '^$|^#' regular_express.txt

   搜索去除空行和#开头的行

#grep -n 'root' /etc/passwd 

   搜索包含root字段的行，并输出行号。

#grep -nv 'root' /etc/passwd  

  搜索没有包含root字段的行，并输出行号。

#grep -n 't[as]st' /etc/passwd   

  搜索包含t[as]st 字符，其中[]表示包含a或者s。

#grep -n '[^g]oo' /etc/passwd

  搜索oo前没有g字符的所在行。

[a-z0-9A-Z]表示所有数字与英文字符，^表示行的开始 $ 表示行的末尾，^$表示空行

#grep -n 'ooo*' regular_express.txt

前两个o一定存在，第三个o可没有，也可有多个, . * 只能限制0个或多个， 如果要确切的限制字符重复数量，就用{范围} 。范围是数字用,隔开 2,5 表示2~5个, 2表示2个，2, 表示2到更多个

```

# sed 的使用

sed是非交互模式的编辑器，默认情况下是，所有的输出行都被打印到屏幕上。sed把每一行都存在缓冲区中，对副本进行编辑，所以对源文件是没影响的。

**案例**

1. 命令p用于显示模式空间的内容。

默认情况下，sed把输入行打印在屏幕上，选项-n用于取消默认的打印操作。当选项-n和命令p同时出现时,sed可打印选定的内容。

```bash
sed '/my/p' datafile
#默认情况下，sed把所有输入行都打印在标准输出上。如果某行匹配模式my，p命令将把该行另外打印一遍。


sed -n '/my/p' datafile
#选项-n取消sed默认的打印，p命令把匹配模式my的行打印一遍。

```

2. 命令d用于删除输入行。

sed先将输入行从文件复制到模式空间里，然后对该行执行sed命令，最后将模式空间里的内容显示在屏幕上。如果发出的是命令d，当前模式空间里的输入行会被删除，不被显示。

```bash
sed '$d' datafile
#删除最后一行，其余的都被显示

sed '/my/d' datafile
#删除包含my的行，其余的都被显示

```

3. s命令,用一个字符串替换另一个

```bash

sed 's/^My/You/g' datafile
#命令末端的g表示在行内进行全局替换，也就是替换以My开头的行，所有的My都被替换为You。

sed -n '1,20s/My$/You/gp' datafile
#取消默认输出，处理1到20行里匹配以My结尾的行，把行内所有的My替换为You，并打印到屏幕上。

sed 's#My#Your#g' datafile
#紧跟在s命令后的字符就是查找串和替换串之间的分隔符。分隔符默认为正斜杠，但可以改变。无论什么字符（换行符、反斜线除外），只要紧跟s命令，就成了新的串分隔符。

```

4. e是编辑命令，用于sed执行多个编辑任务的情况下。在下一行开始编辑前，所有的编辑动作将应用到模式缓冲区中的行上。

```bash

sed -e '1,10d' -e 's/My/Your/g' datafile

#选项-e用于进行多重编辑。第一重编辑删除第1-10行。第二重编辑将出现的所有My替换为Your。因为是逐行进行这两项编辑（即这两个命令都在模式空间的当前行上执行），所以编辑命令的顺序会影响结果。

```

5. r命令是读命令。sed使用该命令将一个文本文件中的内容加到当前文件的特定位置上。

```bash

sed '/My/r introduce.txt' datafile
#如果在文件datafile的某一行匹配到模式My，就在该行后读入文件introduce.txt的内容。如果出现My的行不止一行，则在出现My的各行后都读入introduce.txt文件的内容。

w命令，将所选的行写入文件。

sed -n '/hrwang/w me.txt' datafile

```

6. a\ 命令是追加命令，追加将添加新文本到文件中当前行（即读入模式缓冲区中的行）的后面。所追加的文本行位于sed命令的下方另起一行。如果要追加的内容超过一行，则每一行都必须以反斜线结束，最后一行除外。最后一行将以引号和文件名结束。

```bash

sed '/^hrwang/a\
>hrwang and mjfan are husband\
>and wife' datafile
#如果在datafile文件中发现匹配以hrwang开头的行，则在该行下面追加hrwang and mjfan are husband and wife

```














 

















