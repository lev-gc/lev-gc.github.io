---
title: Java Collection
date: 2015-08-15 11:24:00
tags: [Java]
---

http://skyuck.iteye.com/blog/526358

在 Java2中，有一套设计优良的接口和类组成了Java集合框架Collection，使程序员操作成批的数据或对象元素极为方便。这些接口和类有很多对抽象数据类型操作的API，而这是我们常用的且在数据结构中熟知的。例如Map，Set，List等。并且Java用面向对象的设计对这些数据结构和算法进行了封装，这就极大的减化了程序员编程时的负担。程序员也可以以这个集合框架为基础，定义更高级别的数据抽象，比如栈、队列和线程安全的集合等，从而满足自己的需要。

Java2的集合框架，抽其核心，主要有三种：List、Set和Map。如下图所示：

需要注意的是，这里的 Collection、List、Set和Map都是接口（Interface），不是具体的类实现。 List lst = new ArrayList(); 这是我们平常经常使用的创建一个新的List的语句，在这里， List是接口，ArrayList才是具体的类。

常用集合类的继承结构如下：
Collection<--List<--Vector
Collection<--List<--ArrayList
Collection<--List<--LinkedList
Collection<--Set<--HashSet
Collection<--Set<--HashSet<--LinkedHashSet
Collection<--Set<--SortedSet<--TreeSet
Map<--SortedMap<--TreeMap
Map<--HashMap

-----------------------------------------------SB分割线------------------------------------------

List：
List是有序的Collection，使用此接口能够精确的控制每个元素插入的位置。用户能够使用索引（元素在List中的位置，类似于数组下 >标）来访问List中的元素，这类似于Java的数组。

Vector：
基于数组（Array）的List，其实就是封装了数组所不具备的一些功能方便我们使用，所以它难易避免数组的限制，同时性能也不可能超越数组。所以，在可能的情况下，我们要多运用数组。另外很重要的一点就是Vector是线程同步的(sychronized)的，这也是Vector和ArrayList 的一个的重要区别。

ArrayList：
同Vector一样是一个基于数组上的链表，但是不同的是ArrayList不是同步的。所以在性能上要比Vector好一些，但是当运行到多线程环境中时，可需要自己在管理线程的同步问题。

LinkedList：
LinkedList不同于前面两种List，它不是基于数组的，所以不受数组性能的限制。
它每一个节点（Node）都包含两方面的内容：
1.节点本身的数据（data）；
2.下一个节点的信息（nextNode）。
所以当对LinkedList做添加，删除动作的时候就不用像基于数组的ArrayList一样，必须进行大量的数据移动。只要更改nextNode的相关信息就可以实现了，这是LinkedList的优势。

List总结：
所有的List中只能容纳单个不同类型的对象组成的表，而不是Key－Value键值对。例如：[ tom,1,c ]

所有的List中可以有相同的元素，例如Vector中可以有 [ tom,koo,too,koo ]

所有的List中可以有null元素，例如[ tom,null,1 ]

基于Array的List（Vector，ArrayList）适合查询，而LinkedList 适合添加，删除操作


--------------------------------------NB分割线------------------------------------

Set：
Set是一种不包含重复的元素的无序Collection。

HashSet：
虽然Set同List都实现了Collection接口，但是他们的实现方式却大不一样。List基本上都是以Array为基础。但是Set则是在 HashMap的基础上来实现的，这个就是Set和List的根本区别。HashSet的存储方式是把HashMap中的Key作为Set的对应存储项。看看 HashSet的add（Object obj）方法的实现就可以一目了然了。
Java代码  收藏代码
public boolean add(Object obj) {
   return map.put(obj, PRESENT) == null;
}

这个也是为什么在Set中不能像在List中一样有重复的项的根本原因，因为HashMap的key是不能有重复的。

LinkedHashSet：
HashSet的一个子类，一个链表。

TreeSet：
SortedSet的子类，它不同于HashSet的根本就是TreeSet是有序的。它是通过SortedMap来实现的。

Set总结：
Set实现的基础是Map（HashMap）

Set中的元素是不能重复的，如果使用add(Object obj)方法添加已经存在的对象，则会覆盖前面的对象


--------------------------------------2B分割线------------------------------------

Map：
Map 是一种把键对象和值对象进行关联的容器，而一个值对象又可以是一个Map，依次类推，这样就可形成一个多级映射。对于键对象来说，像Set一样，一个 Map容器中的键对象不允许重复，这是为了保持查找结果的一致性;如果有两个键对象一样，那你想得到那个键对象所对应的值对象时就有问题了，可能你得到的并不是你想的那个值对象，结果会造成混乱，所以键的唯一性很重要，也是符合集合的性质的。当然在使用过程中，某个键所对应的值对象可能会发生变化，这时会按照最后一次修改的值对象与键对应。对于值对象则没有唯一性的要求，你可以将任意多个键都映射到一个值对象上，这不会发生任何问题（不过对你的使用却可能会造成不便，你不知道你得到的到底是那一个键所对应的值对象）。

Map有两种比较常用的实现：HashMap和TreeMap。

HashMap也用到了哈希码的算法，以便快速查找一个键，

TreeMap则是对键按序存放，因此它便有一些扩展的方法，比如firstKey(),lastKey()等，你还可以从TreeMap中指定一个范围以取得其子Map。
键和值的关联很简单，用put(Object key,Object value)方法即可将一个键与一个值对象相关联。用get(Object key)可得到与此key对象所对应的值对象。

--------------------------------------JB分割线------------------------------------

其它：
一、几个常用类的区别
1．ArrayList: 元素单个，效率高，多用于查询
2．Vector: 元素单个，线程安全，多用于查询
3．LinkedList:元素单个，多用于插入和删除
4．HashMap: 元素成对，元素可为空
5．HashTable: 元素成对，线程安全，元素不可为空

二、Vector、ArrayList和LinkedList
大多数情况下，从性能上来说ArrayList最好，但是当集合内的元素需要频繁插入、删除时LinkedList会有比较好的表现，但是它们三个性能都比不上数组，另外Vector是线程同步的。所以：
如果能用数组的时候(元素类型固定，数组长度固定)，请尽量使用数组来代替List；
如果没有频繁的删除插入操作，又不用考虑多线程问题，优先选择ArrayList；
如果在多线程条件下使用，可以考虑Vector；
如果需要频繁地删除插入，LinkedList就有了用武之地；
如果你什么都不知道，用ArrayList没错。

三、Collections和Arrays
在 Java集合类框架里有两个类叫做Collections（注意，不是Collection！）和Arrays，这是JCF里面功能强大的工具，但初学者往往会忽视。按JCF文档的说法，这两个类提供了封装器实现（Wrapper Implementations）、数据结构算法和数组相关的应用。
想必大家不会忘记上面谈到的“折半查找”、“排序”等经典算法吧，Collections类提供了丰富的静态方法帮助我们轻松完成这些在数据结构课上烦人的工作：
binarySearch：折半查找。

sort：排序，这里是一种类似于快速排序的方法，效率仍然是O(n * log n)，但却是一种稳定的排序方法。

reverse：将线性表进行逆序操作，这个可是从前数据结构的经典考题哦！

rotate：以某个元素为轴心将线性表“旋转”。

swap：交换一个线性表中两个元素的位置。
……
Collections还有一个重要功能就是“封装器”（Wrapper），它提供了一些方法可以把一个集合转换成一个特殊的集合，如下：

unmodifiableXXX：转换成只读集合，这里XXX代表六种基本集合接口：Collection、List、Map、Set、SortedMap和SortedSet。如果你对只读集合进行插入删除操作，将会抛出UnsupportedOperationException异常。

synchronizedXXX：转换成同步集合。

singleton：创建一个仅有一个元素的集合，这里singleton生成的是单元素Set，
singletonList和singletonMap分别生成单元素的List和Map。

空集：由Collections的静态属性EMPTY_SET、EMPTY_LIST和EMPTY_MAP表示。

这次关于Java集合类概述就到这里，下一次我们来讲解Java集合类的具体应用，如List排序、删除重复元素。