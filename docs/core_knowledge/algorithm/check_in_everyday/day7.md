---
comments: true
--- 

## [344.反转字符串](https://leetcode.cn/problems/reverse-string/)

1. 直接调用库函数来进行反转
```c++ linenums="1"
void reverseString(vector<char>& s) {
    reverse(s.begin(), s.end());
}
```
PS: **如果题目关键部分直接用库函数可以实现，那么尽量别用**

1. 双指针，前后字符进行交换
```c++ linenums="1"
void reverseString(vector<char>& s) {
    for (int i = 0, j = s.size() - 1; i < s.size() / 2; i ++, j --) 
        swap(s[i], s[j]);
}
``` 

## [541.反转字符串II](https://leetcode.cn/problems/reverse-string-ii/)

直接进行模拟即可，利用reverse反转符合要求的

> 当需要固定规律一段一段去处理字符串的时候，想想在for循环上可实现否?

```c++ linenums="1" hl_lines="2"
string reverseStr(string s, int k) {
    for(int i = 0; i < s.size(); i += 2 * k) {
        if (i + k > s.size()) reverse(s.begin() + i , s.end());
        else reverse(s.begin() + i, s.begin() + i + k);
    }
    return s;
}
```

## [151.反转字符串里的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

要求原地$O(1)$空间复杂度

- 整体反转，再单词再反转，空格怎么办? --> 直接用不是空格的字符进行填充
- [图片参考](https://assets.leetcode.com/users/images/b62b1a27-3688-41eb-b294-c26a5ba11d19_1634751350.7162225.png)

```c++ linenums="1"
string reverseWords(string s) {
    reverse(s.begin(), s.end());
    int l = 0, r = 0, i = 0;
    while( i < s.size() ) {
        while( i < s.size() && s[i] == ' ' ) i ++;
        while( i < s.size() && s[i] != ' ' ) s[r ++] = s[i ++];
        if (l < r) {
            reverse(s.begin() + l, s.begin() + r);
            s[r ++] = ' ';
            l = r;
        }
    }
    s.resize(r - 1);
    return 0;
}
```