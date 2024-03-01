---
comments: true
---

## [1005.K次取反后最大化的数组和](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)

局部最优：让绝对值大的负数变为正数，当前数值达到最大，整体最优：整个数组和达到最大

若都变为正数，K仍>0，即 ^^一个有序正整数序列，如何转变K次正负，让数组和达到最大?^^ 贪心 

1. 将数组按照绝对值大小从大到小排序
2. 从前向后遍历，遇到负数将其变为正数，同时K--
3. 若K还大于0，那么反复转变数值最小的元素，将K用完, 之后求和

```cpp linenums="1"
class Solution {
static bool cmp(int a, int b) { return abs(a) > abs(b); }
public:
    int largestSumAfterKNegations(vector<int>& nums, int k) {
        sort(nums.begin(), nums.end(), cmp); //根据绝对值从大到小
        for(int i = 0; i < nums.size(); i ++)   
            if(k > 0 && nums[i] < 0) nums[i] *= -1, k --;
        if(k % 2) nums[nums.size() - 1] *= -1;
        int sum = 0;
        for(auto i : nums) sum += i;
        return sum;
    }
};
```

## [134.加油站](https://leetcode.cn/problems/gas-station/)

暴力: 若跑一圈，中途没有断油，而且最后油量>=0，则是ok的

- 35/40 cases pased(N/A)

```cpp linenums="1"
int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    for(int i = 0; i < cost.size(); i ++) {
        int rest = gas[i] - cost[i]; //剩余油量
        int idx = (i + 1) % cost.size();
        while(rest > 0 && idx != i) {
            rest += gas[idx] - cost[idx];
            idx = (idx + 1) % cost.size();
        }
        if(rest >= 0 && idx == i) return i;
    }
    return -1;
}
```

贪心: 若总油量-总消耗>=0,则一定可以跑完一圈，即各个站点的剩油量`rest[i]`相加>=0

`i`从0开始累加`rest[i]`，和为`curSum`，一旦`curSum`< 0，说明`[0,i]`区间都不能作为起始位置，因为这个区间选择任何一个位置作为起点，到`i`这里都会断油，那么起始位置从`i+1`算起，再从0计算`curSum`  --> [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20230117165628.png)

- 局部最优：当前累加`rest[i]`的和`curSum`一旦 < 0，起始位置至少是`i+1`
- 全局最优：找到可以跑一圈的起始位置

```cpp linenums="1"
int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    int res = 0, curSum = 0, sum = 0;
    for(int i = 0; i < gas.size(); i ++) {
        curSum += gas[i] - cost[i];
        sum += gas[i] - cost[i];
        if(curSum < 0) curSum = 0, res = i + 1;
    }
    if(sum < 0) return -1;
    return res;
}
```

## [135.分发糖果](https://leetcode.cn/problems/candy/)

确定一边之后，再确定另一边

- 局部最优：只要右边评分比左边大，右边的孩子就多一个糖果
- 全局最优：相邻的孩子中，评分高的右孩子获得比左边孩子更多的糖果

先确定右边评分>左边的情况(从前向后遍历), 再确定左孩子>右孩子的情况(从后向前遍历)

```cpp linenums="1"
int candy(vector<int>& ratings) {
    vector<int> res(ratings.size(), 1);
    for(int i = 1; i < ratings.size(); i ++) // 前->后
        if(ratings[i] > ratings[i - 1]) res[i] = res[i - 1] + 1;
    for(int i = ratings.size() - 2; i >= 0; i --) // 后->前
        if(ratings[i] > ratings[i + 1]) res[i] = max(res[i], res[i + 1] + 1);
    int sum = 0;
    for(int i = 0; i < res.size(); i ++) sum += res[i];
    return sum;
}
```