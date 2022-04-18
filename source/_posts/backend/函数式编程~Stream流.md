---
title: 函数式编程~Stream流
date: 2022-04-18
categories:
    - [学习笔记, Java]
---
# 函数方编程思想
## 概念
面向对象思想需要关注用什么对象完成什么事情，而函数式编程思想类似数学中的函数。主要关注运用函数完成对数据的操作
## 优点
- 代码简洁，快速开发
- 易于并发编程

# Lambda表达式
Java Lambda表达式的一个重要用法是简化某些匿名内部类（`Anonymous Classes`）的写法。实际上Lambda表达式并不仅仅是匿名内部类的语法糖，JVM内部是通过invokedynamic指令来实现Lambda表达式的。
:::info
Lambda表达式并不能取代所有的匿名内部类，只能用来取代**函数接口**（`Functional Interface`）的简写
:::

:::primary
能够使用Lambda的依据是必须有相应的函数接口（函数接口，是指内部只有一个抽象方法的接口）。这一点跟Java是强类型语言吻合，也就是说你并不能在代码的任何地方任性的写Lambda表达式。实际上Lambda的类型就是对应函数接口的类型。Lambda表达式另一个依据是类型推断机制，在上下文信息足够的情况下，编译器可以推断出参数表的类型，而不需要显式指名。
:::
例如：无参函数的简写
如果需要新建一个线程，一种常见的写法是这样：
```java 新建一个线程
new Thread(new Runnable(){// 接口名
	@Override
	public void run(){// 方法名
		System.out.println("Thread run()");
	}
}).start();
```

简化为如下形式：
```java
new Thread(
		() -> System.out.println("Thread run()")// 省略接口名和方法名
).start();
```
# stream流
## 创建流
单列集合：`集合对象.stream()`
```java
List<Author>() authors = getAuthors();
authors.stream();
```
数组：`Arrays.stream(数组)`或者`Stream.of`创建
双列集合：`转化成单列结合后再创建`
```java
Map<String,Integer> map = new HashMap<>();
map.put("夏日重现",12);
map.put("间谍过家家",13);
map.put("辉夜大小姐",14);
Stream<Map.Entry<String,Integer>> stream = map.entrySet().stream();

```