## Queue

- 什么是队列

  队列是一种先进先出策略的抽象数据结构。比如生活中排队，总是按照先来的先服务，后来的后服务。队列在数据结构中举足轻重，其在算法中应用广泛，最常用的就是在宽度优先搜索中，记录待扩展的节点。队列内部存储元素的方式，一般有两种，数组和链表。两者最主要的区别是：数组对随机访问有较好的性能，链表对于插入删除元素有较好的性能

  ```java
  public class MyQueue {
    /*
    * @param item: An integer
    * @return: nothing
    */
    public int head, tail;
    public int MAXSIZE = 100000;
    public int[] queue = new int[MAXSIZE];
    
    public MyQueue() {
      head = tail = 0;
    }
    public void enqueue(int item) {
      if (tail == MAXSIZE) {
        return;
      }
      queue[tail++] = item;
    }
    
    public int dequeue() {
      if (head == tail) {
        return -1;
      }
      
      return queue[head++];
    }
  }
  ```
  
  
  
  但是大家会发现，如果这样实现队列的话，我们考虑MAXSIZE为4的情况，如果我们采取下面的操作
  
  ```
  enqueue(1)
  enqueue(2)
  enqueue(3)
  enqueue(4)
  dequeue()
  dequeue()
  ```
  
  结束后数组的状态时[^, ^, 3, 4], head = 2, tail = 4。（'^'表示该位置为空，即当前元素已经出队）
  从我们之前的判断来看，`tail == MAXSIZE` , 当前队列已经满了，不能继续添加元素了，但是实际上我们还可以继续添加元素。因此在使用数组实现队列时，可能会出现空间未有效利用的情况，因此，我们有两种解决方法：
  
  1. 使用链表来实现队列
  2. 使用数组来实现循环队列
  
  那么我们就先来看用链表来实现队列的方法：
  
  链表是由多个节点构成的，一个节点由两部分组成：一个是数据域，一个是指针域
  
  链表分为：单链表（只能是父节点引用子节点），双链表（相邻的节点可互相引用），循环链表（在双链表的基础上，头尾节点可互相引用）。实现链表，就是在链表里加入节点，使用节点的引用域使节点之间形成连接，可互相调用。链表队列的实现原理：首先定义一个节点类，节点类包含引用域和数据域，然后定义一个链表类，链表类形成节点间的引用关系
  
  ```java
  class Node {
    int val;
    
    public Node(int val) {
      this.val = val;
    }
  }
  
  class MyQueue {
    Node head;
    Node tail;
    
    public MyQueue() {
      head = tail = null;
    }
    
    public void enqueue(int item) {
      if(tail == null) {
        tail = new Node(item);
        head = tail;
      } else {
        tail.next = new Node(item);
        tail = tail.next;
      }
    }
    
    public int dequeue() {
      if(head == null) {    //尽量避免写else 可以写return -1；
        return -1;
      } else {
        int val = head.val;
        head = head.next;
        return val;
      }
    }
  }
  ```
  
  队列是一种先进先出的线性表，它只允许在表的另一端进行插入，而在另一端删除元素。允许插入的一段称为队尾，允许删除的一端称为队首。但是我们之前也提到了，数组实现的队列会导致“虽然数组没满，但是tail已经指向了数组末尾，返回数组已满，队列溢出的错误信号，我们称之为假溢出。
  
  为充分利用向量空间，克服假溢出的现象的方法是：将向量空间想象为一个首尾相接的圆环，并称这种向量为循环向量。存储在其中的队列称为循环队列。循环队列是把顺序队列首尾相连，把存储队列元素的表从逻辑上看作一个环，称为循环队列。
  
  在循环队列中，除了用一组地址连续的存储单元依次存储从队首到队尾的元素外，还需要附设两个整型变量head和tail分别指示队首和队尾的位置。
  
  我们可以将循环队列视作一个类，通过成员变量数组来表示一组地址连续的存储单元，在定义两个成员变量head和tail，将循环队列的基本操作定义成类的方法，循环效果则用“模”运算实现，以此来实现循环队列。
  
  每当tail到达末尾的时候，将tail对MAXSIZE取模，使其回到队首。但是如果这样我们会发现一个问题，队列为空和队列已满的条件都成了tail == head
  
  为了避免这种无法判断的情况，我们规定当循环队列只剩下一个空位的时候，就认为队列已满。这样队列已满的条件就成了（tail + 1) % MAXSIZE == head
  
  ```java
  public class MyQueue {
    public int head, tail;
    public int SIZE = 4;
    public int[] queue = new int[SIZE];
    
    public MyQueue() {
      head == tail;
    }
    
    public void enqueue(int item) {
      if ((tail + 1) % SIZE == head) {
        return;
      }
      queue[tail++] = item;
      tail = tail % SIZE;
    }
    
    public int dequeue() {
      if (tail == head) {
        return -1;
      } else {
          int item = queue[head++];
          head %= SIZE;
          return item;
      }
    }
  }
  ```
  
  Set注重独一无二，该体系集合可以知道某物是否已经存在于集合中，不会储存重复的元素。Set的实现在面试中常用的是：HashSet与TreeSet
  
  - HashSet
  
    无重复数据
  
    可以用空数据
  
    数据无序
  
  ```java
  Set<String> set = new HashSet<>();
  for (int i = 1; i < 6; i ++) {
  	set.add(i + "");
  }
  set.add("1"); //不会重复写入数据
  set.add(null);//可以写入空数据
  Iterator<String> iter = set.iterator();
  while (iter.hasNext()) {
  	system.out.print(iter.next() + " ");//数据无序 
  }// 输出(无序)为 3 4 1 5 null 2
  ```

- TreeSet

  无重复数据

  不能有空数据

  数据有序

  ```java
  Set<String> set = new TreeSet<>();
  for (int i = 1; i < 6; i ++) {
  	set.add(i + "");
  }
  set.add("1"); //不会重复写入数据
  //set.add(null);//不可以写入空数据
  Iterator<String> iter = set.iterator();
  while (iter.hasNext()) {
  	system.out.print(iter.next() + " ");//数据有序
  }// 输出(有序)为 1 2 3 4 5
  ```

Map

map用于存储具有映射关系的数据。Map中存了两组数据(key与value)，它们都可以是任何类型的数据，key不能重复，我们可以通过key取到对应的value。Map的实现类在面试中是：HashMap和TreeMap

- HashMap

  key无重复，value允许重复

  允许key和value为空

  数据无序

  ```java
  public class Solution {
      public static void main(String[] args){
          Map<String, String> map = new HashMap<>();
          for (int i = 5; i > 0; i --) {
              map.put(i + "", i + "");
          }
          map.put("1","1");//key无重复
          map.put("11","1");//value可以重复
          map.put(null, null);//可以为空
          for (Iterator i = map.keySet().iterator(); i.hasNext(); ) {
              String key = (String)i.next();
              String value = map.get(key);
              System.out.println("key = " + key + ", value = " + value);
          }
      }
  }
  //输出
  /*
  key = 11, value = 1
  key = null, value = null
  key = 1, value = 1
  key = 2, value = 2
  key = 3, value = 3
  key = 4, value = 4
  key = 5, value = 5
  */
  //输出顺序与输入顺序无关
  ```

- TreeMap

  key无重复，value允许重复

  不允许有null

  有序（存入元素的时候对元素进行自动排序，迭代输出的时候就按排序顺序输出)

  ```java
  public class Solution {
      public static void main(String[] args){
          Map<String, String> map = new TreeMap<>();
          for (int i = 5; i > 0; i --) {
              map.put(i + "", i + "");
          }
          map.put("1","1");//key无重复
          map.put("11","1");//value可以重复
          //map.put(null, null);//不可以为空
          for (Iterator i = map.keySet().iterator(); i.hasNext(); ) {
              String key = (String)i.next();
              String value = map.get(key);
              System.out.println("key = " + key + ", value = " + value);
          }
      }
  }
  //输出
  /*
  key = 1, value = 1
  key = 11, value = 1
  key = 2, value = 2
  key = 3, value = 3
  key = 4, value = 4
  key = 5, value = 5
  */
  //输出顺序位String排序后的顺序
  ```

- List

  一个list是一个元素有序的、可以重复、可以为null的集合，list在面试中常用的是：LinkedList和ArrayList

  - LinkedList

    基于链表实现

  - ArrayList

    基于动态数组实现

  - 对于随机访问get和set，ArrayList绝对优于LinkedList, 因为LinkedList要移动指针

  - 对于新增和删除操作，在已经得到了需要新增加和删除的元素位置的前提下，linkedlist可以在o(1)的时间内删除和增加元素，而ArrayList需要移动增加或删除元素之后的所有元素的位置，时间复杂度是O(n)

  interface可以继承interface

  抽象类和接口：

  在很多情况下可以互换。Java是单继承，只能继承一个类，不能同时继承很多抽象类。

  

