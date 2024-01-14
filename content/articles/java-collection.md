---
title: "Java集合基础"
description: "Java集合详解、数据结构、代码分析"
published: "2023-11-20"
cover: "https://r2-img.lnbiuc.com/blog/2023/11/82370e1f0ec8be9982e46789d43cdb9a.png"
tags: ["java"]
views: "41"
---

# Java集合

## Java集合框架

![image](https://user-pic-1308549476.cos.ap-nanjing.myqcloud.com/pic/99111661686225193.png)

## 概述

> Collection和Map可以理解为存放内容的容器，有两大接口派生而来，一个是`Collection`，另一个是`Map`，在`Collection`接口下，又分别有三个子接口，分别是`List`、`Set`、`Queue`。
> 相同的是Collection都只能用来存储对象，对于基本数据类型(int, long, float, double等)，需要将其转化为包装类型，这些装箱操作能自动完成，但是会使用额外的性能和空间。
> 不同的是Collection只能用来存放单例对象，而Map只能存放双例对象

---

> Q1：List，Set，Queue，Map四者的区别？

- `List`存储元素是有序的，可重复的。
- `Set`存储元素是无序的，不可重复的。
- `Queue`按照队列规则确定先后顺序，是有序的，可重复的。
- `Map`使用key-value的键值对存储元素，其中key是无序的，不可重复的，value是无序，可重复的，且每个key只能对应一个value。

### List

- `ArrayList<Object[]>`
- `Vector<Object[]>`
- `LinkedList<Object>`

---

> Q2：ArrayList和Vector的区别？

- `ArrayList`是`List`的主要实现类，底层使用`Object[]`存储，适用于需要频繁查找的工作，线程不安全。
- `Vector`是`List`古老实现类，包含很多不属于集合框架的传统方法，底层使用`Object[]`存储，同步访问，线程安全。

> Q3：ArrayList和LinkedList的区别？

- **是否线程安全**：都不线程安全
- **底层数据结构**：`ArrayList`底层使用Object数组。
  `LinkedList`在JDK1.6以前使用循环链表，JDK1.7使用链表
- **是否支持随机访问**：即是否直接`get(int index)`,`ArrayList`支持，`LinkedList`不支持
- **空间占用情况**：`ArrayList`占用多余空间在于`ArrayList`自动扩容的性质。
  `LinkedList`占用空间在于需要额外存储前驱以及后继
  > 优先使用`ArrayList`

### Set

- `HashSet<Object>`
- `LinkedHashSet<Object>`
- `TreeSet<Object>`

---

> Q4: HashSet、LinkedHashSet、TreeSet 三者区别

- 三者都实现了Set接口，都能保证元素的唯一，但是都不时线程安全的
- 三者主要区别在于底层数据结构不同
  - `HashSet`底层数据结构时哈希表，基于HashMap实现
  - `LinkedHashSet`底层时链表和哈希表，元素插入于取出顺序先入先出
  - `TreeSet`底层书红黑树，元素是有序的
- 由于底层数据结构的不同，导致三者适用场景不同 - `HashSet`适用于不需要保证元素插入和取出顺序的场景 - `LinkedHashSet`适用于元素插入取出顺序满足先入先出特性的场景 - `TreeSet`适用于需要支持元素自定义排序规则的场景
  > Q5：无序性
- 无序性是指存储的数据在底层数组中不按数组的索引顺序添加，而是根据数据的哈希值添加

---

### Queue

- `PriorityQeue<Object>`
- `ArrayQeue<Object>`
  > Q6：Queue 与 Deque 的区别
- `Queue`是单向队列，只能从一端插入元素，另一端删除元素，遵循先入先出原则
- `Deque`是双向队列，在队列两端都可插入删除元素，仍然遵循先入先出原则

> Q7：ArrayDeque 与 LinkedList 的区别

- 两者都实现了Deque接口，都具有队列的功能
- **数据结构不同**：
  - `ArrayDeque`基于可变长的数组和双向指针实现
  - `LinkedList`基于链表实现
- **性能**：
  - `ArrayDeque`插入时可能出现扩容，但是复杂度为O(1)
  - `LinkedList`不需要扩容，但是每次插入数据都需要申请新的堆空间
    `ArrayDeque`拥有更好的性能
- `ArrayDeque` 可用于实现栈

---

### Map

- `HashMap<K,V>`
- `LinkedHashMap<K,V>`
- `HashTable<K,V>`
- `TreeMap<K,V>`

> Q8：HashMap 和 Hashtable 的区别
>
> 1. ArrayList实现了List接口，是顺序容器，底层通过数组实现。
> 2. 当使用add()方法是，为了增加效率，未实现同步(synchronized)，如果需要多线程并发访问，应该使用Vector
> 3. HashMap可以存储null value和null key，HashTable不允许出现null value 和 null key，会抛出异常`NullPointerException`
> 4. 初始容量大小和扩充容量大小的区别
>    1. 创建时未指定容量，HashTable默认为11，每次扩充为2n+1。HashMap默认为16，每次扩容为2n
>    2. 创建时指定了初始容量，HashTable会使用你指定的大小，HashMap会扩容2的幂次方大小
>    3. 底层数据结构：HashMap在当链表长度大于阈值8之后，会将链表转为红黑树，以较少索引时间

# 数据结构

> [二叉搜索树算法可视化](https://visualgo.net/zh/bst)
>
> [二叉搜索树相关算法介绍](https://www.cnblogs.com/MrListening/p/5782752.html)

## 二叉搜索树

> 二叉搜索树（又：二叉查找树，二叉排序树，Binary Search Tree，BST）是一种二叉树。
>
> 其中每个结点最多有两个子结点且具有二叉搜索树性质。
>
> 左子树上所有结点的值均小于它的根结点的值。
>
> 右子树上所有结点的值均大于它的根结点的值。

个人对于二叉搜索树的理解，例如：下图是一个二叉搜索树

1. 如果要用组数构建平衡二叉树，首先将这组数排序。
2. 选出中位数为根节点，大于根节点的数放在右边，小于根节点的数放在左边。
3. 之后在小于根节点部分继续选出中位数，为上个根节点的左孩子。大于根节点部分选出中位数，为根节点右孩子。
4. 继续选取中位数，构建完整二叉搜索树。

![image-20220909170628143](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220909170628143.png)

> 二叉搜索树的插入，查找时间复杂度低为O(log n)

### 查找

> 在二叉树b中查找x的过程
>
> 1. 如果b树 == null，查询失败
> 2. 如果x等于b的根节点数，则查找成功，否则：
> 3. 如果x小于b的根节点数，则搜索左子树，否则
> 4. 查找右子树

**例如：查找x = 77的过程**

1. x = 77，x > 41 查找右子树
2. 右子树根节点65，x > 65，查找右子树
3. 右子树根节点91，x < 91，查找左子树

![image-20220909171820326](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220909171820326.png)

![image-20220921210426744](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220921210426744.png)

### 插入

> 不能插入重复节点
>
> 1. 若b是空树，将x作为根节点插入
> 2. x 等于b根节点，插入成功，否则
> 3. x 小于b根节点，x插入到左子树，否则
> 4. x 插入到右子树中。（新插入节点总是叶子节点）

例如：插入x = 27

![image-20220909172718987](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220909172718987.png)

### 删除

删除的4种情况

1. 要删除的节点无左右孩子

   - 归为234情况中一种

2. 只有左孩子

   - 让该节点的父亲结点指向该节点的左孩子，然后删除该节点

   ![image-20220909173759162](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220909173759162-20220909173918255.png)

3. 只有右孩子

   - 让该节点的父亲结点指向该节点的右孩子，然后删除该节点

   ![image-20220909173821209](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220909173821209.png)

4. 有左右孩子

   1. 找到该节点的右子树中的最左孩子（也就是右子树中序遍历的第一个节点）
   2. 把它的值和要删除的节点的值进行交换
   3. 按照之前的方法删除该节点

   ![image-20220909173857000](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220909173857000.png)

## 平衡二叉树

> [原文连接](https://zhuanlan.zhihu.com/p/56066942)

> 平衡二叉查找树，简称平衡二叉树，又称AVL树
>
> 平衡二叉树可能是空树。
>
> 如果不是空树，任何一个结点的左子树与右子树都是平衡二叉树，并且高度之差不超过 1。

### 平衡因子

> 某节点左子树右子树高度差为该节点的平衡因子

> 平衡二叉树中不存在高度差大于1的节点

个人对于平衡二叉树的理解：在二叉搜索树的基础上，通过调整树的结构（左旋，右旋），来使二叉树降低高度，保持平衡。为了保持平衡，平衡二叉树需要严格遵守性质3，左子树与右子树高度差不超过1。这导致了添加和删除节点很容易引起树的不平衡，需要消耗资源来使树重新达到平衡状态。这些资源消耗大，带来的收益却有限，所以产生了红黑树。

### AVL树插入

看原文

如果插入之后某个节点的平衡因子大于1或者小于-1，则需要通过旋转**最小失衡子树**来保持树的平衡。

> **最小失衡子树**：在新插入的节点向上查找，以第一个平衡因子的绝对值超过1的节点为根的子树，成为最小不平衡子树。
>
> 例如：
>
> ![image-20220914153626091](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220914153626091.png)
> 此图中，99为新插入的节点，66的右子树为最小失衡子树。

### 左旋

1. 节点（第一个平衡因子绝对值超过1的节点）的右孩子替代此节点的位置
2. 右孩子的左子树变为该节点的右子树
3. 节点本身变成右孩子的左子树

![image-20220914154035495](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220914154035495.png)

### 右旋

1. 节点的左孩子代替此节点
2. 节点的左孩子的右子树边唱节点的左子树
3. 将此节点作为左孩子的右子树

## 红黑树

> [原文连接](https://www.jianshu.com/p/e136ec79235c)

### 定义及性质

红黑树是一种含有红黑节点，并且能自平衡的二叉搜索树。

满足以下性质：

1. 每个节点只能是黑色或者红色
2. 根节点是黑色
3. 叶子节点是黑色
4. 每个红色节点的两个子节点一定是黑色
5. **任意一节点到每个叶子节点的路径都包含数量相同的黑节点**

红黑树在插入或者删除时，树的结构会发生改变，导致条件4、5不成立，需要通过旋转来使其重新满足红黑树的条件

个人对于红黑树的理解：红黑树是一种弱平衡二叉树，由于平衡二叉树对于平衡的严格限制，导致需要频繁旋转树来达到平衡，红黑树放宽了这一限制，使得在需要频繁插入删除节点的情况下，红黑树优于平衡二叉树。同时红黑树给每个节点都标记了颜色，使红黑树有三种方式来保持平衡，左旋、右旋、变色。

红黑树通过左旋、右旋、变色来保持平衡：

- 左旋：右子结点变为旋转结点的父结点，右子结点的左子结点变为旋转结点的右子结点，左子结点保持不变。
- 右旋：左子结点变为旋转结点的父结点，左子结点的右子结点变为旋转结点的左子结点，右子结点保持不变。
- 变色：节点的颜色由红变黑或由黑变红

### 查找

同二叉树的查找方式。

# 源码

## ArrayList

### 自动扩容机制

- 每当向ArrayList中添加元素时，会先判断插入之后的size是否会大于当前的size，如果是，则进行扩容。

- 扩容通过方法`ensureCapacity(int minCapacity)`实现

- 当初始化一个ArrayList，且没有指定capacity时，该数组的长度是0。

- 当第一次向ArrayList中添加元素时，数组会扩容capacity到10。

- 当添加第10个元素时，此时的容量仍为10，不会进行扩容。

- 当添加第11个元素时，数组会扩容到原来的1.5倍。在添加第11个元素之前，会判断出添加第11个元素会导致超过最大容量，此时数组会进行扩容，扩容到原有容量的1.5倍，也就是15，之后执行添加操作。

扩容

```java
private void grow(int minCapacity)
            {
            // oldCapacity为旧容量，newCapacity为新容量
            int oldCapacity = elementData.length;
            int newCapacity = oldCapacity + (oldCapacity >> 1);
            // 将oldCapacity 右移一位，其效果相当于oldCapacity 除2
            // 位运算快于整除运算
            if (newCapacity - minCapacity < 0)
                newCapacity = minCapacity;
            // 检查扩容后容量是否满足最小需要容量
            if (newCapacity - MAX_ARRAY_SIZE > 0)
                newCapacity = hugeCapacity(minCapacity);
            // 检查扩容后容量是否大于规定的数组最大容量
            // 如果超过了使用hugeCapacity()方法比较minCapacity和 MAX_ARRAY_SIZE
            // 如果minCapacity大于MAX_ARRAY_SIZE，则新容量则为Integer.MAX_VALUE
            // 否则，新容量大小则为 MAX_ARRAY_SIZE。
            elementData = Arrays.copyOf(elementData, newCapacity);
        }
```

### `System.arraycopy()`和`Arrays.copyOf()`方法

```
    /**
    *   复制数组
    * @param src 源数组
    * @param srcPos 源数组中的起始位置
    * @param dest 目标数组
    * @param destPos 目标数组中的起始位置
    * @param length 要复制的数组元素的数量
    */
    public static native void arraycopy(Object src,  int  srcPos,
                                        Object dest, int destPos,
                                        int length);

```

**区别**：
`arraycopy()` 需要目标数组，将原数组拷贝到你自己定义的数组里或者原数组，而且可以选择拷贝的起点和长度以及放入新数组中的位置 `copyOf()` 是系统自动在内部新建一个数组，并返回该数组。

### 方法

- `add()`

  - ```java
      public boolean add(E e) {
              ensureCapacityInternal(size + 1);  // Increments modCount!!
              elementData[size++] = e;
              return true;
          }
    ```

  - 1. 是否扩容检查
    2. 到数组最后一位插入

- `add(int index, E e)`根据索引插入，该方法需要先对元素进行移动

  - ```java
      public void add(int index, E element) {
              rangeCheckForAdd(index);

              ensureCapacityInternal(size + 1);  // Increments modCount!!
              System.arraycopy(elementData, index, elementData, index + 1,
                               size - index);
              elementData[index] = element;
              size++;
          }
    ```

  - 1. 检查索引是否越界
    2. 是否扩容检查
    3. 移动索引位之后的元素，空出一个位置
    4. 插入

- `remove(int index)`删除指定位置元素，将删除点之后的元素向前移动一个位置

  - ```java
      public E remove(int index) {
              rangeCheck(index);

              modCount++;
              E oldValue = elementData(index);

              int numMoved = size - index - 1;
              if (numMoved > 0)
                  System.arraycopy(elementData, index+1, elementData, index,
                                   numMoved);
              elementData[--size] = null; // clear to let GC do its work

              return oldValue;
          }
    ```

  - **注意：**为了让GC起作用，必须显式的为最后一个位置赋`null`值。

- `remove(Object o)`删除第一个与Object o equals的元素

  - ```java
      public boolean remove(Object o) {
              if (o == null) {
                  for (int index = 0; index < size; index++)
                      if (elementData[index] == null) {
                          fastRemove(index);
                          return true;
                      }
              } else {
                  for (int index = 0; index < size; index++)
                      if (o.equals(elementData[index])) {
                          fastRemove(index);
                          return true;
                      }
              }
              return false;
          }
    ```

- `trimToSize()`将底层数组的大小调整为当前列表保存元素实际大小

## Vector

## LinkedList

> `LinkedList`实现了多个接口，同时满足顺序容器，队列，栈的特性。

> 数据结构：JDK在JDK1.6以前使用循环链表，JDK1.7使用链表

### 方法

- `add(int index, E element)`
  1. 根据index找到要插入的位置
  2. 修改引用
     因为链表是双向的，姐可以从前往后查找，也可以从后往前查找，所以第一步查找位置时，会先判断index更靠近前端还是后端。

## PriorityQueue

> 优先队列

1. 相比于Queue必须先进先出，PriorityQueue可以通过修改优先级来指定出的顺序
2. 放入PriorityQueue的元素必须实现Comparable接口
3. 如果需要放入的元素没有实现Comparable接口，则需要在构造时传入的比较器
4. 优先取出权值最小的元素
5. 非线程安全
6. 使用二叉堆数据结构实现，底层使用可变长数组存储数据
7. 通过堆元素的上浮和下沉，实现在O(logn)时间复杂度内插入元素和删除堆顶元素

## HashMap

> JDK8中HashMap底层数据结构：数组+链表+红黑树，JDK8之前为：数组+链表
> JKD8中，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。

> 即：如果链表长度 > 8 && 数组长度 < 64 才将链表转换为红黑树
> 如果数组长度 < 64则会先扩容数组。如果红黑树中元素个数 < 6，将红黑树转换回链表。每个数组一个元素对应一个链表。

![image-20220909134246601](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220909134246601.png)

### HashMap.put()

> HashMap 通过 key 的 `hashcode` 经过扰动函数处理过后得到 hash 值，然后通过 `(n - 1) & hash` 判断当前元素存放的位置（这里的 n 指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的 hash 值以及 key 是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。

> 如果Hash冲突，插入之前会判断该节点类型是TreeNode还是Node，如果是TreeNode则按照红黑树的方法插入元素，如果是Node，在JDK1.7中使用头插法，JDK1.8中使用尾插法。

> **头插法和尾插法**：一个是在链表头部插入元素，一个是咋链表尾部插入元素。头插法会改变链表的顺序，在并发时，可能会出现链表成环的问题。尾插法不会改变链表原来的顺序，不会出现成环的问题。

> **"扰动函数"**： HashMap 的 `hash` 方法。使用 `hash` 方法也就是扰动函数是为了防止一些实现比较差的 `hashCode()` 方法 换句话说使用扰动函数之后可以减少碰撞。

> **“拉链法”** ：将链表和数组相结合。也就是说创建一个链表数组，数组中每一格就是一个链表。若遇到哈希冲突，则将冲突的值加到链表中即可。

```java
public V put(K key, V value) {
        return putVal(hash(key), key, value, false, true);
    }

    /**
     * Implements Map.put and related methods.
     *
     * @param hash key的hash值
     * @param key the key
     * @param value the value to put
     * @param onlyIfAbsent 如果为真，只有在不存在key时才会put
     * @param evict if false, the table is in creation mode.
     * @return previous value, or null if none
     */
    final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        //第一次put会使用resize方法检查是否需要扩容
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        //给节点赋值
        //如果没有节点，就创建一个新的节点
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        //eles如果这个节点已经有数据了
        else {
            Node<K,V> e; K k;
            // 首先，判断该位置的第一个数据和我们要插入的数据，key 是不是"相等"
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                //如果是，取出这个节点
                e = p;
            // 如果该节点是代表红黑树的节点，调用红黑树的插值方法
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            //else数组该位置不是红黑树节点，而是链表
            else {
                for (int binCount = 0; ; ++binCount) {
                    //插入到链表后面
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        //TREEIFY_THRESHOLD = 8
                        //如果新插入的值时链表中的第8个
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            //treeifyBin()方法会将链表转化为红黑树
                            treeifyBin(tab, hash);
                        break;
                    }
                    //如果在链表中找到了相等的key
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        //break 此时e为链表中与要插入新值的key相等的node
                        break;
                    p = e;
                }
            }
            //e != null 说明旧值的key与插入的key相等
            //值覆盖，返回旧值
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        //如果插入后size超过阈值，就对其扩容
        //从整个方法来看，先插入再考虑扩容
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
```

### HashMap扩容

> 每次扩容为原来的2倍
>
> resize()同时也用来初始化容量

```java
	/**
     * Initializes or doubles table size.  If null, allocates in
     * accord with initial capacity target held in field threshold.
     * Otherwise, because we are using power-of-two expansion, the
     * elements from each bin must either stay at same index, or move
     * with a power of two offset in the new table.
     *
     * @return the table
     */
    final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        //扩容
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            //数组扩大一倍
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                //阈值扩大一倍
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
        //用新的数组大小初始化新的数组
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        //初始化数组结束
        table = newTab;

        //数据迁移
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
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
        return newTab;
    }
```

```java
 /**
     * Returns a power of two size for the given target capacity.
     */
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

> Q11：为什么HashMap的长度是2的幂次方
>
> HashMap中数组下标的计算方式是 `(n - 1) & hash`，为什么是&而不是%呢
>
> **“取余(%)操作中如果除数是 2 的幂次则等价于与其除数减一的与(&)操作（也就是说 hash%length == hash&(length-1)的前提是 length 是 2 的 n 次方；）。”** 并且 **采用二进制位操作 &，相对于%能够提高运算效率，这就解释了 HashMap 的长度为什么是 2 的幂次方。**

### HashMap.get()

1. 计算key的hash值，根据hash值找到对应数组下标
2. 判断数组该位置的元素是否时需要的value
3. 如果不是，判断该元素类型是否时`TreeNode`，如果是，用红黑树的方法取数据
4. 如果不是，遍历链表，直到找到(== || equals)的key

```java
public V get(Object key) {
        Node<K,V> e;
        return (e = getNode(hash(key), key)) == null ? null : e.value;
    }

    /**
     * Implements Map.get and related methods.
     *
     * @param hash hash for key
     * @param key the key
     * @return the node, or null if none
     */
    final Node<K,V> getNode(int hash, Object key) {
        Node<K,V>[] tab; Node<K,V> first, e; int n; K k;
        if ((tab = table) != null && (n = tab.length) > 0 &&
            (first = tab[(n - 1) & hash]) != null) {
            //判断第一个节点是不是要找的
            if (first.hash == hash && // always check first node
                ((k = first.key) == key || (key != null && key.equals(k))))
                return first;
            if ((e = first.next) != null) {
                //判断是否为红黑树
                if (first instanceof TreeNode)
                    return ((TreeNode<K,V>)first).getTreeNode(hash, key);
                do {
                    //遍历链表
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        return e;
                } while ((e = e.next) != null);
            }
        }
        return null;
    }
```

> Q12：HashMap的7种遍历方式
>
> [原文连接](https://mp.weixin.qq.com/s/zQBN3UvJDhRTKP6SzcZFKw)
>
> 1. 使用迭代器（Iterator）EntrySet 的方式进行遍历；
> 2. 使用迭代器（Iterator）KeySet 的方式进行遍历；
> 3. 使用 For Each EntrySet 的方式进行遍历；
> 4. 使用 For Each KeySet 的方式进行遍历；
> 5. 使用 Lambda 表达式的方式进行遍历；
> 6. 使用 Streams API 单线程的方式进行遍历；
> 7. 使用 Streams API 多线程的方式进行遍历。
>
> 结论：性能最好的是`entrySet`最差的是`keyset`

## HashSet

> HashSet基于HashMap实现
>
> **区别**：`HashSet`使用成员对象计算hashcode值，hashcode有可能重复，当hashcode重复时还需要equals判断是否相等
>
> HashMao使用key计算hashcode

> Q9：HashMap 和 TreeMap 区别
>
> - 两者都继承`AbstractMap`,但是TreeMap额外实现了`NavigableMao`和`SOrtedMap`使TreeMap何以查询和排序
> - TreeMap排序默认按照key升序排列

> Q10：HashSet如何检查是否重复
>
> 当你把对象加入`HashSet`时，`HashSet` 会先计算对象的`hashcode`值来判断对象加入的位置，同时也会与其他加入的对象的 `hashcode` 值作比较，如果没有相符的 `hashcode`，`HashSet` 会假设对象没有重复出现。但是如果发现有相同 `hashcode` 值的对象，这时会调用`equals()`方法来检查 `hashcode` 相等的对象是否真的相同。如果两者相同，`HashSet` 就不会让加入操作成功。

## ConcurrentHashTable

> 并发HashMap(ConcurrentHashMap)

> Q11：ConcurrentHashMap和HashMap的区别：
>
> - **底层数据结构**：JDK7中`ConcurrentHashMap`底层采用**分段的数组+链表**，JDK8种使用**数组+链表+红黑树**实现。`HashTable`使用数组+链表，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的。
> - **实现线程安全的方式**：JDK8中，`ConcurrentHashMap`并发控制使用`Synchorized`和CAS操作。也使用 `synchronized` 来保证线程安全，但是效率非常低下。当一个线程访问同步方法时，其他线程也访问同步方法，可能会进入阻塞或轮询状态，如使用 put 添加元素，另一个线程不能使用 put 添加元素，也不能使用 get，竞争会越来越激烈效率越低。

HashTable的数据结构

![image-20220909164559669](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220909164559669.png)

ConcurrentHashTable的数据结构

![image-20220909164639598](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220909164639598.png)

> Q12：JDK 1.7 和 JDK 1.8 的 ConcurrentHashMap 实现有什么不同？
>
> 1. 线程安全实现方式不同：
>    - JDK7使用Segment分段锁保证安全。
>    - JDK8使用Node + CAS + synchorized保证安全，synchorized只锁定当前链表或红黑树的首节点
> 2. 解决Hash碰撞的方法不同：
>    - JDK7使用拉链法
>    - JDK8使用拉链法结合红黑树（怎么结合？链表长度超过8之后，将链表转换为红黑树）
> 3. 并发度不同：
>    - JDK7中最大并发数是Segment个数，默认16。
>    - JDK8中最大并发度是Node数组的大小，并发量更大

## TreeMap和TreeSet

> TreeMap实现了SortedMap接口，可以按照key的自然顺序（natural ordering）排序或者手动传入比较器（Comparator）
>
> TreeMap底层通过红黑树实现，查找，添加，删除，时间复杂度均为log(n)
>
> TreeMap是非同步的，线程不安全

## WeekHashMap
