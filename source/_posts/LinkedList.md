---
title: LinkedList操作总结
categories: java基础知识
---

LinkedList 是java中常用的数据结构，它既是一个list,也实现了queue和stack的接口。所以我们在平常使用queue和stack这两种数据结构的时候，经常使用就是LinkedList.

类型|方法
:-: | :-: 
stack| pop push getLast
queue| poll offer peek
list | remove add contains

这里列举的是stack，queue,list分别对应的方法，queue的所有方法在List为空的时候返回的是null，然而其它的方法则会抛出异常。

添加| 获取 | 删除 | 
:-: | :-: | :-: |
addFirst| getFirst| removeFirst |
addLast| getLast| removeLast| 

这里面的方法都会在List为空的时候抛出异常，所以需要利用isEmpty进行判断。

添加| 获取 | 删除 
:-: | :-: | :-: 
offerFirst| peekFirst| pollFirst 
offerLast| peekLast| pollLast

这里面的方法会在List为空的时候返回null,所以在实现栈和队列的操作时，使用这3组方法最好。
