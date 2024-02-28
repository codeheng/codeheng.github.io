---
comments: true
---

## 理论基础 

> 贪心的本质是选择每一阶段的局部最优，从而达到全局最优

**贪心算法并没有固定的套路**。唯一的难点就是如何通过局部最优，推出整体最优

刷题时，手动模拟一下感觉可以局部最优推出整体最优，且想不到反例，则试一试贪心

**一般解题步骤**: 

- 将问题分解为若干个子问题
- 找出适合的贪心策略
- 求解每一个子问题的最优解
- 将局部最优解堆叠成全局最优解

**做题的时候，只要想清楚局部最优是什么，如何推导出全局最优，就够了**

## [455.分发饼干](https://leetcode.cn/problems/assign-cookies/)

局部最优: 大饼干喂给胃口大的; 全局最优喂饱尽可能多的小孩

- 排序,从后向前遍历小孩数组g，用大饼干s优先满足胃口大的，并统计满足小孩数量

```cpp linenums="1"
int findContentChildren(vector<int>& g, vector<int>& s) {
    sort(g.begin(), g.end());
    sort(s.begin(), s.end());
    int cnt = 0, idx = s.size() - 1;
    for(int i = g.size() - 1; i >= 0; i --) 
        if(idx >= 0 && s[idx] >= g[i]) cnt ++, idx --;
    return cnt;
}
```

## [376.摆动序列](https://leetcode.cn/problems/wiggle-subsequence/)

- 局部最优：删除单调坡度上的节点，那么这个坡度就可以有两个局部峰值
- 整体最优：整个序列有最多的局部峰值，从而达到最长摆动序列

PS: 不需要真正删除,计数即可(要求的是最长摆动子序列的长度)

**三种情况**: 上下坡中有平坡[ref](https://code-thinking-1253855093.file.myqcloud.com/pics/20230106172613.png), 数组首尾两端[ref](https://code-thinking-1253855093.file.myqcloud.com/pics/20201124174357612.png), 单调坡中有平坡[ref](https://code-thinking-1253855093.file.myqcloud.com/pics/20230108171505.png)

```cpp linenums="1" hl_lines="6"
int wiggleMaxLength(vector<int>& nums) {
    if(nums.size() <= 1) return nums.size();
    // cnt记录峰值个数，序列默认序列最右边有一个峰值
    int cnt = 1, cur = 0, prev = 0; //prev前一对差值, cur当前一对差值
    for(int i = 0; i < nums.size() - 1; i ++) {
        cur = nums[i + 1] - nums[i];
        if(prev >= 0 && cur < 0 || prev <= 0 && cur > 0) cnt ++, prev = cur;
    }
    return cnt;
}
```

也可以利用动态规划 --> [参考](https://programmercarl.com/0376.%E6%91%86%E5%8A%A8%E5%BA%8F%E5%88%97.html#%E6%80%9D%E8%B7%AF:~:text=%23-,%E6%80%9D%E8%B7%AF%202%EF%BC%88%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92%EF%BC%89,-%E8%80%83%E8%99%91%E7%94%A8%E5%8A%A8%E6%80%81)

## [53.最大子序和](https://leetcode.cn/problems/maximum-subarray/)

暴力: 第一层for设置起始位置，第二层for遍历数组寻找最大值

- 203/210 cases passed(N/A) 

```cpp linenums="1"
int maxSubArray(vector<int>& nums) {
    int res = INT32_MIN, cnt;
    for(int i = 0; i < nums.size(); i ++) {
        cnt = 0;
        for(int j = i; j < nums.size(); j ++) {
            cnt += nums[j];
            res = cnt > res ? cnt : res;
        }
    }
    return res;
}
```

**贪心** : 

- 局部最优：当前连续和为负数时立刻放弃，从下一个元素重新计算连续和
- 全局最优：选取最大连续和

**相当于是暴力解法中的不断调整最大子序和区间的起始位置** [示意图](https://code-thinking.cdn.bcebos.com/gifs/53.%E6%9C%80%E5%A4%A7%E5%AD%90%E5%BA%8F%E5%92%8C.gif)

```cpp linenums="1" hl_lines="5 6"
int maxSubArray(vector<int>& nums) {
    int res = INT32_MIN, cnt = 0;
    for(int i = 0; i < nums.size(); i ++) {
        cnt += nums[i];
        if(cnt > res) res = cnt;// 取区间累计的最大值
        if(cnt <= 0) cnt = 0; // 重置最大子序起始位置
    }
    return res;
}
```