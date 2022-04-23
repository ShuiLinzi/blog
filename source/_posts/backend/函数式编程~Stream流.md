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
## 操作流
### `filter()`过滤
通过 `filter() `方法可以从流中筛选出我们想要的元素。
```java
public class FilterStreamDemo {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("周杰伦");
        list.add("王力宏");
        list.add("陶喆");
        list.add("林俊杰");
        Stream<String> stream = list.stream().filter(element -> element.contains("王"));
        stream.forEach(System.out::println);
    }
}
```
filter() 方法接收的是一个 Predicate（Java 8 新增的一个函数式接口，接受一个输入参数返回一个布尔值结果）类型的参数，因此，我们可以直接将一个 Lambda 表达式传递给该方法，比如说 element -> element.contains("王") 就是筛选出带有“王”的字符串。

forEach() 方法接收的是一个 Consumer（Java 8 新增的一个函数式接口，接受一个输入参数并且无返回的操作）类型的参数，类名 :: 方法名是 Java 8 引入的新语法，System.out 返回 PrintStream 类，println 方法你应该知道是打印的。

### `map()` 映射
如果想通过某种操作把一个流中的元素转化成新的流中的元素，可以使用` map() `方法。
```java
public class MapStreamDemo {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("周杰伦");
        list.add("王力宏");
        list.add("陶喆");
        list.add("林俊杰");
        Stream<Integer> stream = list.stream().map(String::length);
        stream.forEach(System.out::println);
    }
}
```
`map()` 方法接收的是一个 Function（Java 8 新增的一个函数式接口，接受一个输入参数 T，返回一个结果 R）类型的参数，此时参数 为 String 类的 length 方法，也就是把 `Stream<String>` 的流转成一个 `Stream<Integer> `的流。
### 其他
1. `distinct` 去重
2. `sorted` 排序
3. `limit`
4. `skip`

## 终止操作
1. `foreach()` 对流中的元素进行遍历操作，我们通过传入的参数去指定对遍历到的元素执行具体的操作
2. `count()` 获取当前流中的个数，返回一个long型结果
3. `max & min` 返回一个optional对象
4. `collection`把流中的元素收集成新的集合

# 方法引用
简单地说，就是一个 Lambda 表达式。在 Java 8 中，我们会使用 Lambda 表达式创建匿名方法，但是有时候，我们的 Lambda 表达式可能仅仅调用一个已存在的方法，而不做任何其它事，对于这种情况，通过一个方法名字来引用这个已存在的方法会更加清晰，Java 8 的方法引用允许我们这样做。方法引用是一个更加紧凑，易读的 Lambda 表达式，注意方法引用是一个 Lambda 表达式，其中方法引用的操作符是双冒号 "::"。

## 什么场景适合使用方法引用
当一个 Lambda 表达式调用了一个已存在的方法，且没有别的代码

**什么场景不适合使用方法引用**
需要往引用的方法传参数的时候不适合：

