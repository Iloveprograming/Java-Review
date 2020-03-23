# **Java集合**
### **集合的由来**  

* 数组一旦初始化后，长度就确定了，存储数据对象不能达到动态扩展。
* 数组存储元素不便于对数组进行添加、修改、删除操作。
* 数组可以存储重复元素，但很多时候有去重操作。  

这时就需要使用新的方式存储多个元素  
***

集合分为Collection和Map两种体系
![集合继承关系](collections_1.jpg)
***


 ## **Collection集合**  
 Collection下有几个常用子接口：Set、Queue、List。然后是是一些抽象类，最后是具体实现类。  
 
### **Set集合**
一个不包含重复元素的 collection。更确切地讲，set 不包含满足 `e1.equals(e2)` 的元素对 `e1` 和 `e2`，并且最多包含一个 null 元素。正如其名称所暗示的，此接口模仿了数学上的 _set_ 抽象。 
主要的实现类是：**HashSet**和**TreeSet**  

***

#### **HashSet**

**HashSet常用方法**  
```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
{
  //HashSet集合内部与map集合联系起来的参数
  private transient HashMap<E,Object> map;
  
  //添加元素时使用的value值
  private static final Object PRESENT = new Object();
  
  //HashSet无参构造方法，通过HashMap实现HashSet集合
  public HashSet() 
  {
    map = new HashMap<>(); 
  }
 
  //集合中没有元素时返回true
  public boolean isEmpty() 
  {
    return map.isEmpty(); 
  }

  //集合包含对象o时返回true
  public boolean contains(Object o) 
  {
    return map.containsKey(o); 
  }
  
  //向集合中添加元素相当于通过map中put方法添加key，value值为系统创建的object对象
  //底层调用map的put方法
  public boolean add(E e) 
  {
    return map.put(e, PRESENT)==null; 
  }

  //如果集合中存在Object o则删除
  public boolean remove(Object o) 
  {
    return map.remove(o)==PRESENT; 
  }

  
}
```

   
HashSet底层由HashMap来实现，为哈希表结构，新增元素相当于HashMap的key，value默认为一个固定的Object。

1. HashSet继承AbstractSet类，获得了Set接口大部分的实现，减少了实现此接口所需的工作
2. HashSet实现了Set接口，获取Set接口的方法，可以自定义具体实现，也可以继承AbstractSet类中的实现
3. HashSet实现Cloneable，得到了clone()方法，可以实现克隆功能
4. HashSet实现Serializable，表示可以被序列化，通过序列化去传输

**特点**
*   不允许出现重复因素
*   允许插入Null值
*   元素无序（添加顺序和遍历顺序不一致）
*   **线程不安全**

**注意** ：向HashSet中添加元素时由于底层使用map中的put方法，所以判断元素是否重复是通过map中的**hashCode**()方法和**equals**()方法。

***

#### **TreeSet**  

1. 与HashSet同理，TreeSet继承AbstractSet类，获得了Set集合基础实现操作
2. TreeSet实现NavigableSet接口，而NavigableSet又扩展了SortedSet接口。这两个接口主要定义了搜索元素的能力，例如给定某个元素，查找该集合中比给定元素大于、小于、等于的元素集合，或者比给定元素大于、小于、等于的元素个数；**简单地说，实现NavigableSet接口使得TreeSet具备了元素搜索功能**
3. TreeSet实现Cloneable接口，意味着它也可以被克隆
4. TreeSet实现了Serializable接口，可以被序列化


**特点**
*   对插入的元素进行排序，是一个有序的集合
*   底层使用红黑树结构，而不是哈希表结构
*   允许插入Null值
*   不允许插入重复元素
*   **线程不安全**  


**注意**
TreeSet在插入元素时会对元素进行排序，其中分为自然排序（默认）和自定义排序
1. 当元素类型为基本数据类型或字符串时，会按照字母或者数字顺序顺序排列
2. 当元素类型是某个类的对象时，必须实现**Comparable接口**，不然会报**ClassCastException**错


***

### **List集合**  
有序的 collection（也称为_序列_）。此接口的用户可以对列表中每个元素的插入位置进行精确地控制。用户可以根据元素的整数索引（在列表中的位置）访问元素，并搜索列表中的元素。

主要的实现类是：**LinkedList**和**ArrayList**  

***

#### **LinkedList**  
**特点**
底层通过链表来实现，随着元素的增加不断向链表的后端增加节点。
*   容量不固定，随着容量的增加而动态扩容
*   有序集合
*   插入的元素可以为null
*   底层通过链表实现（**JDK1.7之前的版本是环形链表，而到了JDK1.7以后进行了优化，变成了直线型双链表结构**）
*   线程不安全  

在LinkedList中存储了first头结点和last尾结点

```java
public boolean add(E e) {
    linkLast(e);
 return true; 
 }

/**
 * Links e as last element. */ void linkLast(E e) {
    final Node<E> l = last;
 final Node<E> newNode = new Node<>(l, e, null);
  last = newNode;
 if (l == null)
        first = newNode;
 else  l.next = newNode;
  size++;
  modCount++; 
 }
```

LinkedList默认插入方法为尾插法

**简单来说LinkedList是一个保存头结点，尾结点的直线型双链表**
**基于以上特点：每次查找要么从头开始，要么从尾开始，查询效率相对于ArrayList较差**

***

#### **ArrayList**
##### **特点**
*   容量不固定，随着容量的增加而动态扩容
*   有序集合
*   插入的元素可以为null
*   底层通过数组实现，随机访问性能高
*   线程不安全
##### **容量与扩容**  
```java
/* Default initial capacity. */ 
 private static final int DEFAULT_CAPACITY = 10;
```

由此可知初始容量为10

接下来探究一下ArrayList的add方法
```java
public boolean add(E e) {
    ensureCapacityInternal(size + 1); // Increments modCount!!
    elementData[size++] = e;
    return true; 
}

private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity)); 
}

private static int calculateCapacity(Object[] elementData, int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity; 
}

private void ensureExplicitCapacity(int minCapacity) {
    modCount++;    // overflow-conscious code
  if (minCapacity - elementData.length > 0)
        grow(minCapacity); 
}

private void grow(int minCapacity) {
    // overflow-conscious code
  int oldCapacity = elementData.length;
  int newCapacity = oldCapacity + (oldCapacity >> 1);
  if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
  if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
  // minCapacity is usually close to size, so this is a win:
  //// 复制elementData返回一个新的数组对象，这个时候list.add完成
  elementData = Arrays.copyOf(elementData, newCapacity); 
}

private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow
    throw new OutOfMemoryError();
    return (minCapacity > MAX_ARRAY_SIZE) ?
        Integer.MAX_VALUE :
        MAX_ARRAY_SIZE; 
}

```

由上可知，新容量newCapacity计算公式
**newCapacity = oldCapacity + (oldCapacity >> 1)**  
由此可知若要扩容则会扩容1.5倍

**由于在grow方法中会返回新的对象，当两个线程进行读取时返回的两个新的对象有可能会造成数据覆盖的情况，这也是线程不安全的原因**




***
### **Map集合**
Map接口中的集合都有两个泛型变量&lt;K,V&gt在使用时，要为两个泛型变量赋予数据类型。两个泛型变量&lt;K,V&gt的数据类型可以相同，也可以不同。

#### HashMap


##### **HashMap 的构造函数 **
1. 无参构造函数
```java
static final float DEFAULT_LOAD_FACTOR = 0.75f;//负载因子，默认0.75

static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // 默认容量，默认为16

//无参构造函数，设定负载因子为0.75
public HashMap() {
    this.loadFactor = DEFAULT_LOAD_FACTOR; 
}
```
2. 有一个参数且参数类型为整型的构造函数
```java
//有一个参数的构造函数，本质上是调用有两个参数的构造函数
public HashMap(int initialCapacity) {
    this(initialCapacity, DEFAULT_LOAD_FACTOR); 
}
```
3. 有两个参数的构造函数
```java
//有两个参数的构造函数
public HashMap(int initialCapacity, float loadFactor) {
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " +
                                           initialCapacity);
 if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
 if (loadFactor <= 0 || Float.isNaN(loadFactor))
        throw new IllegalArgumentException("Illegal load factor: " +
                                           loadFactor);
 this.loadFactor = loadFactor;
 this.threshold = tableSizeFor(initialCapacity); 
}
```
4. tableSizeFor方法，用于获取哈希表容量，该容量大于或等于手动输入值并且必为2的幂次方
```java
//该方法会返回一个比给定整数大且最接近2的幂次方的整数
static final int tableSizeFor(int cap) {
    int n = cap - 1;
  n |= n >>> 1;
  n |= n >>> 2;
  n |= n >>> 4;
  n |= n >>> 8;
  n |= n >>> 16;
 return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1; 
}
```
5. 构造函数参数是Map类型的构造函数
```java
//构造函数参数是Map类型的构造函数
public HashMap(Map<? extends K, ? extends V> m) {
    this.loadFactor = DEFAULT_LOAD_FACTOR;
    putMapEntries(m, false); 
}

final void putMapEntries(Map<? extends K, ? extends V> m, boolean evict) {
    int s = m.size();
 if (s > 0) {
 //判断table是否已经初始化，如果未初始化，则先初始化一些变量
        if (table == null) { // pre-size
//根据map的size大小计算出HashMap的容量        
  float ft = ((float)s / loadFactor) + 1.0F;
 int t = ((ft < (float)MAXIMUM_CAPACITY) ?
                     (int)ft : MAXIMUM_CAPACITY);
 if (t > threshold)
 //将计算出的容量放入threshold中
                threshold = tableSizeFor(t);
  }//如果table已经被初始化则判断它的size并和threshold比较
  //如果超出容量则先调用resize方法进行扩容
        else if (s > threshold)
            resize();
 for (Map.Entry<? extends K, ? extends V> e : m.entrySet()) {
            K key = e.getKey();
  V value = e.getValue();
  putVal(hash(key), key, value, false, evict);
  }
    }
}

```
6. **resize方法扩容**  
```java
final Node<K,V>[] resize() {
//保存当前table
    Node<K,V>[] oldTab = table;
    //保存当前table容量
 int oldCap = (oldTab == null) ? 0 : oldTab.length;
 //保存当前阈值
 int oldThr = threshold;
 //初始化新的table容量和阈值
 int newCap, newThr = 0;
 if (oldCap > 0) {
        if (oldCap >= MAXIMUM_CAPACITY) {
        //若原容量超过最大容量，更新阈值
            threshold = Integer.MAX_VALUE;
 return oldTab;
  }
        else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                 oldCap >= DEFAULT_INITIAL_CAPACITY)
 //如果翻倍容量没有超过最大容量且旧容量超过默认容量，则容量增倍
            newThr = oldThr << 1;
  }
    else if (oldThr > 0) 
//当table没初始化时，将阈值赋给新容量
  newCap = oldThr;
 else {               
 // oldCap小于等于零且oldTrd等于0时调用
  newCap = DEFAULT_INITIAL_CAPACITY;
  newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
  }
    if (newThr == 0) {//新阈值为0的情况
        float ft = (float)newCap * loadFactor;
  newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                  (int)ft : Integer.MAX_VALUE);
  }
    threshold = newThr;
  @SuppressWarnings({"rawtypes","unchecked"})
    Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
  table = newTab;
 if (oldTab != null) {
// 把 oldTab 中的节点reHash到newTab中去
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e;
 if ((e = oldTab[j]) != null) {
                oldTab[j] = null;
// 若节点是单个节点，直接在 newTab　中进行重定位
 if (e.next == null)
                    newTab[e.hash & (newCap - 1)] = e;
 else if (e instanceof TreeNode)
 //若结点时TreeNode结点则进行红黑树的rehash
                    ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
//若是链表则进行链表的rehash                    
 else { // preserve order
  Node<K,V> loHead = null, loTail = null;
  Node<K,V> hiHead = null, hiTail = null;
  Node<K,V> next;
 do {
                        next = e.next;
 if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
 else  loTail.next = e;
  loTail = e;
  }
                        else {
                            if (hiTail == null)
                                hiHead = e;
 else  hiTail.next = e;
  hiTail = e;
  }
                    } while ((e = next) != null);
 if (loTail != null) {
                        loTail.next = null;
  newTab[j] = loHead;
  }
                    if (hiTail != null) {
                        hiTail.next = null;
  newTab[j + oldCap] = hiHead;
  }
                }
            }
        }
    }
    return newTab; }

```

当哈希表进行扩容时，由于是使用2次幂的扩展,所以哈希值会在原有的基础上多一bit最高位，当该bit为0时，保持原位置，当该bit为1时，新位置=原位置+oldCap
![reHash](reHash.jpg)

7. putVal方法
```java
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
 boolean evict) {
    Node<K,V>[] tab; Node<K,V> p; int n, i;
 if ((tab = table) == null || (n = tab.length) == 0)
        n = (tab = resize()).length;
 if ((p = tab[i = (n - 1) & hash]) == null)
        tab[i] = newNode(hash, key, value, null);
 else {
        Node<K,V> e; K k;
 if (p.hash == hash &&
            ((k = p.key) == key || (key != null && key.equals(k))))
            e = p;
 else if (p instanceof TreeNode)
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
 else {
            for (int binCount = 0; ; ++binCount) {
                if ((e = p.next) == null) {
                    p.next = newNode(hash, key, value, null);
 if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
  treeifyBin(tab, hash);
 break;  }
                if (e.hash == hash &&
                    ((k = e.key) == key || (key != null && key.equals(k))))
                    break;
  p = e;
  }
        }
        if (e != null) { // existing mapping for key
  V oldValue = e.value;
 if (!onlyIfAbsent || oldValue == null)
                e.value = value;
  afterNodeAccess(e);
 return oldValue;
  }
    }
    ++modCount;
 if (++size > threshold)
        resize();
  afterNodeInsertion(evict);
 return null; }

```


##### **HashMap的纵向延申方式 **  
* JDK1.7以前使用的是头插法，容易因为多线程使用resize方法出现逆序且环形链表系循环问题。
* JDK1.8以后因为加入了红黑树使用尾插法，一定程度解决了这些问题，但多线程下操作同一对象时，对象内部属性的不一致性仍可能导致死循环。

| JDK版本 | 实现方式 | 节点数>=8 | 节点数<=6 |
| --- | --- | --- | --- |
| 1.8以前 | 数组+单向链表 | 数组+单向链表 | 数组+单向链表 |
| 1.8以后 | 数组+单向链表+红黑树 | 数组+红黑树 | 数组+单向链表 |


#### TreeMap
能够把它保存的记录根据键(key)排序,默认是按升序排序，也可以指定排序的比较器，当用Iterator 遍历TreeMap时，得到的记录是排过序的。TreeMap不允许key的值为null。（**非同步**）



#### ConcurrentHashMap

ConcurrentHashMap是Java中的一个**线程安全且高效的HashMap实现**。高并发的优质map结构。

参考资料：
1. [https://www.jianshu.com/u/b3381a075b62](https://www.jianshu.com/u/b3381a075b62)
2. [https://www.jianshu.com/p/ee0de4c99f87](https://www.jianshu.com/p/ee0de4c99f87)
3. [https://www.cnblogs.com/IT-CPC/](https://www.cnblogs.com/IT-CPC/)
