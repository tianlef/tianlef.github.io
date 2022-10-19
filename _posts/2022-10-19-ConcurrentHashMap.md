# ConcurrentHashMap

## HashMap

- Entry对象数组
- 数组中的每一个元素是一个带有链表的头节点
- HashMap不是线程安全的
- 在并发的情况下，有可能在链表中形成环形链表
  - 链表主要产生在重新哈希的过程中
  - HashMap.Size   >=  Capacity * LoadFactor
  - 1.扩容
    - 创建一个新的Entry空数组，长度是原数组的2倍。
  - 2.ReHash
    - 遍历原Entry数组，把所有的Entry重新Hash到新数组。为什么要重新Hash呢？因为长度扩大以后，Hash的规则也随之改变。
    - 让我们回顾一下Hash公式：
    - index =  HashCode（Key） &  （Length - 1
    - 两个线程A、B同时到达容量临界点，同时进行扩容
    - 线程A顺利执行完扩容，并更新了Map中的数组
    - B next 指向被 A 数组更改
    - 头插法 3 - 2 - 3 产生一个环路
    - 由于环路的存在，导致当查找一个不存在的key，但这个key的hash值为这个环路指向的值，就会产生死循环
  
  ```java
    void transfer(Entry[] newTable, boolean rehash) {
      int newCapacity = newTable.length;
      for (Entry<K,V> e : table) {
        while(null != e) {
            Entry<K,V> next = e.next;
            if (rehash) {
                e.hash = null == e.key ? 0 : hash(e.key);
            }
            int i = indexFor(e.hash, newCapacity);
            e.next = newTable[i];
            newTable[i] = e;
            e = next;
        }
      }
    }
    ```
- 红黑树
  - 当链表长度大于8时，将链表转化成红黑树，以加快检索速度
  

## ConcurrentHashMap

- Map并发安全的解决办法
  - HashTable
  - Collections.synchronizedMap
    - 无论读操作还是写操作，给整体的集合加锁，导致其他操作被阻塞
  - ConcurrentMap
- 关键概念：Segement
  - 相当于HashMap对象
  - ConcurrentHashMap中有2的N次方个Segment
  - 是一个二级哈希表
  - 类似于数据库的水平拆分
- 不同Segment的写入是可以并发执行的
- 同一segment的写和读可以并发执行
- 同一segment的写入需要上锁
- ConcurrentHashMap读写均需要二次定位 segment segment的数组下标
- Size的计算
  - 并发的问题
    - 统计所有segemnt元素的修改次数
    - 判断总修改次数是否大于上一次的总修改次数
    - 有差别的的，就重新统计一次
    - 大致思路是 检测 - 重做 - 定义重做的阈值
    - 超过阈值 加锁统计
    - 乐观锁 悲观锁


