---
comments: true
---

## [122.买卖股票的最佳时机II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

只有一只股票 & 只有买或卖 --> 把利润分为每天为单位的维度, 收集每天的正利润即可, [参考图](https://code-thinking-1253855093.file.myqcloud.com/pics/2020112917480858-20230310134659477.png)

- 局部最优：收集每天的正利润; 全局最优：求得最大利润

```cpp linenums="1" hl_lines="4"
int maxProfit(vector<int>& prices) {
    int res = 0;
    for(int i = 1; i < prices.size(); i ++)
        res += max(prices[i] - prices[i - 1], 0);
    return res;
}
```
也可以利用[动态规划](https://programmercarl.com/0122.%E4%B9%B0%E5%8D%96%E8%82%A1%E7%A5%A8%E7%9A%84%E6%9C%80%E4%BD%B3%E6%97%B6%E6%9C%BAII.html#%E6%80%9D%E8%B7%AF:~:text=%23-,%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92,-%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%E5%B0%86)

## [55.跳跃游戏](https://leetcode.cn/problems/jump-game/)

跳几步无所谓, 关键是可跳的覆盖范围 --> 转化为跳跃覆盖范围究竟可不可以覆盖到终点

- 每次移动取最大跳跃步数(得到最大的覆盖范围)，每移动一个单位，就更新最大覆盖范围 [参考](https://code-thinking-1253855093.file.myqcloud.com/pics/20230203105634.png)

```cpp linenums="1" hl_lines="5"
bool canJump(vector<int>& nums) {
    if(nums.size() == 1) return true;
    int dis = 0;
    for(int i = 0; i <= dis; i ++) {
        dis = max(dis, i + nums[i]); 
        if(dis >= nums.size() - 1) return true;
    }
    return false;
}
```

## [45.跳跃游戏II](https://leetcode.cn/problems/jump-game-ii/)

要计算最少步数，什么时候步数才一定要加一呢？

- 局部最优：当前可移动距离尽可能多走，如果还没到终点，步数再加一
- 整体最优：一步尽可能多走，从而达到最少步数

从覆盖范围出发，不管怎么跳，覆盖范围内一定是可以跳到的，**以最小的步数增加覆盖范围，覆盖范围一旦覆盖了终点，得到的就是最少步数**。需要统计两个覆盖范围，当前这一步的最大覆盖和下一步最大覆盖， [图示](https://code-thinking-1253855093.file.myqcloud.com/pics/20201201232309103.png)

```cpp linenums="1" hl_lines="5"
int jump(vector<int>& nums) {
    if(nums.size() == 1) return 0;
    //curDis当前覆盖的最远距离下标, nextDis下一步..
    int cnt = 0, curDis = 0, nextDis = 0;
    for(int i = 0; i < nums.size() - 1; i ++) {
        nextDis = max(nextDis, i + nums[i]);
        if(i == curDis) curDis = nextDis, cnt ++;
    }
    return cnt;
}
```

其精髓在于控制移动下标`i`只移动到`nums.size() - 2`的位置，所以移动下标只要遇到当前覆盖最远距离的下标，直接步数加一 [参考](https://programmercarl.com/0045.%E8%B7%B3%E8%B7%83%E6%B8%B8%E6%88%8FII.html#%E6%80%9D%E8%B7%AF:~:text=%E5%9B%A0%E4%B8%BA%E5%BD%93%E7%A7%BB%E5%8A%A8%E4%B8%8B%E6%A0%87%E6%8C%87%E5%90%91%20nums.size%20%2D%202%20%E6%97%B6%EF%BC%9A)