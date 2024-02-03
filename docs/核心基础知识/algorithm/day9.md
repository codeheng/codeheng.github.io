---
comments: true
---

## [20.有效的括号](https://leetcode.cn/problems/valid-parentheses/)

> 括号匹配栈的经典问题

!!! Tips
    在匹配左括号时 **对应的右括号先入栈**，此时只需要比较当前元素和栈顶相等否

```c++ linenums="1"
bool isValid(string s) {
    stack<int> st;
    for (int i = 0; i < s.size(); i ++) {
        if (s[i] == '{') st.push('}');
        else if (s[i] == '(') st.push(')');
        else if (s[i] == '[') st.push(']');
        else if (st.empty() || st.top() != s[i]) return false;
        else st.pop();
    }
    return st.empty();
}
```

## [1047.删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)

利用栈完美解决 --> [参考动图](https://code-thinking.cdn.bcebos.com/gifs/1047.%E5%88%A0%E9%99%A4%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E6%89%80%E6%9C%89%E7%9B%B8%E9%82%BB%E9%87%8D%E5%A4%8D%E9%A1%B9.gif)

```c++ linenums="1"
string removeDuplicates(string s) {
    stack<char> st;
    for (auto i : s) {
        if (!st.empty() && i == st.top()) st.pop();
        else st.push(i);
    }
    s = "";
    while(!st.empty()) s += st.top(); st.pop();
    reverse(s.begin(), s.end());
    return s;
}
```

## [150.逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/description/)

> 逆波兰表达式：是一种后缀表达式，即二叉树的后序遍历  --> [动图参考](https://code-thinking.cdn.bcebos.com/gifs/150.%E9%80%86%E6%B3%A2%E5%85%B0%E8%A1%A8%E8%BE%BE%E5%BC%8F%E6%B1%82%E5%80%BC.gif)

```c++ linenums="1"
int evalRPN(vector<string>& tokens) {
    stack<int> st;
    for (auto s : tokens) {
        if (s == "+" || s == "-" || s == "*" || s == "/") {
            auto x2 = st.top(); st.pop();
            auto x1 = st.top(); st.pop();
            if (s == "+") st.push(x1 + x2);
            if (s == "-") st.push(x1 - x2);
            if (s == "*") st.push(x1 * x2);
            if (s == "/") st.push(x1 / x2);
        }
        else st.push(stoi(s)); // a string into an integer
    }
    return st.top();
}
```