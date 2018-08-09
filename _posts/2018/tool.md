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

```bash
`grep -n 'root' /etc/passwd` 

   搜索包含root字段的行，并输出行号。

`grep -nv 'root' /etc/passwd`  

  搜索没有包含root字段的行，并输出行号。

`grep -n 't[as]st' /etc/passwd`   

  搜索包含t[as]st 字符，其中[]表示包含a或者s。

`grep -n '[^g]oo' /etc/passwd `

  搜索oo前没有g字符的所在行。

[a-z0-9A-Z]表示所有数字与英文字符，^表示行的开始 $ 表示行的末尾，^$表示空行

`grep -n 'ooo*' regular_express.txt`  前两个o一定存在，第三个o可没有，也可有多个。

. * 只能限制0个或多个， 如果要确切的限制字符重复数量，就用{范围} 。范围是数字用,隔开 2,5 表示2~5个, 2表示2个，2, 表示2到更多个

`grep -v '^$' regular_express.txt | grep -v '^#'`  等同 `egrep -v '^$|^#' regular_express.txt`

  搜索去除空行和#开头的行
```

