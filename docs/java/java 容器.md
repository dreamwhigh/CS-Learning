[TOC]

## 源码分析

[**如何查看源码**](<https://www.cnblogs.com/lxmyhappy/p/7084097.html>)

在 Eclipse 中，选中类名或方法名，按 F3 ，就可以查看对应的源码。

### ArrayList

基于动态数组实现，所以支持快速随机访问。RandomAccess 接口标识着该类支持快速随机访问。

实现了 `java.io.Serializable` 这个接口，能够被序列化。

```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```

#### 初始容量

参考[ArrayList 扩容机制](<https://www.imooc.com/article/278549>)

```java
//ArrayList 源码里关于元素存放的几个私有属性

// 默认容量是10
private static final int DEFAULT_CAPACITY = 10;

// 如果容量为0的时候，就返回这个数组，该数组容量为 0
private static final Object[] EMPTY_ELEMENTDATA = {};

// 使用默认容量10时，返回这个数组，该数组容量为 10
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

// 元素存放的数组
transient Object[] elementData;

// 当前存放的元素个数
private int size;

// 记录被修改的次数
protected transient int modCount = 0;

// 数组的最大值
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8
```

#### 扩容

参考[ArrayList 扩容机制](<https://www.jianshu.com/p/14d5f89d46a3>)

```java
/**
	*在 ArrayList 尾部加入新元素 e
	*size 表示此 ArrayList 实际存储数据元素的个数
	*/
public boolean add(E e) {
    //调用 ensureCapacityInternal()
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    elementData[size++] = e;
    return true;
}

private void ensureCapacityInternal(int minCapacity) {
    //先调用 calculateCapacity(elementData, minCapacity)
    //再调用 ensureExplicitCapacity()
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

/**
	*elementData 是 ArrayList 用于存储元素的数组
	*minCapacity 代表 elementData 数组的新长度(原数组长度+要增加的数组长度)
	*此时 minCapacity = size + 1
	*calculateCapacity() 判断 ArrayList 默认的元素存储数组是否为空
	*返回数组实际的新长度
	*/
private static int calculateCapacity(Object[] elementData, int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }//数组为空，返回两者中的最大值
    return minCapacity;//若不为空，返回当前数组的新 size
}

//默认大小的空数组实例
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

//elementData 数组的默认长度
private static final int DEFAULT_CAPACITY = 10;

//
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;
    
    // 如果最小需要空间比 elementData 的内存空间要大，则需要扩容
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}

private void grow(int minCapacity) {
    int oldCapacity = elementData.length;//获取 elementData 数组的内存空间长度
    int newCapacity = oldCapacity + (oldCapacity >> 1);  //扩容至数组原长度的 1.5 倍
    //再判断一下新数组的容量是否大于期望容量
    //新容量依然小于期望容量，则返回期望容量
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    //判断新容量是否越界，若预设值大于默认的最大值检查是否溢出
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    //将原数组元素复制到扩容后的数组中
    elementData = Arrays.copyOf(elementData, newCapacity);
}

private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow 溢出
        throw new OutOfMemoryError();
    //若期望容量大于预设值 MAX_ARRAY_SIZE，则返回 Integer.MAX_VALUE
    //否则返回预设值
    return (minCapacity > MAX_ARRAY_SIZE) ?
        Integer.MAX_VALUE :
        MAX_ARRAY_SIZE;
}
```

调用过程如下：

<div align="center">
    <img src="pics/ArrayList 扩容机制.png" width="400px">
</div>

#### 删除元素

[ArrayList 提供了 四个方法进行元素的删除](<http://wiki.jikexueyuan.com/project/java-enhancement/java-twentyone.html>)

- `remove(int index)` 移除此列表中指定位置上的元素
- `remove(Object o)` 移除此列表中首次出现的指定元素（如果存在）
- `removeRange(int fromIndex, int toIndex)` 移除列表中索引在 fromIndex（包括）和 toIndex（不包括）之间的所有元素
- `removeAll()` 是继承自 AbstractCollection 的方法，ArrayList 本身并没有提供实现。

```java
public E remove(int index) {
    //位置验证
    rangeCheck(index);
    modCount++;
    //需要删除的元素
    E oldValue = (E) elementData[index];   
    //需要向左移的位数
    int numMoved = size - index - 1;
    //若需要移动，则想左移动numMoved位
    if (numMoved > 0)
        System.arraycopy(elementData, index + 1, elementData, index,
                numMoved);
    //置空最后一个元素
    elementData[--size] = null; // Let gc do its work
    return oldValue;
}
```

`remove(int index)` 需要调用 `System.arraycopy()` 将 index+1 后面的元素都复制到 index 位置上，该操作的时间复杂度为 O(N)，可以看出 ArrayList 删除元素的代价是非常高的。

#### 查找

[`get(int index)` 用于读取 ArrayList 中的元素。](<http://wiki.jikexueyuan.com/project/java-enhancement/java-twentyone.html>)

由于 ArrayList 是动态数组，支持随机访问。所以我们直接根据下标来获取 ArrayList 中的元素，且速度较快。

```java
public E get(int index) {
    RangeCheck(index);
    return (E) elementData[index];//直接通过下标索引
}
```

#### Fail - Fast

**fail-fast 机制是 java 集合(Collection)中的一种错误机制。**当多个线程对同一个集合的内容进行操作时，就可能会产生fail-fast事件。

**结构变化**：指添加或者删除至少一个元素的所有操作，或者是调整内部数组的大小，仅仅只是设置元素的值不算结构发生变化。

`modCount`：记录 ArrayList 结构发生变化的次数。

在进行序列化或者迭代等操作时，需要比较操作前后 `modCount` 是否改变，如果改变了需要抛出 `ConcurrentModificationException` 。

```java
private void writeObject(java.io.ObjectOutputStream s)
    throws java.io.IOException{
    // Write out element count, and any hidden stuff
    int expectedModCount = modCount;//保存序列化之前的 modCount，防止序列化期间有修改
    // 写出非transient非static属性（会读取size属性）
    s.defaultWriteObject();
    // Write out size as capacity for behavioural compatibility with clone()
    //写出元素个数
    s.writeInt(size);
    // Write out all elements in the proper order.
    //依次写出元素
    for (int i=0; i<size; i++) {
        s.writeObject(elementData[i]);
    }
    if (modCount != expectedModCount) {
        throw new ConcurrentModificationException();
    }//前后 modCount 不等，抛出异常
}
```

#### 序列化

`java.io.Serializable 接口` ：实现该接口的类是可序列化的，这个类的所有属性和方法都会自动序列化。

`transient` ：将不需要序列化的属性前添加关键字 `transient` ，序列化对象的时候，这个属性就不会被序列化。

```java
transient Object[] elementData;
```

ArrayList 的数据存储都是依赖于被  `transient` 修饰的 elementData 数组，该关键字声明数组默认不会被序列化。

[ArrayList 序列化分析](<https://yq.aliyun.com/articles/590588>)

当声明一个数组时长度是固定的，在 ArrayList 中遇到元素个数超过数组大小时会采用自动扩展的方式来加大数组容量，而扩展方法是在原有数组容量的基础上扩大到 1.5 倍。这种扩展方法会带来底层数组会存在空值问题，也就是真实数组容量远大于实际元素个数，在这样的前提下，如果直接序列 `elementData` 数组会带来不必要的开销，序列化很多空值，从而降低了序列化、反序列化性能。

ArrayList 通过 `writeObject()` 和 `readObject()` 来控制只序列化数组中有元素填充那部分内容，从而提升性能。

```java
private void readObject(java.io.ObjectInputStream s)
    throws java.io.IOException, ClassNotFoundException {
    // 声明为空数组
    elementData = EMPTY_ELEMENTDATA;
    // Read in size, and any hidden stuff
    // 读入非transient非static属性（会读取size属性）
    s.defaultReadObject();
    // Read in capacity
    // 读入元素个数，没有作用可忽略，只是因为写出的时候写了 size 属性，读的时候也要按顺序来读
    s.readInt(); // ignored
    if (size > 0) {
        // be like clone(), allocate array based upon size not capacity
        ensureCapacityInternal(size);// 检查是否需要扩容 
        Object[] a = elementData;
        // Read in all elements in the proper order.
        // 依次读取元素到数组中
        for (int i=0; i<size; i++) {
            a[i] = s.readObject();
        }
    }
}
```

序列化时需要使用 ObjectOutputStream 的 writeObject() 将对象转换为字节流并输出。而 writeObject() 方法在传入的对象存在 writeObject() 的时候会去反射调用该对象的 writeObject() 来实现序列化。反序列化使用的是 ObjectInputStream 的 readObject() 方法，原理类似。

```java
ArrayList list = new ArrayList();
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(file));
oos.writeObject(list);

```

### Vector

#### 同步

Vector 的实现与 ArrayList 类似，但是使用了 synchronized 进行同步。

线程的同步，即某一时刻只有一个线程能够写Vector，避免多线程同时写而引起的不一致性，但实现同步需要很高的花费，因此，访问它比访问ArrayList慢。

```java
//末尾插入元素
public synchronized boolean add(E e) {
    modCount++;
    ensureCapacityHelper(elementCount + 1);
    elementData[elementCount++] = e;
    return true;
}
//查找指定位置元素，数组下标索引即可
public synchronized E get(int index) {
    if (index >= elementCount)
        throw new ArrayIndexOutOfBoundsException(index);
        
    return elementData(index);
}
```

#### 与 ArrayList 的比较

- Vector 是同步的，因此开销就比 ArrayList 要大，访问速度更慢。最好使用 ArrayList 而不是 Vector，因为同步操作完全可以由程序员自己来控制；
- Vector 每次扩容请求其大小的 2 倍空间，而 ArrayList 是 1.5 倍；
- Vector提供 `indexOf(obj, start)` 接口，ArrayList 没有。

#### 线程安全的 ArrayList

1. 使用 `Collections.synchronizedList();` 得到一个线程安全的 ArrayList。

```
List<String> list = new ArrayList<>();
List<String> synList = Collections.synchronizedList(list);
```

2. 使用 concurrent 并发包下的 [`CopyOnWriteArrayList 类`](<https://blog.csdn.net/linsongbin1/article/details/54581787>)。

```
List<String> list = new CopyOnWriteArrayList<>();
```

### CopyOnWrite

#### CopyOnWrite 容器

[并发容器之CopyOnWriteArrayList](<https://www.cnblogs.com/dolphin0520/p/3938914.html>)

CopyOnWrite 容器即写时复制的容器。通俗的理解是当我们往一个容器添加元素的时候，不直接往当前容器添加，而是先将当前容器进行 Copy，复制出一个新的容器，然后新的容器里添加元素，添加完元素之后，再将原容器的引用指向新的容器。这样做的好处是我们可以对 CopyOnWrite 容器进行并发的读，而不需要加锁，因为当前容器不会添加任何元素。所以 CopyOnWrite 容器也是一种读写分离的思想，读和写不同的容器。

##### 缺点

- **内存占用问题**：因为 CopyOnWrite 的写时复制机制，所以在进行写操作的时候，内存里会同时驻扎两个对象的内存，旧的对象和新写入的对象（注意:在复制的时候只是复制容器里的引用，只是在写的时候会创建新对象添加到新容器里，而旧容器的对象还在使用，所以有两份对象内存）。
- **数据一致性问题**：CopyOnWrite 容器只能保证数据的最终一致性，不能保证数据的实时一致性。

#### CopyOnWriteArrayList

写操作在一个复制的数组上进行，读操作还是在原始数组中进行，读写分离，互不影响。

写操作需要加锁，防止并发写入时导致写入数据丢失。

写操作结束之后需要把原始数组指向新的复制数组。

```java
//写入
public boolean add(E e) {
    final ReentrantLock lock = this.lock;
    lock.lock(); //上锁，只允许一个线程进入
    try {
        Object[] elements = getArray(); // 获得当前数组对象
        int len = elements.length;
        Object[] newElements = Arrays.copyOf(elements, len + 1);//拷贝到一个新的数组中
        newElements[len] = e;//插入数据元素到新数组中
        setArray(newElements);//将 array 引用指向到新数组
        return true;
    } finally {
        lock.unlock();//释放锁
    }
}

final void setArray(Object[] a) {
    array = a;
}
```

```java
//读出
@SuppressWarnings("unchecked")
private E get(Object[] a, int index) {
    return (E) a[index];
}
```

#### 试用场景

CopyOnWriteArrayList 在写操作的同时允许读操作，大大提高了读操作的性能，因此很适合读多写少的应用场景。

但由于其内存占用和数据不一致的缺点，不适合内存敏感以及对实时性要求很高的场景。

### LinkedList

#### 概述

LinkedList 是一个继承于 `AbstractSequentialList` 的双向链表。它也可以被当作堆栈、队列或双端队列进行操作。是非同步的。

- 实现 `List` 接口，能对它进行队列操作；
- 实现 `Deque` 接口，即能将 LinkedList 当作双端队列使用；
- 实现 `Cloneable` 接口，即覆盖了函数 `clone()`，能克隆；
- 实现`java.io.Serializable` 接口，这意味着 LinkedList 支持序列化，能通过序列化去传输。

```java
public class LinkedList<E>
extends AbstractSequentialList<E>
implements List<E>, Deque<E>, Cloneable, Serializable
```

基于双向链表实现，使用 Node 存储链表节点信息。

```java
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;
}
```

`LinkedList` 包含三个重要的成员 `size` , `first` ,`last` ：

```java
//双向链表中节点的个数
transient int size = 0;

/**
 * Pointer to first node.
 */
transient Node<E> first;

/**
 * Pointer to last node.
 */
transient Node<E> last;
```

<div align="center">
    <img src="pics/linkedList 结构.png" width="400px">
</div>

#### 与 ArrayList 的比较

- ArrayList 基于动态数组实现，LinkedList 基于双向链表实现；
- ArrayList 支持随机访问，LinkedList 不支持；
- LinkedList 在任意位置添加删除元素更快。

### HashMap

#### 存储结构

HashMap 是采用了拉链法来解决冲突，也就是**数组+链表**的方式。其主干是一个Entry 数组。Entry 是 HashMap 的基本组成单元，每一个 Entry 包含一个 key-value 键值对，hash 值，指向下一个 Entry 的引用。

```java
static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;
    V value;
    Node<K,V> next;//存储指向下一个Entry的引用，单链表结构

    //Entry 存储着键值对 K,V 
    //next 表明 Entry 是一个链表
    Node(int hash, K key, V value, Node<K,V> next) {
        this.hash = hash;
        this.key = key;
        this.value = value;
        this.next = next;
    }

    public final K getKey()        { return key; }
    public final V getValue()      { return value; }
    public final String toString() { return key + "=" + value; }

    public final int hashCode() {
        return Objects.hashCode(key) ^ Objects.hashCode(value);
    }

    public final V setValue(V newValue) {
        V oldValue = value;
        value = newValue;
        return oldValue;
    }

    public final boolean equals(Object o) {
        if (o == this)
            return true;
        if (o instanceof Map.Entry) {
            Map.Entry<?,?> e = (Map.Entry<?,?>)o;
            if (Objects.equals(key, e.getKey()) &&
                Objects.equals(value, e.getValue()))
                return true;
        }
        return false;
    }
}
```

<div align="center">
    <img src="pics/hashmap.png" width="400px">
</div>

#### 拉链法的工作原理

链表的插入是以**头插法**方式进行

```java
HashMap<String, String> map = new HashMap<>();
map.put("K1", "V1");
map.put("K2", "V2");
map.put("K3", "V3");
```

- 新建一个 HashMap，默认大小为 16；
- 插入 <K1,V1> 键值对，先计算 K1 的 hashCode 为 115，使用除留余数法得到所在的桶下标 115%16=3。
- 插入 <K2,V2> 键值对，先计算 K2 的 hashCode 为 118，使用除留余数法得到所在的桶下标 118%16=6。
- 插入 <K3,V3> 键值对，先计算 K3 的 hashCode 为 118，使用除留余数法得到所在的桶下标 118%16=6，插在 <K2,V2> 前面。

<div align="center">
    <img src="pics/拉链法.png" width="400px">
</div>

#### 查找

- 计算键值对所在的桶；
- 在链表上顺序查找，时间复杂度显然和链表的长度成正比。

#### put 操作

```java
//HashMap 的 put() 方法
public V put(K key, V value) {
    if (table == EMPTY_TABLE) {
        inflateTable(threshold);
    }
    // 键为 null 单独处理
    if (key == null)
        return putForNullKey(value);
    //根据 key 计算出 hashcode
    int hash = hash(key);
    // 根据 hash 值定位出所在桶下标
    int i = indexFor(hash, table.length);
    //如果桶是一个链表（e != null）则需要遍历判断里面的 hashcode、key 是否和传入 key 相等，如果相等则进行覆盖，并返回原来的值
    for (Entry<K,V> e = table[i]; e != null; e = e.next) {
        Object k;
        if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }
	//如果不存在相等的键值，或者桶为空，则 modCount++，并且插入新键值对
    modCount++;
    addEntry(hash, key, value, i);
    return null;
}

```

`putForNullKey(V value)`

HashMap 允许插入键为 null 的键值对。但是因为无法调用 null 的 hashCode() 方法，也就无法确定该键值对的桶下标，只能通过强制指定一个桶下标来存放。HashMap 使用第 0 个桶存放键为 null 的键值对。

```java
//键为 null 单独处理
private V putForNullKey(V value) {
    for (Entry<K,V> e = table[0]; e != null; e = e.next) {
        if (e.key == null) {
            V oldValue = e.value;
            e.value = value;
            e.recordAccess(this);
            return oldValue;
        }
    }
    modCount++;
    //强制指定第 0 个桶存放键为 null 的键值对
    addEntry(0, null, value, 0);
    return null;
}
```

`addEntry(int hash, K key, V value, int bucketIndex)` ，使用链表的头插法。

```java
//将新的键值对插在链表的头部
void addEntry(int hash, K key, V value, int bucketIndex) {
    //判断是否需要扩容
    if ((size >= threshold) && (null != table[bucketIndex])) {
        //进行两倍扩充
        resize(2 * table.length);
        //重新计算当前 key 的 hash 值
        //key 为 null 时，强制桶下标为 0
        hash = (null != key) ? hash(key) : 0;
        //确定桶下标
        bucketIndex = indexFor(hash, table.length);
    }
    //插入新的键值对
    createEntry(hash, key, value, bucketIndex);
}

void createEntry(int hash, K key, V value, int bucketIndex) {
    Entry<K,V> e = table[bucketIndex];
    // 头插法，链表头部指向新的键值对
    table[bucketIndex] = new Entry<>(hash, key, value, e);
    size++;
}
```

```java
Entry(int h, K k, V v, Entry<K,V> n) {
    value = v;
    next = n;
    key = k;
    hash = h;
}
```

#### 确定桶下标

hash ：该方法主要是将Object转换成一个整型。

indexFor ：该方法主要是将hash生成的整型转换成链表数组中的下标。

```java
//根据 key 计算出 hashcode
int hash = hash(key);
// 根据 hash 值定位出所在桶下标
int i = indexFor(hash, table.length);
```

[`hash(Object k)`](<https://blog.csdn.net/majinggogogo/article/details/80260400>)

对 key 的 hashCode 进行**扰动计算**，防止不同 hashCode 的高位不同但低位相同导致的hash冲突。简单点说，就是为了**把高位的特征和低位的特征组合起来，降低哈希冲突的概率**，也就是说，尽量做到任何一位的变化都能对最终得到的结果产生影响。

```java
//In Java 7
final int hash(Object k) {
    int h = hashSeed;
    if (0 != h && k instanceof String) {
        return sun.misc.Hashing.stringHash32((String) k);
    }
    h ^= k.hashCode();
    //扰动计算
    h ^= (h >>> 20) ^ (h >>> 12);
    return h ^ (h >>> 7) ^ (h >>> 4);
}

public final int hashCode() {
    return Objects.hashCode(key) ^ Objects.hashCode(value);
}

public static int hashCode(Object o) {
    return o != null ? o.hashCode() : 0;
}
```

Java 8中这一步做了优化，只做一次16位右位移异或混合，而不是四次，但原理是不变的

```java
//In Java 8
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

`indexFor(hash, table.length)`

位运算( & )代替取模运算( % )：**X % 2^n^ = X & (2^n^ – 1)**

确定桶下标的最后一步是将 key 的 hash 值对桶个数取模：hash % capacity，如果能保证 capacity 为 2 的 n 次方，那么就可以将这个操作转换为位运算。

```java
static int indexFor(int h, int length) {
    return h & (length-1);
}
```

#### 扩容-基本原理

设 HashMap 的 table 长度为 M，需要存储的键值对数量为 N，如果哈希函数满足均匀性的要求，那么每条链表的长度大约为 N/M，因此平均查找次数的复杂度为 O(N/M)。

为了让查找的成本降低，应该尽可能使得 N/M 尽可能小，因此需要保证 M 尽可能大，也就是说 table 要尽可能大。HashMap 采用动态扩容来根据当前的 N 值来调整 M 值，使得空间效率和时间效率都能得到保证。

和扩容相关的参数主要有：capacity、size、threshold 和 load_factor。

| 参数       | 含义                                                         |
| ---------- | ------------------------------------------------------------ |
| capacity   | table 的容量大小，默认为 16。需要注意的是 capacity 必须保证为 2 的 n 次方。 |
| size       | 键值对数量。                                                 |
| threshold  | size 的临界值，当 size 大于等于 threshold 就必须进行扩容操作。 |
| loadFactor | 装载因子，table 能够使用的比例，threshold = capacity * loadFactor。 |

```java
static final int DEFAULT_INITIAL_CAPACITY = 16;

static final int MAXIMUM_CAPACITY = 1 << 30;

static final float DEFAULT_LOAD_FACTOR = 0.75f;

transient Entry[] table;

transient int size;

int threshold;

final float loadFactor;

transient int modCount;
```

`addEntry()` 

```java
void addEntry(int hash, K key, V value, int bucketIndex) {
    Entry<K,V> e = table[bucketIndex];
    table[bucketIndex] = new Entry<>(hash, key, value, e);
    //每次扩容，增加为原来的两倍
    if (size++ >= threshold)
        resize(2 * table.length);
}

```

扩容使用 `resize()` 实现，扩容操作同样需要把 oldTable 的所有键值对重新插入 newTable 中，因此这一步是很费时的。

```java
void resize(int newCapacity) {
    Entry[] oldTable = table;
    int oldCapacity = oldTable.length;
    if (oldCapacity == MAXIMUM_CAPACITY) {
        threshold = Integer.MAX_VALUE;
        return;
    }
    Entry[] newTable = new Entry[newCapacity];
    transfer(newTable);
    table = newTable;
    threshold = (int)(newCapacity * loadFactor);
}

void transfer(Entry[] newTable) {
    Entry[] src = table;
    int newCapacity = newTable.length;
    for (int j = 0; j < src.length; j++) {
        Entry<K,V> e = src[j];
        if (e != null) {
            src[j] = null;
            do {
                Entry<K,V> next = e.next;
                int i = indexFor(e.hash, newCapacity);
                e.next = newTable[i];
                newTable[i] = e;
                e = next;
            } while (e != null);
        }
    }
}
```

#### 扩容-重新计算桶下标

在进行扩容时，需要把键值对重新放到对应的桶上。HashMap 使用了一个特殊的机制，可以降低重新计算桶下标的操作。

假设原数组长度 capacity 为 16，扩容之后 new capacity 为 32：

```html
capacity     : 00010000
new capacity : 00100000
```

对于一个 Key：

- 它的哈希值如果在第 5 位上为 0，那么取模得到的结果和之前一样；
- 如果为 1，那么得到的结果为原来的结果 +16。

#### 计算数组容量

HashMap 构造函数允许用户传入的容量不是 2 的 n 次方，因为它可以自动地将传入的容量转换为 2 的 n 次方。

先考虑如何求一个数的掩码，对于 10010000，它的掩码为 11111111，可以使用以下方法得到：

```html
mask |= mask >> 1    11011000
mask |= mask >> 2    11111110
mask |= mask >> 4    11111111
```

mask+1 是大于原始数字的最小的 2 的 n 次方。

```html
num     10010000
mask+1 100000000
```

以下是 HashMap 中计算数组容量的代码：

```java
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

#### 链表转红黑树

从 JDK 1.8 开始，一个桶存储的链表长度大于 8 时会将链表转换为红黑树。

#### 与 HashTable 的比较

- HashTable 使用 synchronized 来进行同步。
- HashMap 可以插入键为 null 的 Entry。
- HashMap 的迭代器是 fail-fast 迭代器。
- HashMap 不能保证随着时间的推移 Map 中的元素次序是不变的。

### ConcurrentHashMap

```java
static final class HashEntry<K,V> {
    final int hash;
    final K key;
    volatile V value;
    volatile HashEntry<K,V> next;
}
```







## 参考资料

[CS-Notes](<https://cyc2018.github.io/CS-Notes/#/>)