---
title: Java基础知识扫盲
date: 2022-01-01
categories: 
- 后端
tags:
- Java
- 读书笔记
- 查漏补缺
---
## 面向对象程序设计
1. 创建一个数字数组时，所有元素都初始化为0。boolean数组的元素会初始化为false。对象数组的元素则初始化为一个特殊值null，表示这些元素(还)未存放任何元素。
<!-- more -->
2. 增强for循环 for(variavle : collection),其中collection这一集合表达式必须是一个数组或者是一个实现了Iterable接口的类对象

3. Math.random方法返回一个0到1之间(包含0、不包含1)的随机浮点数。用n乘以这个浮点数，就可以得到从0到n-1之间的一个随机数。

4. 注意不要编写返回可变对象引用的访问器方法。比如下面的Employee类就违反了这个设计原则，因为其中的getHireDay方法返回了一个Date类对象:

```java
    class Employee{
        private Date hireDay;
        ···
        public Date getHireDay(){
            return hireDay;
        }
    }
```

LocalDate类没有更改器方法，与之不同，Date类有一个更改器方法setTime。
Date对象是可变的，这一点就破坏了封装性！请看下面这段代码：

```java
    Employee harry = ...;
    Date d = harry.getHireDay();
    double tenYearsInMilliSeconds = 10*365.25*24*60*60*1000;
    d.setTime(d.getTime()-(long) tenYearsIMilliSeconds);
```

出错的原因很微妙。d和harry.hireDay 引用同一个对象。对d调用更改器方法就可以自动地改变这个Employee对象的私有状态！

如果需要返回一个可变对象的引用，首先应该对它进行克隆(clone)。对象克隆是指放在另一个新位置上的对象副本。

5. final实例字段
- 可以将实例字段定义为final。这样的字段必须在构造对象时初始化。也就是说，必须确保在每一个构造器执行之后，这个字段的值已经设置，并且以后不能再修改这个字段。例
如，可以将 Employee类中的name字段声明为 final,因为在对象构造之后，这个值不会再改变，即没有setname方法。

- final修饰符对于类型为基本类型或者不可变类的字段尤其有用。（如果类中的所有方法都不会改变其对象，这样的类就是不可变的类。例如， String类就是不可变的。）
## 静态字段和静态方法
### 静态字段

如果将一个字段定义为static，每个类只有一个这样的字段。而对于非静态的实例字段，每个对象都有自己的一个副本。例如，假设需要给每一个员工赋予唯一的标识码。这里给Employee类添加一个实例字段id和一个静态字段nextId:

```java
    class Employee{
        private static int nextId = 1;
        private int id;
    }
```

现在，每一个 Employee 对象都有一个自己的id字段，但这个类的所有实例将共享一个nextId字段。换句话说，如果有1000个Employee类对象，则有1000个实例字段id，分别对应每一个对象。但是，只有一个静态字段nextId。即使没有Employee对象，静态字段nextId也存在。**它属于类，而不属于任何单个的对象**。
> 在一些面向对象程序设计语言中，静态字段被称为类字段。术语“静态”只是沿用了C++的叫法，并无实际意义。

一句话讲就是静态方法是类的方法，静态字段是类的字段，可以直接通过类.方法进行调用，比如Math.sort();
### 静态常量
静态变量使用的比较少，但静态常量却很常用。比如，在Math类中定义一个静态常量:
```java
public class Math{
    public static final double PI = 3.1415923;
}
```
在程序中，可以用Math.PI来访问这个常量。

如果省略关键字static，PI就变成了Math类的一个实例字段。也就是说，需要通过Math类的一个对象类访问PI，并且每一个Math对象都有它自己的一个PI副本。

由于每个类对象都可以修改公共字段，所以，最后不要有公共字段。然而，公共常量(即final字段)却没问题，因为final不需要在进行修改。

>如果查看 System类，就会发现有一个 setOut方法可以将 System.out设置为不同的
流。你可能会感到奇怪，为什么这个方法可以修改final变量的值。原因在于， setout
方法是一个**原生**方法，而不是在Java语言中实现的。原生方法可以绕过Java语言的
访问控制机制。这是一种特殊的解决方法，你自己编写程序时不要模仿这种做法。
抽象类：abstract，抽象类用abstract定义，表示不能被实例的对象，只能被继承，抽象类里面的抽象方法不能被直接调用，需要其子类实现之后才能调用，而抽象方法充当着占位方法的角色，它们在子类中具体实现，拓展抽象类可以有两种选择。一种是在子类中保留抽象类中的部分或者所有抽象方法未定义，这样就必须将子类也标为抽象类；另一种做法是定义全部方法，这样一来，子类就不是抽象的了

抽象方法的一种应用
```java
    var people = new Person[2];//var变量，在jdk10以后投入使用
    people[0] = new Employee(...);
    people[1] = new Student(...);

    for(Peoson p : people){
        System.out.println(p.getName()+", "+p.getDescription());
    }
```
有人可能对下面这个调用感到困惑：
p.getDescription()
这不是调用了一个没有定义的方法吗？请牢记，用于不能构造抽象类Person的对象，所以变量p永远不会引用Person对象，而是引用诸如Employee或者Student这样的具体子类的对象，而这些对象都定义了getDescription方法。

## lambda表达式
1. 使用->表示，最简单的lambda表达式
(String first,String second) -> first.length() - second.length()
2. 如果代码要完成的计算无法放在一个表达式中，就可以像写方法一样，把这些代码放在{}中，并包含显示的return语句。
3. 即使lambda表达式没有参数，仍要提供空括号，就像无参数方法一样：
() -> {for(int i = 100; i>=0; i--) System.out.println(i);}
4. 如果可以推导出一个lambda表达式的参数类型，则可以忽略其类型
5. 如果方法只有一个参数，而且这个参数的类型可以推导出，那么甚至可以省略小括号
   
6. 无需指定lambda表达式的返回类型。lambda表达式的返回类型总是由上下文推导得出

## 方法引用
闭包：lambda表达式能捕获方法之外的变量，但是其必须是事实最终变量。事实最终变量是指，这个变量初始化之后就不会再为他赋新值

## Exception
Java语音规范将派生于Error类或RuntimeException类的所有异常成为非检查型(unchecked)异常，所有其他的异常称为检查型(checked)异常。
![异常分支](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/异常分支.webp)
总之，一个方法必须声明所有可能抛出的检查型异常，而非检查型异常要么在你的控制之外(Error),要么是由从一开始就应该避免的情况所导致的(RuntimeException)。如果你的方法没有声明所有可能发生的检查型异常，编译器就会发出一个错误消息

一般经验是，要捕获那些你知道如何处理的异常，而继续传播那些你不知道怎样处理的异常

## 调试技巧
1. 可以使用下面的方法打印或记录任意变量的值：
    System.out.println("x=" +x);
    如果x是一个数值，则会被转换成等价的字符串。如果x是一个对象，那么Java会调用这个对象 的toString方法。要想获得隐式参数对象的状态，可以打印this对象的状态。
    Java类库中的绝大部分类都覆盖了toString方法，以便能够提供有用的类信息。这样会使调试更加便捷。在你自定义的类中也应该这样做。

2. 还有一个不太为人所知但是非常有效的技巧，可以在每一个类中放置一个单的的main方法。这样就可以提供一个单元测试桩(stub)，能够独立地测试类

3. 使用Junit，Junit是一个非常流行的单元测试框架，利用它可以很容易地组织使用试用例套件

## 集合
### 队列和双端队列
**Queue<> queue = new LinkedList<>() 队列，底层用链表实现**<br>
**Queue<> queue = new ArrayDeque<>() 队列，底层用循环数组实现**
   - offer() 增加节点，如果超过容量会返回false，不会抛出异常
  
   - add() 增加节点，只不过如果超过容量会抛出异常提醒
   - remove() 删除节点，如果不存在，会抛出异常
   - poll() 删除节点
   - element()
   - peek() 如果队列不为空，返回这个队列队头的元素，但不删除。如果队列为空，第一个方法抛出NoSuchElementException，第二个返回null
   - isEmpty() 是否为空

**Queue<> queue = new ArrayDeque<>() 队列，底层用循环数组实现** <br>
**Deque<> queue = new LinkedList<>() 双端队列，底层用链表实现**
   - addFirst()
  
   - addLast()
   - offerFirst()
   - offerLast() 将给定的对象添加到双端队列的队头或者队尾，如果这个双端队列已满，前面两个方法将抛出IllegalStateException，而后面两个方法返回false
   - removeFirst()
   - removeLast()
   - pollFirst()
   - pollLast()
   - getFirst()
   - getLast()
   - peekFirst()
   - peekLast()
### 优先队列
优先队列(priority queue)中的元素可以按照任意的顺序插入，但会按照有序的顺序进行检索。也就是说，无论何时调用remove方法，总会获得当前优先队列中最小的元素。不过，优先队列并没有对所有元素进行排序。如果迭代处理这些元素，并不需要对它们进行排序。优先队列使用了一个精巧且高效的数据结构，称为堆(heap)。堆是一个可以自组织的二叉树，其添加(add)和删除(remove)操作可以让最小的元素移动到根，而不必花费时间对元素进行排序。与TreeSet一样，优先队列既可以保存实现了Comparable接口的类对象，也可以保存构造器中提供的Comparator对象。
### Collections工具栏常用api
![collections-api](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/算法/collections-api.webp)
## 并发
### 在一个单独的线程中运行一个任务的简单过程
有三种使用线程的方法：
- 实现 Runnable 接口；
- 实现 Callable 接口；
- 继承 Thread 类。

实现 Runnable 和 Callable 接口的类只能当做一个可以在线程中运行的任务，不是真正意义上的线程，因此最后还需要通过 Thread 来调用。可以说任务是通过线程驱动从而执行的。

### 继承 Thread 类
同样也是需要实现 run() 方法，因为 Thread 类也实现了 Runable 接口。
当调用 start() 方法启动一个线程时，虚拟机会将该线程放入就绪队列中等待被调度，当一个线程被调度时会执行该线程的 run() 方法。
```java
public class MyThread extends Thread {
    public void run() {
// ...
    }
}
public static void main(String[] args) {
    MyThread mt = new MyThread();
    //不能使用mt.run()，这样Java虚拟机不会在重新创建一个线程
    mt.start();
}
```
- 不能使用mt.run()，这样Java虚拟机不会在重新创建一个线程
- 要想启用多个线程，就创建多个对象

### Thread类的相关方法
- start() 启动线程，并执行对象的run()方法
- run() 调用run方法
- getName() 返回线程的名称
- setName() 设置该线程的名称
- yield() 释放当前线程的执行权力
- join() 在线程a中调用线程b的join(),此时线程a处于堵塞状态，直到线程b完全执行以后，线程a才结束堵塞状态
- sleep() 睡眠
- isAlive() 判断当前线程是否存活

### 实现Runnable接口
1. 创建一个实现了Runnable接口的类
2. 实现类去实现Runnable中的抽象方法：run()
3. 创建实现类的对象
4. 将此对象作为参数传递到Thread类的构造器中，创建Thread类的对象
5. 通过Thread类的对象调用start()

### 线程状态
线程有以下六种状态：
- New(新建)
- Runnable(可运行)
- Blocked(堵塞)
- Waiting(等待)
- Timed waiting(⏲等待)
- Terminated(终止)

### 解决线程安全
多个线程操作共享数据，会出现线程安全问题，加锁用来解决共享数据安全问题，
方法一：同步代码块
```java
synchronized (监视器){
            
        }
```
方法二：同步方法
如果操作共享数据的代码完整的声明在一个方法中，我们不妨将此方法声明为同步的
```java
public synchronized void test(){

    }
```
方法三：jdk5.0新增，lock类解决  

### 多线程死锁问题
不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己需要的同步资源，就形成的线程的死锁问题

### 多线程通信问题
- wait() 一旦执行此方法，当前线程就进入阻塞状态，并释放同步监视器
- notify() 一旦执行此方法，就会唤醒被wait的一个线程，如果有多个线程被wait，就唤醒优先级高的那个
- notifyAll() 一旦执行此方法，就会唤醒所有被wait的线程

说明：
- wait() notify() notifyAll()三个方法必须使用在同步代码块或者同步方法中
- wait() notify() notifyAll()三个方法的调用者必须是同步代码块或者同步方法中的同步监视器，否则会出现IllegalMonitorStateException异常
- wait() notify() notifyAll() 三个方法是定义在Java.lang.Object类中的

### 新增线程创建方式
1. 实现Callable接口
   与Runnable相比，Callable功能更强大
   - 有返回值
   - 可以抛出异常
   - 支持泛型返回
   - 需要使用FutureTask类
2. 实现过程
   - 创建一个实习Callable 的实现类
   - 实现call方法，可以有返回值
   - 创建对象
   - 将此Callable接口实现类的对象作为参数传递给FutureTask构造器中，创建FutureTask的对象
   - 将FutureTask作为参数，创建Thread对象，然后调用start方法

### 使用线程池
线程池的好处可以
- 提高响应速度(减少了创建新线程的时间)
- 降低资源消耗(重复利用线程池中的线程，不需要每次都创建)
- 便于线程管理
- corePoolSize：核心池的大小
- maximumPoolSize：最大线程数
- keepAliveTime：线程没有任务时最多保持多长时间后终止

## 日期相关api使用
SimpleDateFormat对日期Date类进行格式化和解析
```java
Date date = new Date();
SimpleDateFormat simpleDateFormat = new SimpleDateFormat();
//日期->字符串
String date =  simpleDateFormat.format(date);
//字符串->日期
Date date1 = simpleDateFormat.parse(date);

```

### jdk8新增关于时间的api
LocalDate,LocalTime,LocalDateTime，分别获取当前的日期，时间，日期+时间
```java
 //now()获取当前时间的日期，时间，日期加时间
        LocalDate now = LocalDate.now();
        LocalTime now1 = LocalTime.now();
        LocalDateTime localDateTime = LocalDateTime.now();
        System.out.println(now);
        System.out.println(now1);
        System.out.println(localDateTime);
        //of()设置指定的日期时间
        LocalDate localDate = LocalDate.of(2002, 10, 1);
        System.out.println(localDate);
        //get具体的数值
        int dayOfMonth = localDate.getDayOfMonth();
        //设置具体的数值，返回一个新的localDate，体现出不可变性
        LocalDate localDate1 = localDate.withDayOfMonth(21);
        //加时间，减时间 plus，minus
        //解析相关api ==> DateTimeFormatter.ofPattern()
        System.out.println(localDate1);
```

## java比较器的使用
### Comparable接口
对于自定义类，如果需要排序，我们可以让自定义类实现Comparable接口，重写compareTo方法
### Comparator接口
当元素的类型没有实现Comparable接口而又不方便修改代码，或者实现了Comparable接口的排序规格不适合当前的操作，那么可以考虑使用Comparator接口
```java
Array.sort(arr,new Comparator(){
    @Override
    public int compare(Object o1, Object o2){
        //具体的比较规则
    }
});
```
## 枚举类三个api
- values() 返回一个字符串，里面有枚举类所有的常量名
- valueOf(String objName) 返回枚举类中对象名是objName的对象
- toString() 返回指定对象的常量名

## 注解(Annotation)
定义新的Annotation类型使用@interface关键字
- 注解声明为 @interface
- 内部定义成员，通常使用value表示
- 可以指定成员的默认值，使用default定义
- 如果自定义注解没有成员，表明是一个标识作用
  
如果注解有成员。在使用注解时，需要指明成员的值
自定义注解必须配上注解的信息处理流程(使用反射)才有意义
jdk5.0提供了4个标准的meta-annotation类型，分别是
- Retention 指定所修饰的Annotation的生命周期：SOURCE\CLASS(默认行为)\RUNTIME，只有声明为RUNTIME生命周期的注解，才能通过反射获取。
- Target 用于指定被修饰的 Annotation 能用于修饰哪些程序元素
- Documented 表示所修饰的注解在被Javadoc解析时，保留下来
- Inherited 被他修饰的 Annotation 将具有保留性

## 集合
### 遍历集合(使用迭代器或者增强for循环)
集合对象每次调用iterator()方法都会得到一个全新的迭代器对象，默认游标都在集合的第一个元素之前，Iterator中的remove()方法，如果还未调用next()或在上一次调用next方法之后已经调用了remove方法，
再调用remove都会报ILLegalStateException。
```java
    @Test
    public void listTest(){
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(1);
        list.add(1);
        System.out.println(list);//打印的为[1,1,1]
        for(Integer i : list){//底层是迭代器实现的
            System.out.println(i);//直接把1，1，1打印出来
        } 
        Iterator<Integer> iterator = list.iterator();
        while (iterator.hasNext()){
            Integer next = iterator.next();////直接把1，1，1打印出来
            System.out.println(next);
        }
    }
```
### List，Set，Map接口
List接口：存储有序的、可重复的数据。
- ArrayList:作为List接口的主要实现类；线程不安全的，效率高；底层使用Object[] eLementData存储
- LinkedList:对于频繁的插入、删除操作，使用此类效率比ArrayList高：底层使用双向链表存储(Node节点)
- Vector:作为List接口的古老实现类；线程安全的，效率低；底层使用Object[] eLementData存储

set接口：存储无序的，不可重复的数据
- 无序性：不等于随机性，会通过计算哈希值来判断元素放到哪个位置
- 不可重复性：相同元素只添加一个

LinkedHashSet的使用：LinkedHashSet作为HashSet的子类，在添加数据的同时，每个数据还维护了两个引用，记录此数据的前一个数据和后一个数据，因此可以实现按照添加的顺序进行遍历，LinkedHashMap同理

TreeSet:会自动对插入的数据实现排序，定制排序需要使用比较器
### Map接口
接口实现图示
![map接口](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/算法/map接口.webp)

#### Map结构的理解：
Map中的key：无序的、不可重复的，使用set存储所有的key

Map中的value：无序的，可重复的，使用collection存储所有的value

一个键值对：key-value构成了一个Entry对象

Map中的entry：无序的，不可重复的，使用set存储所有的entry
- HashMap的底层实现原理，以jdk7为例：在实例化以后，底层创建了长度是16的一维数组Entry[] table,首先，调用key1所在类的hashCode()计算key1哈希值，此哈希值经过某种算法计算以后，得到在Entry数组中的存放位置。
如果此位置上的数据为空，此时的key1-value添加成功。
如果此位置上的数据不为空，（意味着此位置上存在一个或多个数据（以链表形式存在）），比较key1和已经存在的一个或多个数据的哈希值：
如果key1的哈希值与已经存在的数据的哈希值都不相同，此时key1-value1添加成功
如果key1的哈希值和已经存在的某一个数据(key2-value2)的哈希值相同，继续比较：调用key1所在类的equals(key2
如果equals()返回false:此时key1-value1添加成功。
如果equals()返回true:使用value1替换value2。
在不断添加的过程中，会涉及到扩容问题，默认的扩容方式：扩容到原来容量的2倍，并将所有数据复制过来

**jdk8的HashMap相对于jdk7底层实现的不同:**
- 1. new HashMap():底层没有创建一个长度为16的数组
  
- 2. jdk8底层的数组是：Node[], 而非Entry[]
- 3. 首次调用put()方法时，底层创建长度为16的数组
- 4. jdk7底层结构只有：数组+链表,jdk8中底层结构：数组+链表+红黑树。
当数组的某一个索引位置上的元素以链表形式存在的数据个数>8且当前数组的长度>64时，
此时此素引位置上的所有数据改为使用红黑存储。

遍历所有的key集：Set set = map.KeySet();
遍历所有的value集：Collection coll = map.values();
遍历k-v集合：
```java
Set entry = map.entrySet();
//然后随意遍历，任君发挥
Iterator iterator =  entry.iterator();
```

## IO流
### File类
创建File类的实例：
1、 public File(String pathname) ：通过将给定的路径名字符串转换为抽象路径名来创建新的 File实例。
2、 public File(String parent, String child) ：从父路径名字符串和子路径名字符串创建新的 File实例。
3、 public File(File parent, String child) ：从父抽象路径名和子路径名字符串创建新的 File实例。

### 流的分类
- 按操作数据单位不同分为：字节流(InputStream,OutStream)，字符流(Read,Writer)
- 按照数据流的流向不同分为：输入流，输出流
- 按流的角色不同分为：节点流，处理流

|        | 输入流              | 输出流               |
| ------ | ---------------------- | ----------------------- |
| 字节流 | 字节输入流 InputStream | 字节输出流 OutputStream |
| 字符流 | 字符输入流 Reader | 字符输出流 Writer  |
![流](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/算法/流.webp)

### 字节输出流（OutputStream）
字节输出流的基本共性功能方法:
- 1、 public void close() ：关闭此输出流并释放与此流相关联的任何系统资源。
- 2、 public void flush() ：刷新此输出流并强制任何缓冲的输出字节被写出。
- 3、 public void write(byte[] b)：将 b.length个字节从指定的字节数组写入此输出流。
- 4、 public void write(byte[] b, int off, int len) ：从指定的字节数组写入 len字节，从偏移量 off开始输出到此输出流。 也就是说从off个字节数开始读取一直到len个字节结束
- 5、 public abstract void write(int b) ：将指定的字节输出流。

**以上五个方法则是字节输出流都具有的方法，由父类OutputStream定义提供，子类都会共享以上方法**

#### FileOutputStream类
OutputStream有很多子类，我们从最简单的一个子类FileOutputStream开始。看名字就知道是文件输出流，用于将数据写出到文件。
构造方法:
- 1、 public FileOutputStream(File file)：根据File对象为参数创建对象。
- 2、 public FileOutputStream(String name)： 根据名称字符串为参数创建对象。
- 3、public FileOutputStream(File file, boolean append)
- 4、public FileOutputStream(String name, boolean append)


这两个构造方法，第二个参数中都需要传入一个boolean类型的值，true 表示追加数据，false 表示不追加也就是清空原有数据。这样创建的输出流对象，就可以指定是否追加续写了，至于Windows换行则是 \n\r ，下面将会详细讲到。
#### FileOutputStream写出字节数据
```java
public void write(int b)
public void write(byte[] b)
public void write(byte[] b,int off,int len)  //从`off`索引开始，`len`个字节

```
###  字节输入流（InputStream）
字节输入流的基本共性功能方法:
- 1、 public void close() ：关闭此输入流并释放与此流相关联的任何系统资源。
- 2、public abstract int read()： 从输入流读取数据的下一个字节。
- 3、 public int read(byte[] b)： 该方法返回的int值代表的是读取了多少个字节，读到几个返回几个，读取不到返回-1

#### FileInputStream读取字节数据
1.读取字节：read方法，每次可以读取一个字节的数据，提升为int类型，读取到文件末尾，返回-1
```java
public class FISRead {
    public static void main(String[] args) throws IOException{
      	// 使用文件名称创建流对象
       	FileInputStream fis = new FileInputStream("read.txt");
      	// 定义变量，保存数据
        int b ；
        // 循环读取
        while ((b = fis.read())!=-1) {
            System.out.println((char)b);
        }
		// 关闭资源
        fis.close();
    }
}
```
2.使用字节数组读取：read(byte[] b)，每次读取b的长度个字节到数组中，返回读取到的有效字节个数，读取到末尾时，返回-1 ，代码使用演示：
```java
public class FISRead {
    public static void main(String[] args) throws IOException{
      	// 使用文件名称创建流对象.
       	FileInputStream fis = new FileInputStream("read.txt"); // read.txt文件中内容为abcde
      	// 定义变量，作为有效个数
        int len ；
        // 定义字节数组，作为装字节数据的容器   
        byte[] b = new byte[2];
        // 循环读取
        while (( len= fis.read(b))!=-1) {
           	// 每次读取后,把数组变成字符串打印
            System.out.println(new String(b));
        }
		// 关闭资源
        fis.close();
    }
}

输出结果：
ab
cd
ed
```
在开发中一般强烈推荐使用数组读取文件，代码如下：
```java
package io;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class input2 {
    public static void main(String args[]){
        FileInputStream inputStream = null;
        try {
            inputStream = new FileInputStream("a.txt");
            int len = 0 ;
            byte[] bys = new byte[1024];
            while ((len = inputStream.read(bys)) != -1) {
                System.out.println(new String(bys,0,len));
            }
        
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            try {
                inputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }
}
```

复制图片文件，代码如下：
```java
public class Copy {
    public static void main(String[] args) throws IOException {
        // 1.创建流对象
        // 1.1 指定数据源
        FileInputStream fis = new FileInputStream("D:\\test.jpg");
        // 1.2 指定目的地
        FileOutputStream fos = new FileOutputStream("test_copy.jpg");

        // 2.读写数据
        // 2.1 定义数组
        byte[] b = new byte[1024];
        // 2.2 定义长度
        int len;
        // 2.3 循环读取
        while ((len = fis.read(b))!=-1) {
            // 2.4 写出数据
            fos.write(b, 0 , len);
        }

        // 3.关闭资源
        fos.close();
        fis.close();
    }
}

```
### 字符输入流（Reader）、字符输出流（Writer）
对txt等文本文件进行处理，方法和上面差不多，但是不能对视频音频等文件进行操作

## 缓冲流
缓冲流,也叫高效流，是对4个FileXxx 流的“增强流”。
**缓冲流的基本原理：**

- 1、使用了底层流对象从具体设备上获取数据，并将数据存储到缓冲区的数组内。
- 2、通过缓冲区的read()方法从缓冲区获取具体的字符数据，这样就提高了效率。
- 3、如果用read方法读取字符数据，并存储到另一个容器中，直到读取到了换行符时，将另一个容器临时存储的数据转成字符串返回，就形成了readLine()功能。

也就是说在创建流对象时，会创建一个内置的默认大小的缓冲区数组，通过缓冲区读写，减少系统IO次数，从而提高读写的效率。
缓冲书写格式为BufferedXxx，按照数据类型分类：

- 字节缓冲流：BufferedInputStream，BufferedOutputStream
- 字符缓冲流：BufferedReader，BufferedWriter

```java
public class BufferedDemo {
    public static void main(String[] args) throws FileNotFoundException {
      	// 记录开始时间
        long start = System.currentTimeMillis();
		// 创建流对象
        try (
		 BufferedInputStream bis = new BufferedInputStream(new FileInputStream("py.exe"));
		 BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("copyPy.exe"));
        ){
          	// 读写数据
            int len;
            byte[] bytes = new byte[8*1024];
            while ((len = bis.read(bytes)) != -1) {
                bos.write(bytes, 0 , len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
		// 记录结束时间
        long end = System.currentTimeMillis();
        System.out.println("缓冲流使用数组复制时间:"+(end - start)+" 毫秒");
    }
}
//缓冲流使用数组复制时间:521 毫秒  
```

## 转换流
![转换流](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/转换流.webp)
### 字符编码与解码
编码:字符(能看懂的)--字节(看不懂的)

解码:字节(看不懂的)-->字符(能看懂的)
### 字符集
![字符集](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/字符集.webp)

###  InputStreamReader类-----(字节流到字符流的桥梁)
转换流java.io.InputStreamReader，是Reader的子类，从字面意思可以看出它是从字节流到字符流的桥梁。它读取字节，并使用指定的字符集将其解码为字符。它的字符集可以由名称指定，也可以接受平台的默认字符集。
#### 构造方法
InputStreamReader(InputStream in): 创建一个使用默认字符集的字符流。
InputStreamReader(InputStream in, String charsetName): 创建一个指定字符集的字符流。
```java
public class ReaderDemo2 {
    public static void main(String[] args) throws IOException {
      	// 定义文件路径,文件为gbk编码
        String FileName = "C:\\A.txt";
      	// 创建流对象,默认UTF8编码
        InputStreamReader isr = new InputStreamReader(new FileInputStream(FileName));
      	// 创建流对象,指定GBK编码
        InputStreamReader isr2 = new InputStreamReader(new FileInputStream(FileName) , "GBK");
		// 定义变量,保存字符
        int read;
      	// 使用默认编码字符流读取,乱码
        while ((read = isr.read()) != -1) {
            System.out.print((char)read); // �����ʺ      
        }
        isr.close();
      
      	// 使用指定编码字符流读取,正常解析
        while ((read = isr2.read()) != -1) {
            System.out.print((char)read);// 哥敢摸屎
        }
        isr2.close();
    }
}
```
### OutputStreamWriter类-----(字符流到字节流的桥梁)
转换流java.io.OutputStreamWriter ，是Writer的子类，字面看容易混淆会误以为是转为字符流，其实不然，OutputStreamWriter为从字符流到字节流的桥梁。使用指定的字符集将字符编码为字节。它的字符集可以由名称指定，也可以接受平台的默认字符集。
```java
public class OutputDemo {
    public static void main(String[] args) throws IOException {
      	// 定义文件路径
        String FileName = "C:\\s.txt";
      	// 创建流对象,默认UTF8编码
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(FileName));
        // 写出数据
      	osw.write("哥敢"); // 保存为6个字节
        osw.close();
      	
		// 定义文件路径
		String FileName2 = "D:\\A.txt";
     	// 创建流对象,指定GBK编码
        OutputStreamWriter osw2 = new OutputStreamWriter(new FileOutputStream(FileName2),"GBK");
        // 写出数据
      	osw2.write("摸屎");// 保存为4个字节
        osw2.close();
    }
}
```
## 序列化
Java 提供了一种对象序列化的机制。用一个字节序列可以表示一个对象，该字节序列包含该对象的数据、对象的类型和对象中存储的属性等信息。字节序列写出到文件之后，相当于文件中持久保存了一个对象的信息。

反之，该字节序列还可以从文件中读取回来，重构对象，对它进行反序列化。对象的数据、对象的类型和对象中存储的数据信息，都可以用来在内存中创建对象。看图理解序列化：
![转换流](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/转换流.webp)

一个对象要想序列化，必须满足两个条件:
该类必须实现java.io.Serializable 接口，Serializable 是一个标记接口，不实现此接口的类将不会使任何状态序列化或反序列化，会抛出NotSerializableException。

该类的所有属性必须是可序列化的。如果有一个属性不需要可序列化的，则该属性必须注明是瞬态的，使用transient 关键字修饰。

Serializable 接口给需要序列化的类，提供了一个序列版本号。serialVersionUID 该版本号的目的在于验证序列化的对象和对应类是否版本匹配。
```java
public class Employee implements Serializable {
     // 加入序列版本号
     private static final long serialVersionUID = 1L;
     //里面的每个属性都需要序列化
     public String name;
     public String address;
}
```

## ObjectOutputStream类，ObjectInputStream类
java.io.ObjectOutputStream 类，将Java对象的原始数据类型写出到文件,实现对象的持久存储。

ObjectInputStream反序列化流，将之前使用ObjectOutputStream序列化的原始数据恢复为对象。
```java
public class DeserializeDemo {
   public static void main(String [] args)   {
        Employee e = null;
        try {		
             // 创建反序列化流
             FileInputStream fileIn = new FileInputStream("employee.txt");
             ObjectInputStream in = new ObjectInputStream(fileIn);
             // 读取一个对象
             e = (Employee) in.readObject();
             // 释放资源
             in.close();
             fileIn.close();
        }catch(IOException i) {
             // 捕获其他异常
             i.printStackTrace();
             return;
        }catch(ClassNotFoundException c)  {
        	// 捕获类找不到异常
             System.out.println("Employee class not found");
             c.printStackTrace();
             return;
        }
        // 无异常,直接打印输出
        System.out.println("Name: " + e.name);	// zhangsan
        System.out.println("Address: " + e.address); // beiqinglu
        System.out.println("age: " + e.age); // 0
    }
}
```

## stream api
### stream的介绍
stream的三个步骤：
- 创建stream：一个数据源，获取一个流
- 中间操作：一个中间操作链，对数据源的数据进行处理
- 终止操作：一旦执行终止操作，就执行中间操作链，并产生结果，之后不会再使用

注意：
- stream 自己不会存储数据
- stream 不会改变源对象，相反，他们会返回一个持有结果的新的stream
- stream 操作是延迟执行的


### stream的中间操作
#### 1.筛选和切片
- filter(Predicate p)————接收Lambda，从流中排除某些元素
- limit(n)————截断流，使元素不超过给定数量
- skip(n)————跳过元素，返回一个跳过前n个元素的流，如果没有n个，则返回一个空流
- distinct()————筛选，通过流所生成的元素的hashcode()和equals()去除重复元素

#### 2.映射
- map(Function f)————接收一个函数作为参数，将元素转为其他形式或提取信息，该函数会应用到每个元素上，并将其映射成新的元素
- flatMap(Function f)————接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有的流连接成一个流

#### 3.排序
- sorted()————自然排序
- sorted(Compator com)————定制排序

### stream的终止操作
#### 匹配和查找
#### 规约
#### 收集
collect(Collector c),里面调用Collectors工具类里面封装的方法