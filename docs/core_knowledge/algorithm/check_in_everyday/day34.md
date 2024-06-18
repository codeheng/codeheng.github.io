--- 
comments: true
---

## [343.整数拆分](https://leetcode.cn/problems/integer-break/) 

1. dp[i]: 拆分数字, 可以得到最大的乘积为dp[i]
2. 得到dp[i], 从1遍历j, 一是`j * (i - j)`,二是`j * dp[i - j]`
      
    - `dp[i] = max(dp[i], max( (i - j) * j, dp[i -j] * j))` 

3. 初始化: dp[2] = 1
4. 从前向后进行遍历
5. 举例n = 10, [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210104173021581.png)

```cpp linenums="1" hl_lines="7"
int integerBreak(int n) {
    vector<int> dp(n + 1);
    dp[2] = 1;
    for(int i = 3; i <= n; i ++)   
        for(int j = 1; j <= i; j ++)
    // j 其实最大值为 i-j,再大只不过是重复而已，
            dp[i] = max(dp[i], max((i - j) * j, dp[i - j] * j));
    //  j * (i - j) 是单纯的把整数 i 拆分为两个数, 也就是 i,i-j ，再相乘
    // 而j * dp[i - j]是将i拆分成两个以及两个以上的个数,再相乘
    return dp[n];
}
```

也可以贪心,每次拆成n个3，如果剩下是4，则保留4，然后相乘

```cpp linenums="1" hl_lines="6"
int integerBreak(int n) {
    if(n == 2) return 1;
    if(n == 3) return 2;
    if(n == 4) return 4;
    int res = 1;
    while(n > 4) res *= 3, n -= 3;
    res *= n;
    return res;
}
```

## [96.不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/) 

dp[3]，即1为头结点搜索树的数量 + 2为头结点搜索树的数量 + 3为头结点搜索树的数量

- 1为头结点搜索树的数量 = 右子树有2个元素的搜索树数量 * 左子树有0个元素的搜索树数量
- 2为头结点搜索树的数量 = 右子树有1个元素的搜索树数量 * 左子树有1个元素的搜索树数量
- 3为头结点搜索树的数量 = 右子树有0个元素的搜索树数量 * 左子树有2个元素的搜索树数量

即`dp[3] = dp[2] * dp[0] + dp[1] * dp[1] + dp[0] * dp[2]` [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210107093226241.png)

1. dp[i]: 1到i为节点组成的二叉搜索树的个数为dp[i]
2. dp[i] += dp[以j为头结点左子树节点数量] * dp[以j为头结点右子树节点数量]

    - j相当于是头结点的元素，从1遍历到i为止
    - `dp[i] += dp[j - 1] * dp[i - j];` 
  
        * j-1 为j为头结点左子树节点数量，i-j 为以j为头结点右子树节点数量

3. 初始化dp[0] = 1
4. 从前到后遍历, i-->n, j-->i
5. 举例n=5 --> [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210107093253987.png)

```cpp linenums="1"
int numTrees(int n) {
    vector<int> dp(n + 1);
    dp[0] = 1;
    for(int i = 1; i <= n; i ++)
        for(int j = 1; j <= i; j ++) dp[i] += dp[i - 1] * dp[i - j];
    return dp[n];
}
```



