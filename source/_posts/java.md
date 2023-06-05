---
title: java
date: 2023-06-01 20:10:32
categories:
 -java
---

___

<!-- more -->

# 一.HashMap

HashMap 是一个散列表，它存储的内容是键值(key-value)映射。
HashMap 的 key 与 value 类型可以相同也可以不同，根据定义，不受限制。

## 1.1 基础操作（增、删、改、查）

定义一个哈希表

```
HashMap<Integer, String> hashmap= new HashMap<Integer, String>();
```

添加键值对（key-value）（增）
hashmap.put(1, "string1"); // 执行完后hash表内为{1=string1}
hashmap.put(2, "string2"); // 执行完后hash表内为{1=string1, 2=string2}
hashmap.put(2, "string2"); // 执行完后hash表内为{1=string1, 2=string2, 3=string3}

根据key值访问value（查）
hashmap.get(1); // 返回string1
hashmap.get(2); // 返回string2
hashmap.get(3); // 返回string3

根据key值删除元素（删）
hashmap.remove(1); // 执行完后hash表内为{2=string2, 3=string3}
hashmap.remove(2); // 执行完后hash表内为{3=string3}
hashmap.remove(3); // 执行完后hash表内为{}
// 删除所有键值对
hashmap.clear();

替换 hashMap 中是指定的key对应的 value
hashmap.replace(key,value); // 返回0

返回hashmap中键值对的数量
hashmap.size(); // 返回0

getOrDefault(Object key, V defaultValue)
意思就是当Map集合中有这个key时，就使用这个key对应的value值，如果没有就使用默认值defaultValue；
hashmap.getOrDefault(key,defaultValue);

entrySet() 方法可以与 for-each 循环一起使用，用来遍历迭代 HashMap 中每一个映射项
// foreach循环

```
for(var entry : map.entrySet()){
    // 获得key
    int key = entry.getKey();
	// 获得value
	int value = entry.getValue();
}
```

## 1.2其他操作

检查hashMap中是否存在指定的key对应的映射关系
hashmap.containsKey(key); 

检查hashMap中是否存在指定的value对应的映射关系
hashmap.containsValue(value); 

hashmap是否为空
hashmap.isEmpty(); 

HashMap.values() 方法
hashmap.values(); // 返回所有Value值组成的集合
/*
	如果有HashMap: {1=Google, 2=Runoob, 3=Taobao}
	则返回Values: [Google, Runoob, Taobao]
*/

# 2.HashSet

HashSet是基于HashMap的一个不允许有重复元素的集合，但其中允许存在null值。

## 2.1 基础操作（增、删、改、查）

定义一个hashset

```
Set<Integer> hashset= new HashSet<Integer>();
```

添加值（增）
hashset.add(1);

迭代存入元素

```
int[] nums = new int[]{1,2,3,4,5,6}
for(int x : nums) hash.add(x);
```

判断元素是否存在
hashmap.contains(1);

删除元素（删）
hashmap.remove(1);
// 删除所有元素
hashmap.clear();

## 2.2 HashSet如何实现去重？

HashSet是通过HashCode()与equals()方法实现去重的。

HashCode()的作用是对Java堆上的对象产生一个哈希码，用于确定该对象在哈希表中的索引位置，Java的任何类中都含有HashCode()。
equals()方法用于比较两个对象中存放的地址是否相等。
对象加入HashSet的过程：对象加入HashSet时，HashSet会计算对象的hashcode值来判断对象加入的位置，如果该位置没有值，则直接放入；如果有值，则HashSet通过equals()方法来检查两个对象是否相同，如果两者相同，则加入失败，否则将会重新散列到其他的地方。

注意：

两个对象相等，也就是适用于equals(java.lang.Object) 方法，那么这两个对象的hashCode一定要相等；
hashcode相等，两个对象不一定相等

# 二、字符串操作注意

substring函数使用 sunstring(i,j)为截取字符串，左闭右开

整数转为字符串 String.valueof()

字符串拼接时可以使用stringbuilder

# 三、java泛型 list

定义：List<类型>名字=new ArrayList<>()
作用：自动检查类型，取消类型转换
list.clear()清空集合但是其他引用该list也会被清空

对list排序
collections.sort()//默认降序

# 四、collections类

Java.util.Collections是一个集合工具类，用于操作LIst，Set，Map等集合。
Collections类提供了一系列的静态方法，可以实现对集合元素的排序，添加一些元素，随机排序，替换等操作。

# 五、java对数组排序

arrays.sort()//升序

# 六、java各接口

|----Collection接口：单列集合，用来存储一个一个的对象
|----List接口：存储有序的、可重复的数据。 -->“动态”数组
|----ArrayList、LinkedList、Vector
|----Set接口：存储无序的、不可重复的数据 -->高中讲的“集合”
|----HashSet、LinkedHashSet、TreeSet
|----Map接口：双列集合，用来存储一对(key - value)一对的数据 -->高中函数：y = f(x)
|----HashMap、LinkedHashMap、TreeMap、Hashtable、Properties

# 七、字符串操作

## 1.String

String是不可变的，即一旦一个String对象被创建以后，包含在这个对象中的字符序列是不可改变的，直至这个对象被销毁。

## 2.StringBuffer

StringBuffer对象则代表一个字符序列可变的字符串，当一个StringBuffer被创建以后，
通过StringBuffer提供的append()、insert()、reverse()、setCharAt()、setLength()等方法
可以改变这个字符串对象的字符序列。一旦通过StringBuffer生成了最终想要的字符串，
就可以调用它的toString()方法将其转换为一个String对象。
StringBuffer对象是一个字符序列可变的字符串，它没有重新生成一个对象，而且在原来的对象中可以连接新的字符串。

## 3.StringBuilder类也代表可变字符串对象。

实际上，StringBuilder和StringBuffer基本相似，两个类的构造器和方法也基本相同。
不同的是：StringBuffer是线程安全的，而StringBuilder则没有实现线程安全功能，
所以性能略高。

## 4.Integer.parseInt（s）将字符串解析为整形
