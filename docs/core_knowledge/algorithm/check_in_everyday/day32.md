---
comments: true
---

## 动态规划介绍

> 动规(Dynamic Programming即DP), 若某一问题有很多重叠子问题，使用DP是最有效的

DP中 **每一个状态一定是由上一个状态推导出来的** (区分于贪心) <br>贪心没有状态推导，而是从局部直接选最优的

### 解题步骤

1. 确定dp数组（dp table）以及下标的含义
2. 确定递推公式
3. dp数组如何初始化
4. 确定遍历的顺序
5. 举例推导dp数组

若出问题: 最好方式就是把dp数组打印出来，看看是不是按照自己思路推导的！

- 写代码之前一定要把状态转移在dp数组的上具体情况模拟一遍


## [509.斐波那契数](https://leetcode.cn/problems/fibonacci-number/)

当然可以直接用递归: 
```cpp linenums="1"
int fib(int n) {
    if(n < 2) return n;
    else return fib(n - 1) + fib(n - 2);
}
```

**动规**: 

1. `dp[i]`的定义为：第`i`个数的斐波那契数值是`dp[i]`
2. `dp[i] = dp[i - 1] + dp[i - 2]` (递推公式给出)
3. 初始化: `dp[0] = 0, dp[1] = 1;`
4. 遍历顺序: 从前到后
5. 具体推导: `0 1 1 2 3 5 8 13 21 34 55...`

```cpp linenums="1"
int fib(int n) {
    if(n < 2) return n;
    vector<int> dp(n + 1);
    dp[0] = 0, dp[1] = 1;
    for(int i = 2; i <= n; i ++) dp[i] = dp[i - 1] + dp[i - 2];
    return dp[n];
}
```

但其实不需要维护整个`vector`, 仅仅维护两个值即可
``` cpp linenums="1"
int fib(int n) {
    if(n < 2) return n;
    int dp[2] = {0, 1};
    for(int i = 2; i <= n; i ++) {
        int sum = dp[0] + dp[1];
        dp[0] = dp[1], dp[1] = sum;
    }
    return dp[1];
}
```

## [70.爬楼梯](https://leetcode.cn/problems/climbing-stairs/)

1. dp[i]： 爬到第i层楼梯，有dp[i]种方法
2. `dp[i] = dp[i - 1] + dp[i - 2]`
3. 初始化: `dp[1] = 1, dp[2] = 2;` n > 0, 不考虑n = 0的情况
4. 从前到后进行遍历
5. 举例若n = 5 : `1 2 3 5 8...` 不就是fibonacci([如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210105202546299.png))

```cpp linenums="1" hl_lines="5"
int climbStairs(int n) {
    if(n < 2) return n;
    vector<int> dp(n + 1);
    dp[1] = 1, dp[2] = 2;
    for(int i = 3; i <= n; i ++) dp[i] = dp[i - 1] + dp[i - 2];
    return dp[n];
}
```
空间优化, 和509T同理

## [746.使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/)

1. dp[i]到达第i个台阶花费的体力dp[i]
2. 两个途径得到dp[i], 从dp[i - 1] 或dp[i - 2]分别加上cost, 选两者小的
      - `dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);`  
3. 初始化: `dp[0] = 0, dp[1] = 0;`
4. 遍历顺序: 从前到后
5. 举例: [参考图](https://code-thinking-1253855093.file.myqcloud.com/pics/20221026175104.png)

```cpp linenums="1" hl_lines="5"
int minCostClimbingStairs(vector<int>& cost) {
    vector<int> dp(cost.size() + 1);
    dp[0] = 0, dp[1] = 0; // 默认第一步都是不花费体力的
    for(int i = 2; i <= cost.size(); i ++) 
        dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
    return dp[cost.size()];
}
```