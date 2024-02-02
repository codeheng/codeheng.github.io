---
comments: true
---

## 理论

> 栈是先进后出,队列先进先出

栈是以底层容器完成其工作，对外提供接口，并且可以控制使用哪种容器来实现栈的功能

- STL中栈不被归类为容器，而被归类为container adapter（容器适配器）
- 栈的底层实现可以是`vector,deque,list`都是可以的

常用的[SGI STL](https://blog.csdn.net/guowenyan001/article/details/11480141)，如果没有指定的话，默认是以`deque`为栈/队列的底层结构。

## [232.用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/)

两个栈进行模拟，一个输入栈，一个输出栈
```c++ linenums="1"
stack<int> input, output;
void push(x) {
    input.push(x);
}
int pop() {
    int res = peek();
    output.pop();
    return res;
}
int peek() { // 返回队列开头的元素
    if (output.empty()) {
        while(!input.empty()) output.push(input.top()), input.pop();
    }
    return output.top(); 
}
bool empty() {
    return input.empty() && output.empty();
}
```

## [225.用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/)

用一个队列实现，每次push完若队中元素>1,则队头元素再push进队，并pop队头
```c++ linenums="1"
queue<int> q;
void push(x) {
    q.push(x);
    for (int i = 0; i < q.size() - 1; i ++) 
        q.push(q.front()), q.pop();
}
int pop() {
    int res = top();
    q.pop();
    return res;
}
int top() {
    return q.front();
}
int empty() {
    return q.empty();
}
```