

# 一、概览

容器主要包括 Collection 和 Map 两种，Collection 存储着对象的集合，而 Map 存储着键值对（两个对象）的映射表。

## Collection

### 1.List

- ArrayList：基于动态数组实现，支持随机访问

- LinkedList：基于双向链表实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。

  ​						不仅如此，LinkedList 还可以用作栈、队列和双向队列。

- Vector：和 ArrayList 类似，但它是线程安全的。

- Stack：

### 2.Set

- HashSet：基于哈希表实现，支持快速查找，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用 Iterator 遍历 HashSet 得到的结果是不确定的。
- LinkedHashSet：具有 HashSet 的查找效率，并且内部使用双向链表维护元素的插入顺序。
- TreeSet：基于红黑树实现，支持有序性操作，例如根据一个范围查找元素的操作。但是查找效率不如 HashSet，HashSet 查找的时间复杂度为 O(1)，TreeSet 则为 O(logN)。

### 3. Queue

- LinkedList：可以用它来实现双向队列。

- PriorityQueue：基于堆结构实现，可以用它来实现优先队列。

## Map

- HashMap：基于哈希表实现
  - LindedHashMap：使用双向链表来维护元素的顺序，顺序为插入顺序或者最近最少使用（LRU）顺序
- TreeMap：基于红黑树实现
- ConcurrentHashMap：线程安全，效率更高，因为 ConcurrentHashMap 引入了分段锁。
- Hashtable：和 HashMap 类似，但它是线程安全的，这意味着同一时刻多个线程同时写入 HashTable 不会导致数据不一致

## 



# 二、分析

## List

### ArrayList

​	概念：因为 ArrayList 是基于数组实现的，所以支持快速随机访问。

​	大小：数组的默认大小为 10

​	安全：线程不安全

​	扩容：使用 grow() 方法进行扩容，新容量的大小为 旧容量的 1.5 倍。扩容操作需要调用 `Arrays.copyOf()` ，				操作代价很高，最好在创建时就指定大概的容量大小，减少扩容操作的次数。

​	删除：调用 System.arraycopy() 将 index+1 后面的元素都复制到 index 位置上，删除元素的代价是非常高



### Vector

​	概念：实现与 ArrayList 类似，但是使用了 synchronized 进行同步。

​	大小：数组的默认大小为 10

​	安全：线程安全

​	扩容：默认情况下每次扩容为原来的两倍，若指定参数就增长指定大小

​	删除：同ArrayList 类似



### LinkedList

​	概念：基于双向链表实现，使用 Node 存储链表节点信息。每个链表存储了 first 和 last 指针

​	大小：

​	安全：线程不安全

​	扩容：每次增加一个节点

​	删除：遍历链表，删除指定节点



### CopyOnWriteArrayList

​	概念：写操作在一个复制的数组上进行，读操作还是在原始数组中进行，读写分离，互不影响。

​				写操作需要加锁，防止并发写入时导致写入数据丢失。

​				写操作结束之后需要把原始数组指向新的复制数组。

​	适用场景：适合读多写少的应用场景。

​	缺陷：

​		内存占用：在写操作时需要复制一个新的数组，使得内存占用为原来的两倍左右；

​		数据不一致：读操作不能读取实时性的数据，因为部分写操作的数据还未同步到读数组中。

​		不适合内存敏感以及对实时性要求很高的场景。



### 比较

#### ArrayList与Vector

- Vector 是同步的，因此开销就比 ArrayList 要大，访问速度更慢。最好使用 ArrayList 而不是 Vector
- Vector 每次扩容请求其大小的 2 倍（也可以通过构造函数设置增长的容量），而 ArrayList 是 1.5 倍

#### ArrayList与LinkedList

​	可以归结为数组和链表的区别：

- 数组支持随机访问，但插入删除的代价很高，需要移动大量元素；
- 链表不支持随机访问，但插入删除只需要改变指针。

### 线程安全的List

​	Collections.synchronizedList();

​	Vector

​	CopyOnWriteArrayList



## Set

### HashSet

​	概念：底层实现是一个HashMap（保存数据，把value值写死了），实现Set接口，

​	大小：默认初始容量为16，加载因子为0.75

​	安全：线程不安全，存取速度快

​	扩容：每次增加一个节点

​	删除：遍历链表，删除指定节点

### CopyOnWriteArraySet



### 线程安全的Set

​		Collections.synchronizedSet();

​		CopyOnWriteArraySet

## Map

### HashMap

​	概念：底层是一个Entry 类型的数组 。Entry 存储着键值对，每个元素可以看成链表，使用拉链法来解决冲突

​				数组+链表+红黑树(一个桶存储的链表长度大于等于 8 时会将链表转换为红黑树)

​	大小：默认初始容量为16（2的幂次）

​	安全：线程不安全

​	扩容：每次扩容请求其大小的 2 倍

​	删除：确定桶下标，遍历链表，删除指定节点 

#### 	拉链法

​		应该注意到链表的插入是以头插法方式进行的

​		查找需要分成两步进行：计算键值对所在的桶；在链表上顺序查找，时间复杂度显然和链表的长度成正比。

#### 	put 操作

​		键为 null 单独处理。HashMap 允许插入键为 null 的键值对，通过强制指定第 0 个桶下标来存放。

​		确定桶下标。计算 hash 值，取模

​		先找出是否已经存在键为 key 的键值对，如果存在的话就更新这个键值对的值为 value，否则插入新键值对

#### 	扩容

​		设 HashMap 的 table 长度为 M，存储的键值对数量为 N，每条链表的长度大约为 N/M，查找的复杂度为 O(N/M)。为了让查找的成本降低，应该使 N/M 尽可能小，因此需要保证 M 尽可能大。

​		HashMap 采用动态扩容来根据当前的 N 值来调整 M 值，扩容相关的参数如下

|    参数    | 含义                                                         |
| :--------: | :----------------------------------------------------------- |
|  capacity  | table 的容量大小，默认为 16。需要注意的是 capacity 必须保证为 2 的 n 次方。 |
|    size    | 键值对数量。                                                 |
| threshold  | size 的临界值，当 size 大于等于 threshold 就必须进行扩容操作。 |
| loadFactor | 装载因子，table 能够使用的比例，threshold = (int)(capacity* loadFactor)。0.75 |

​		HashMap扩容同ArrayList一样很操作代价很高，不仅是容器的扩充，还需要把 oldTable 的所有键值对重新插入 newTable 中

​		扩容后重新计算桶下标

```html
capacity     : 00010000	--16
new capacity : 00100000	--32
```

对于一个 Key，它的哈希值 hash 在第 5 位：

- 为 0，那么 hash%00010000 = hash%00100000，桶位置和原来一致

- 为 1，hash%00010000 = hash%00100000 + 16，桶位置是原位置 + 16

  

### ConcurrentHashMap

​	概念：与 HashMap 底层实现类似， 采用了分段锁（Segment），每个分段锁维护着几个桶（HashEntry），				多个线程可以同时访问不同分段锁上的桶，从而使其并发度更高（并发度就是 Segment 的个数）

​	大小：(并发度)16，也就是说默认创建 16 个 Segment。

​	安全：线程安全

​	扩容：

​	删除：同HashMap类似

#### 	size 操作

​		每个 Segment 维护了一个 count 变量来统计该 Segment 中的键值对个数。

​		在执行 size 操作时，需要遍历所有 Segment 然后把 count 累计起来。

​		ConcurrentHashMap 在执行 size 操作时先尝试不加锁，如果连续两次不加锁操作得到的结果一致，那么可以认为这个结果是正确的。

​		尝试次数使用 RETRIES_BEFORE_LOCK 定义，该值为 2，retries 初始值为 -1，因此尝试次数为 3。

​		如果尝试的次数超过 3 次，就需要对每个 Segment 加锁。



### LinkedHashMap

#### 存储结构

继承自 HashMap，因此具有和 HashMap 一样的快速查找特性。

内部维护了一个双向链表，用来维护插入顺序或者 LRU 顺序。

accessOrder 决定了顺序，默认为 false，此时维护的是插入顺序。

LinkedHashMap 最重要的是以下用于维护顺序的函数，它们会在 put、get 等方法中调用。

#### afterNodeAccess()

当一个节点被访问时，如果 accessOrder 为 true，则会将该节点移到链表尾部。也就是说指定为 LRU 顺序之后，在每次访问一个节点时，会将这个节点移到链表尾部，保证链表尾部是最近访问的节点，那么链表首部就是最近最久未使用的节点。

#### afterNodeInsertion()

在 put 等操作之后执行，当 removeEldestEntry() 方法返回 true 时会移除最晚的节点，也就是链表首部节点 first。

evict 只有在构建 Map 的时候才为 false，在这里为 true。

```java
void afterNodeInsertion(boolean evict) { // possibly remove eldest
    LinkedHashMap.Entry<K,V> first;
    if (evict && (first = head) != null && removeEldestEntry(first)) {
        K key = first.key;
        removeNode(hash(key), key, null, false, true);
    }
}
```

removeEldestEntry() 默认为 false，如果需要让它为 true，需要继承 LinkedHashMap 并且覆盖这个方法的实现，这在实现 LRU 的缓存中特别有用，通过移除最近最久未使用的节点，从而保证缓存空间足够，并且缓存的数据都是热点数据。

```java
protected boolean removeEldestEntry(Map.Entry<K,V> eldest) {
    return false;
}
```

#### LRU 缓存

以下是使用 LinkedHashMap 实现的一个 LRU 缓存：

- 设定最大缓存空间 MAX_ENTRIES  为 3；
- 使用 LinkedHashMap 的构造函数将 accessOrder 设置为 true，开启 LRU 顺序；
- 覆盖 removeEldestEntry() 方法实现，在节点多于 MAX_ENTRIES 就会将最近最久未使用的数据移除。

```java
class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private static final int MAX_ENTRIES = 3;

    protected boolean removeEldestEntry(Map.Entry eldest) {
        return size() > MAX_ENTRIES;
    }

    LRUCache() {
        super(MAX_ENTRIES, 0.75f, true);
    }
}
```

```java
public static void main(String[] args) {
    LRUCache<Integer, String> cache = new LRUCache<>();
    cache.put(1, "a");
    cache.put(2, "b");
    cache.put(3, "c");
    cache.get(1);
    cache.put(4, "d");
    System.out.println(cache.keySet());
}
```

```html
[3, 1, 4]
```

### 比较

#### 	HashMap与 Hashtable 

- Hashtable 使用 synchronized 来进行同步。
- HashMap 可以插入键为 null 的 Entry。
- HashMap 的迭代器是 fail-fast 迭代器。
- HashMap 不能保证随着时间的推移 Map 中的元素次序是不变的。

####     HashMap与ConcurrentHashMap



### 线程安全的Map

​		Collections.synchronizedMap();

​		ConcurrentHashMap



# 三、面试题

#### 1.Collection 和 Collections 区别？

- Collection 是一个集合接口，提供了对集合对象进行基本操作的通用接口方法，所有集合都是它的子类，比如 List、Set 等。
- Collections 是一个包装类，包含了很多静态方法，不能被实例化，就像一个工具类，比如提供的排序方法： Collections. sort(list)

#### **2.List、Set、Map 之间的区别是什么？**

List、Set、Map 的区别主要体现在两个方面：元素是否有序、是否允许元素重复。

具体可以参考这篇文章：https://www.cnblogs.com/IvesHe/p/6108933.html

#### 3.HashMap 和 Hashtable 有什么区别？

- 存储：HashMap 运行 key 和 value 为 null，而 Hashtable 不允许。
- 线程安全：Hashtable 是线程安全的，而 HashMap 是非线程安全的。
- 推荐使用：在 Hashtable 的类注释可以看到，Hashtable 是保留类不建议使用，推荐在单线程环境下使用 HashMap 替代，如果需要多线程使用则用 ConcurrentHashMap 替代。

#### **4.如何决定使用 HashMap 还是 TreeMap？**

对于在 Map 中插入、删除、定位一个元素这类操作，HashMap 是最好的选择，因为相对而言 HashMap 的插入会更快，但如果你要对一个 key 集合进行有序的遍历，那 TreeMap 是更好的选择

#### 5.说一下 HashMap 的实现原理？

HashMap 基于 Hash 算法实现的，我们通过 put(key,value)存储，get(key)来获取。当传入 key 时，HashMap 会根据 key. hashCode() 计算出 hash 值，根据 hash 值将 value 保存在 bucket 里。当计算出的 hash 值相同时，我们称之为 hash 冲突，HashMap 的做法是用链表和红黑树存储相同 hash 值的 value。当 hash 冲突的个数比较少时，使用链表否则使用红黑树。

#### 6.说一下 HashSet 的实现原理？

HashSet 是基于 HashMap 实现的，HashSet 底层使用 HashMap 来保存所有元素，因此 HashSet 的实现比较简单，相关 HashSet 的操作，基本上都是直接调用底层 HashMap 的相关方法来完成，HashSet 不允许重复的值。

#### 7.ArrayList 和 LinkedList 的区别是什么？

- 数据结构实现：ArrayList 是动态数组的数据结构实现，而 LinkedList 是双向链表的数据结构实现。
- 随机访问效率：ArrayList 比 LinkedList 在随机访问的时候效率要高，因为 LinkedList 是线性的数据存储方式，所以需要移动指针从前往后依次查找。
- 增加和删除效率：在非首尾的增加和删除操作，LinkedList 要比 ArrayList 效率要高，因为 ArrayList 增删操作要影响数组内的其他数据的下标。

综合来说，在需要频繁读取集合中的元素时，更推荐使用 ArrayList，而在插入和删除操作较多时，更推荐使用 LinkedList。

#### 8.如何实现数组和 List 之间的转换？

- 数组转 List：使用 Arrays. asList(array) 进行转换。
- List 转数组：使用 List 自带的 toArray() 方法。

【代码示例】

```cs
// list to arrayList<String> list = new ArrayList<String>();list. add("王磊");list. add("的博客");list. toArray();// array to listString[] array = new String[]{"王磊","的博客"};Arrays. asList(array);
```

#### 9.ArrayList 和 Vector 的区别是什么？

- 线程安全：Vector 使用了 Synchronized 来实现线程同步，是线程安全的，而 ArrayList 是非线程安全的。
- 性能：ArrayList 在性能方面要优于 Vector。
- 扩容：ArrayList 和 Vector 都会根据实际的需要动态的调整容量，只不过在 Vector 扩容每次会增加 1 倍，而 ArrayList 只会增加 50%。

#### **10.Array 和 ArrayList 有何区别？**

- Array 可以存储基本数据类型和对象，ArrayList 只能存储对象。
- Array 是指定固定大小的，而 ArrayList 大小是自动扩展的。
- Array 内置方法没有 ArrayList 多，比如 addAll、removeAll、iteration 等方法只有 ArrayList 有。

#### **11.在 Queue 中 poll()和 remove()有什么区别？**

- 相同点：都是返回第一个元素，并在队列中删除返回的对象。
- 不同点：如果没有元素 poll()会返回 null，而 remove()会直接抛出 NoSuchElementException 异常。

【代码示例】

```cs
Queue<String> queue = new LinkedList<String>();queue. offer("string"); // addSystem. out. println(queue. poll());System. out. println(queue. remove());System. out. println(queue. size());
```

#### **12.哪些集合类是线程安全的？**

Vector、Hashtable、Stack 都是线程安全的，而像 HashMap 则是非线程安全的，不过在 JDK 1.5 之后随着 Java. util. concurrent 并发包的出现，它们也有了自己对应的线程安全类，比如 HashMap 对应的线程安全类就是 ConcurrentHashMap。

#### **13.迭代器 Iterator 是什么？**

Iterator 接口提供遍历任何 Collection 的接口。我们可以从一个 Collection 中使用迭代器方法来获取迭代器实例。迭代器取代了 Java 集合框架中的 Enumeration，迭代器允许调用者在迭代过程中移除元素。

#### **14.Iterator 怎么使用？有什么特点？**

【代码示例】

```cs
List<String> list = new ArrayList<>();Iterator<String> it = list. iterator();while(it. hasNext()){  String obj = it. next();  System. out. println(obj);}
```

Iterator 的特点是更加安全，因为它可以确保，在当前遍历的集合元素被更改的时候，就会抛出 ConcurrentModificationException 异常。

#### **15.Iterator 和 ListIterator 有什么区别？**

- Iterator 可以遍历 Set 和 List 集合，而 ListIterator 只能遍历 List。
- Iterator 只能单向遍历，而 ListIterator 可以双向遍历（向前/后遍历）。
- ListIterator 从 Iterator 接口继承，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置。

#### 16.怎么确保一个集合不能被修改？

可以使用 Collections. unmodifiableCollection(Collection c) 方法来创建一个只读集合，这样改变集合的任何操作都会抛出 Java. lang. UnsupportedOperationException 异常。

【代码示例】

```cs
List<String> list = new ArrayList<>();list. add("x");Collection<String> clist = Collections. unmodifiableCollection(list);clist. add("y"); // 运行时此行报错System. out. println(list. size());
```

 

