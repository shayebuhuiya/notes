### LRU缓存

- 使用双向链表实现，需要会用C++自己写一个双向链表，链表中存放K-V，双向链表便于大量的插入删除、移动位置的操作；

- 把移动到队列头部操作，拆分成去掉尾部、插入头部两个操作。

- #### [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)

```cpp
class LRUCache {
struct DLinkedNode{
    int key,value;
    DLinkedNode* prev;
    DLinkedNode* next;
    DLinkedNode():key(0),value(0),prev(nullptr),next(nullptr){}
    DLinkedNode(int _key,int _value):key(_key),value(_value),prev(nullptr),next(nullptr){}
};

private:
    unordered_map<int,DLinkedNode*> node_map;
    DLinkedNode* head;
    DLinkedNode* tail;
    int size;
    int capacity;
    
public:
    LRUCache(int _capacity) {
        capacity = _capacity;
        size = 0;
        head = new DLinkedNode();
        tail = new DLinkedNode();
        head->next = tail;
        tail->prev = head;
    }

    void removeNode(DLinkedNode *node) {
        node->prev->next = node->next;
        node->next->prev = node->prev;
    }

    void addToHead(DLinkedNode* node) {
        node->prev = head;
        node->next = head->next;
        head->next->prev = node;
        head->next = node;
    }
    
    void moveToHead(DLinkedNode* node) {
        removeNode(node);
        addToHead(node);
    }
    
    int get(int key) {
        if(node_map.count(key) == 0) {
            return -1;
        }
        moveToHead(node_map[key]);
        return node_map[key]->value;
    }
    
    void put(int key, int value) {
        if(node_map.count(key) != 0) {
            DLinkedNode *node = node_map[key];
            node->value = value;
            moveToHead(node);
        } else {
            DLinkedNode* node = new DLinkedNode(key, value);
            node_map[key] = node;
            addToHead(node);
            size++;
            if(size > capacity) {
                DLinkedNode* remove_node = tail->prev;
                removeNode(remove_node);

                size--;
                node_map.erase(remove_node->key);
                delete remove_node;
            }
        }
    }
};
```

