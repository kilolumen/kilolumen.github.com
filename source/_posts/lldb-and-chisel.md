---
title: lldb and chisel
date: 2016-03-27 18:02:41
categories:
tags:
---

<!--more-->


##lldb

p(print, pri, e - -)  打印
e(expression)      改变调试器和程序中的值
po(e -o - -)           对象description
p 16 默认格式；p/x 16  十六进制；p/t 16  二进制；p/c 16  字符；p/s 16  以空终止的字符串
变量     以$开头定义变量
(lldb) e int $a = 2
(lldb) p $a * 19
c(continue, process continue)
n(next, thread step-over)
s(step, thread step-in)
finish(tread step-out)
frame info             当前行数，源码文件
thread return          在开头伪造返回值
br li (breakpoint list) 断点列表
br dis(breakpoint disable) <breakpointID>       关闭断点
breakpint enable <breakpointID>  打开断点
br del <breakpointID>  删除断点
br s(breakpoint set) -f main.m -l 16


##chisel
pviews             递归打印所有的view
pvc                递归打印viewcontroller
visualise          截图、定位一个view的内容(UIImage,CGImageRef,UIView,CALayer)
fv fvc <name>      搜索当前内存中的view或viewcontroller
show/hide          显示/隐藏 view
mask/unmask        给view覆盖一层遮罩
border/unborder    给view加边框
bmessage           打断点
internals          打印控件类型的内部结构
