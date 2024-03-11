---
comments: true
---

## [1049.最后一块石头的重量II](https://leetcode.cn/problems/last-stone-weight-ii/)

尽量让石头分成重量相同的两堆，相撞之后剩下的石头最小，化解成01背包问题
<br>物品的重量为`stones[i]`，物品的价值也为`stones[i]`

1. dp[j]表示重量为j的背包，最多可以背最大重量为dp[j]
2. 递推公式: `dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);`
3. 最大容量为: `30 * 100 / 2`, 初始均为0
4. 物品遍历的for循环放在外层，遍历背包的for循环放在内层，且内层for循环倒序遍历
5. 举例: [2,4,1,1] --> [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210121115805904.jpg)

```cpp linenums="1"
int lastStoneWeightII(vector<int>& stones) {
    vector<int> dp(30 * 100 + 1, 0);
    int sum = 0;
    for(int i = 0; i < stones.size(); i ++) sum += stones[i];
    int target = sum / 2;
    for(int i = 0; i < stones.size(); i ++)
        for(int j = target; j >= stones[i]; j --)
            dp[j] = max(dp[j], dp[j - stones[i]] + stones[i]);
    return (sum - dp[target]) - dp[target];
}
```
dp[target]里是容量为target的背包所能背的最大重量,一堆石头的总重量是`dp[target]`，另一堆就是`sum - dp[target]`, 因target = sum / 2 因为是向下取整，所以`sum - dp[target] >= dp[target]`

## [494.目标和](https://leetcode.cn/problems/target-sum/)

假设加法的总和为x，那么减法对应的总和就是sum - x。要求的是 x - (sum - x) = target, 则x = (target + sum) / 2, 此时转为 **装满容量为x的背包，有几种方法**, x即背包容量

1. dp[j]：填满j（包括j）这么大容积的包，有dp[j]种方法
2. 公式: `dp[j] += dp[j - nums[i]]` --> [举例参考](https://programmercarl.com/0494.%E7%9B%AE%E6%A0%87%E5%92%8C.html#%E6%80%9D%E8%B7%AF:~:text=i%5D%5D%20%E7%A7%8D%E6%96%B9%E6%B3%95%E3%80%82-,%E4%BE%8B%E5%A6%82,-%EF%BC%9Adp%5Bj%5D%EF%BC%8Cj)
3. 初始化为1
4. nums放在外循环，target在内循环，且内循环倒序
5. 举例nums:[1, 1, 1, 1, 1], target: 3, [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210125120743274.jpg) 

```cpp linenums="1"
int findTargetSumWays(vector<int>& nums, int target) {
    int sum = 0;
    for(int i = 0; i < nums.size(); i ++) sum += nums[i];
    if(abs(target) > sum || (target + sum) % 2) return 0;
    int bagSize = (target + sum) / 2;
    vector<int> dp(bagSize + 1, 0);
    dp[0] = 1;
    for (int i = 0; i < nums.size(); i++) 
        for (int j = bagSize; j >= nums[i]; j--) 
            dp[j] += dp[j - nums[i]];
    return dp[bagSize];
}
```

## [474.一和零](https://leetcode.cn/problems/ones-and-zeroes/)