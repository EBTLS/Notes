# C语言编译与链接过程简介

[toc]

> 来自：https://blog.csdn.net/liuchunjie11/article/details/80252811
>


# 1. 编译过程

编译过程可分为4步

1. 预处理器
2. 编译器
3. 汇编器
4. 连接器


## 1.1. 预处理器

* 处理注释
* 展开宏
* 处理条件编译指令
* 处理``#include``， 展开文件包含
* 处理``#pragma``指令


## 1.2. 编译器

1. 对预处理文件进行语法分析、词法分析、语义分析

   * 语法分析：分析表达式是否遵循语法规则
   * 词法分析：分析关键字，标识符，立即数是否合法
   * 语义分析：在语法分析基础上进一步分析表达式是否合法
2. 分析结束后进行代码优化生成相应的**汇编代码文件**


## 1.3. 汇编器

1. 汇编器将汇编代码转变为机器可以执行的指令，也就是机器指令
2. 每条汇编指令几乎都对应一条机器指令


# 2. 链接过程介绍

链接是将目标文件最终生成可执行文件

## 2.1. 静态链接

目标文件直接进入可执行文件


## 2.2. 动态链接

在程序启动后才动态加载目标文件