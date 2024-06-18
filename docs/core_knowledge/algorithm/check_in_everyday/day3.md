---
comments: true
---

# 移除链表元素 & 设计链表 & 反转链表


!!! Abstract
    掌握画图进行分析以及常见的题型方法

**链表的定义:**
```c++ linenums="1"
struct ListNode {
    int val;
    ListNode *next;
    ListNode(int x) : val(x), next(nullptr) {} // 构造函数
}
```

## [203.移除链表元素](https://leetcode.cn/problems/remove-linked-list-elements/)

1. 两种情况: case1删除的是头，case2删除的非头
```c++ linenums="1"
ListNode* removeElements(ListNode* head, int val) {
    // case1
    while (head != nullptr && head->val == val) {
        ListNode* tmp = head;
        head = head->next;
        delete tmp;
    }
    // case2
    ListNode* cur = head;
    while (cur != nullptr && cur->next != nullptr) {
        if (cur->next->val == val) {
            ListNode* tmp = cur->next;
            cur->next = cur->next->next;
            delete tmp;
        } else {
            cur = cur->next;
        }
    }
    return head;
}
```
2. 利用 ==虚拟头节点dummy== 统一操作
```c++ linenums="1" hl_lines="2"
ListNode* removeElements(ListNode* head, int val) {
    ListNode* dummy = new ListNode(0);
    dummy->next = head;
    ListNode* cur = dummy;
    while (cur->next != nullptr) {
        if (cur->next->val == val) {
            ListNode* tmp = cur->next;
            cur->next = tmp->next;
            delete tmp;
        } else {
            cur = cur->next;
        }
    }
    head = dummy->next;
    delete dummy;
    return head;
}
```
## [707.设计链表](https://leetcode.cn/problems/design-linked-list/)

!!! Note "单链表的增删"

    利用虚拟头节点 --> [Ref](https://programmercarl.com/0707.%E8%AE%BE%E8%AE%A1%E9%93%BE%E8%A1%A8.html#%E6%80%9D%E8%B7%AF:~:text=%E6%9D%A5%E8%BF%9B%E8%A1%8C%E6%93%8D%E4%BD%9C%E3%80%82-,%E8%AE%BE%E7%BD%AE%E4%B8%80%E4%B8%AA%E8%99%9A%E6%8B%9F%E5%A4%B4%E7%BB%93%E7%82%B9%E5%9C%A8%E8%BF%9B%E8%A1%8C%E6%93%8D%E4%BD%9C,-%E3%80%82)

## [206.反转链表](https://leetcode.cn/problems/reverse-linked-list/description/)

每次维护相邻的指针，完成反向，继续向后走
```c++ linenums="1"
ListNode* reverseList(ListNode* head) {
    if (!head || !head->next) return head;s
    ListNode* pre = nullptr, *cur = head;
    while(cur) {
        ListNode* next = cur->next;
        cur->next = pre;
        pre = cur;
        cur = next;
    }
    return pre;
}
```

