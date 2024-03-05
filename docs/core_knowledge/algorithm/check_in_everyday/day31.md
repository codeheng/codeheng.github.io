---
comments: true
---

## [738.单调递增的数字](https://leetcode.cn/problems/monotone-increasing-digits/)

暴力: TLE --> 249/308 cases passed(N/A)
``` cpp linenums="1"
bool check(int num) { // 看num是否为递增
    if(num < 10) return true;
    int max = 10;
    while(num) {
        int t = num % 10;
        if(max >= t) max = t;
        else return false;
        num /= 10;
    }
    return true;
}
int monotoneIncreasingDigits(int n) {
    for(int i = n; i > 0; i --) 
        if(check(i)) return i;
    return 0;
}
```


比如98先转为字符串，若出现`strNum[i - 1] > strNum[i]`的情况（非单调递增）

- 先让`strNum[i - 1]--`，然后`strNum[i]`给为9，整数即89
- 即小于98的最大的单调递增整数为89

```cpp linenums="1" hl_lines="6"
int monotoneIncreasingDigits(int n) {
    string str = to_string(n); // int --> string
    int flag = str.size(); // flag用来标记赋值9从哪里开始
    for(int i = str.size() - 1; i > 0; i--)
        if(str[i - 1] > str[i]) // 若不是逆序了
            flag = i, str[i - 1] --;
    for(int i = flag; i < str.size(); i ++) 
        str[i] = '9';
    return stoi(str);
}
```

## [968.监控二叉树](https://leetcode.cn/problems/binary-tree-cameras/)

贪心和二叉树的一个结合 (难) --> [参考](https://programmercarl.com/0968.%E7%9B%91%E6%8E%A7%E4%BA%8C%E5%8F%89%E6%A0%91.html#%E6%80%9D%E8%B7%AF)