### ArrayList

底层的实现就是一个数组(固定大小),当数组长度不够用的时候就会重新开辟一个新的数组，然后将原来的数据拷贝到新的数组内。

### LinkedList

LinkedList 底层是一个链表，是由java实现的一个双向链表其节点如下:

~~~java
 /**
   * Pointer to first node.
   * Invariant: (first == null && last == null) ||
   *            (first.prev == null && first.item != null)
   */
  transient Node<E> first;

  /**
   * Pointer to last node.
   * Invariant: (first == null && last == null) ||
   *            (last.next == null && last.item != null)
   */
  transient Node<E> last;
~~~

### HashMap

底层是一个数组+链表实现

定义一个LinkedList的数组,然后将数据存储到这个链表数组内

首先有一个每个元素都是链表（可能表述不准确）的数组，当添加一个元素（key-value）时，就首先计算元素key的hash值，
以此确定插入数组中的位置，但是可能存在同一hash值的元素已经被放在数组同一位置了，这时就添加到同一hash值的元素的后面，
他们在数组的同一位置，但是形成了链表，同一各链表上的Hash值是相同的，所以说数组存放的是链表。
