[TOC]

## 源码分析

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

### CopyOnWriteArrayList



## 参考资料

[CS-Notes](<https://cyc2018.github.io/CS-Notes/#/>)