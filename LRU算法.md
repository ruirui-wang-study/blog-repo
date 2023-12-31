## LRU算法
### 介绍

LRU（Least Recently Used）最少最近使用算法，基本思想：当缓存空间已满时，优先淘汰最近最少使用的缓存数据，以腾出更多的缓存空间。
### 实现

使用一个双向链表和一个哈希表。双向链表用于维护缓存数据的访问顺序，哈希表用于快速查找缓存数据。具体来说，当新的数据被访问时，先在哈希表中查找该数据是否已经存在于缓存中，如果存在，则将该数据移动到双向链表的头部，表示该数据是最近访问的数据；如果不存在，则需要将该数据添加到缓存中，并将其添加到双向链表的头部。当缓存空间已满时，需要淘汰双向链表中最后一个节点，同时在哈希表中删除对应的缓存数据。
- 代码

- 结构定义

- 双向链表
```
  class Node<K, V> {
    K key;
    V value;
    Node<K, V> prev;
    Node<K, V> next;
    public Node(K key, V value) {
        this.key = key;
        this.value = value;
    }}
```
- LRU缓存类
```
  public class LRUCache<K, V> {
    private int capacity;  // 缓存容量
    private Map<K, Node<K, V>> map;  // 哈希表
    private Node<K, V> head;  // 双向链表头节点
    private Node<K, V> tail;  // 双向链表尾节点
    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.map = new HashMap<>();           // 初始化双向链表
        this.head = new Node<>(null, null);
        this.tail = new Node<>(null, null);
      this.head.next = this.tail;
      this.tail.prev = this.head;
    }
    /** * 添加数据到缓存中 */
    public void put(K key, V value) {
        Node<K, V> node = map.get(key);
        if (node != null) {
            // 如果已经存在该节点，则将其移动到链表头部
            removeNode(node);
        } else {
            // 如果不存在该节点，则需要添加新节点
            node = new Node<>(key, value);
            map.put(key, node);
            if (map.size() > capacity) {
                // 如果缓存已满，则需要淘汰最近最少使用的节点
                Node<K, V> removedNode = removeLast();
                map.remove(removedNode.key);
            }
        }    // 将新节点添加到链表头部
        addFirst(node);
    }
    /** * 从缓存中获取数据 */
    public V get(K key) {
        Node<K, V> node = map.get(key);
        if (node != null) {
            // 如果存在该节点，则将其移动到链表头部，并返回其值
            removeNode(node);
            addFirst(node);
            return node.value;
        }
        return null;
    }
    /** * 获取当前缓存的大小 */
    public int size() {
        return map.size();
    }
    /** * 清空缓存 */
    public void clear() {
        map.clear();
        head.next = tail;
        tail.prev = head;
    }
    /** * 从双向链表中移除指定节点 */
    private void removeNode(Node<K, V> node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
    /** * 将节点添加到双向链表头部 */
    private void addFirst(Node<K, V> node) {
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }
    /** * 从双向链表尾部移除节点，并返回该节点 */
    private Node<K, V> removeLast() {
        Node<K, V> node = tail.prev;
        if (node == head) {
            return null;
        }
        removeNode(node);
        return node;
    }                                                
  }
```
- LRU算法实现
```
  public class LRUTest {
    public static void main(String[] args) {
        LRUCache<Integer, String> cache = new LRUCache<>(3);
        cache.put(1, "a");
        cache.put(2, "b");
        cache.put(3, "c");
        System.out.println(cache.get(1)); // 输出a
        cache.put(4, "d");
        System.out.println(cache.get(2)); // 输出null
        System.out.println(cache.get(1)); // 输出a
        cache.put(5, "e");
        cache.clear();
        System.out.println(cache.size()); // 输出0
    }}
```




















