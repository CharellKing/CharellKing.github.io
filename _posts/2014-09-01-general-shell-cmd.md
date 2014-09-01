---
title: 常用shell命令
date: 2014-09-01 20:04 +0800
layout: post
permalink: /blog/2014/09/01 general-shell-cmd.html
comments: true
categories:
  - shell
tags:
  - shell
---
列出在工作中常用的一些shell命令

* 查找cpp和h文件

`find ./src -name "*.cpp" -o -name "*.h"`

* 查找某个文件中包含的字符串行(适用目录下含有子目录)

`find ./src -name "*.cpp" -o -name "*.h" | xargs grep "CreateShm"`

* 查找含有某个字符串的文件（适用目录下包含子目录)

`grep -lr "CreateShm" src/`

* 替换某个目录下所有文件中包含的某个字符串

<pre><code>sed -i "s/DetachShm/Close/g" `grep DetachShm -rl src example`</code></pre>

_(这里是使用grep查找到DetachShm，然后用Close替换，如果替换中的字符含有/，可以用#等其他符号分隔替换的字符与被替换的字符)_
