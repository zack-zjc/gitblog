---
title: "JVM垃圾回收"
catalog: true
date: 2020-05-20 10:51:24
subtitle: "垃圾回收基础"
header-img: "img/jieyi.jpg"
comments: true
tags:
- Java
catagories:
- Java
---

## 对象的引用:
垃圾回收就不得不考虑，怎么判断一个对象是否应该被清除

### 引用计数法
当有一个引用指向当前对象时，当前对象的引用计数加一，当对象不存在引用时表示该对象可能被销毁。但该方法无法处理循环引用问题，当两个对象相互引用时虚拟机无法判断该对象是否该销毁。该计数法使用简单高效，object-c当前使用的就是该种计数，把清除对象引用问题抛给开发者

### 根可达搜索算法
该算法通过判断GC-ROOT向下遍历对象是否为节点判断对象是否该销毁。
可作为GC-ROOT的根有：
1.方法区中保存的类的静态变量和静态属性，类的常量等
2.java堆中保存的字符串常量等
3.本地方法栈中native引用的java对象
4.synchronized锁住的对象
5.虚拟机栈中保存的变量等
6.等等

---

## 垃圾回收算法:

### 标记-清除算法
在内存中jvm调用一次GC就会触发遍历循环标记当前内存中未被引用的对象，然后清除当前内存中未被引用的对象，该算法简单高效，但容易造成较多的内存碎片，导致大对象容易在申请内存时出现oom

### 标记-复制算法
在jvm内存中把内存区域分为平等的两个区域，在一个区域中进行对象申请等操作，当jvm触发GC时把标记存活的对象一次性拷贝到另一半内存中，然后清除该片内存，实现gc。在虚拟机young区的回收算法就是使用该种算法，该算法对于存活对象较少的区域进行回收比较高效。

### 标记-整理算法
在jvm中标记清除完剩余的对象会被整理到内存最开始处。该算法适合被回收比较少的内存使用，虚拟机中老年区的回收算法就是使用该种算法。

## 垃圾回收器：
种类较多：serial,parnew，cms等
