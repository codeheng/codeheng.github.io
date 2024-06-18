---
comments: true
---

# 有序数组平方 & 长度最小子数组 & 螺旋矩阵

## [977.有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

第一想法: 对每个数进行平方，然后排序即可
```c++ linenums="1" hl_lines="5"
vector<int> sortedSquares(vector<int>& nums) {
    for (int i = 0; i < nums.size(); i ++) {
        nums[i] = nums[i] * nums[i];
    }
    sort(nums.begin(), nums.end());
    return nums;
}
```
时间复杂度: $O(n + nlogn)$

2.双指针 --> 因为有序，最大值必在两端，两边扫描比较，开个vector，空间换时间([动图](https://code-thinking.cdn.bcebos.com/gifs/977.%E6%9C%89%E5%BA%8F%E6%95%B0%E7%BB%84%E7%9A%84%E5%B9%B3%E6%96%B9.gif))
```c++ linenums="1"
vector<int> sortedSquares(vector<int>& nums) {
    vector<int> vec = nums;
    int i = 0, j = nums.size() - 1, k = nums.size() - 1;
    while (i <= j) {
        if (nums[i] * nums[i] < nums[j] * nums[j]) {
            vec[k --] = nums[j] * nums[j];
            j --;
        } else {
            vec[k --] = nums[i] * nums[i];
            i ++;
        }
    }
    return vec;
}
```

## [209.长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

> 描述: 正整数数组里面找 >= target的长度最小的连续子数组

1. 暴力，两次循环找所有情况，比较更新子数组长度和结果
```c++ linenums="1"
int minSubArrayLen(int target, vector<int>& nums) {
    int max = 0x3f3f3f3f, res = max;
    for (int i = 0; i < nums.size(); i ++) {
        int sum = 0;
        for (int j = i; j < nums.size(); j ++) {
            sum += nums[j];
            if (sum >= target) {
                int sublen = j - i + 1;// get子数组长度
                // res需要比较来进行更新，故需要初始res为一个大数
                res = res < sublen ? res : sublen;
                break;
            }
        }
    }
    return res == max ? 0 : res;
}
```
PS: 关于C++最大值的设置: [ref](https://jimmy-shen.medium.com/0x3f3f3f3f-an-interesting-number-for-inf-f4f0a0d24124)

!!! example "2. 滑动窗口"

    === "理解"
        也即双指针的一种，一个for循环降低复杂度。key: **起始位置i动态的移动**
        
        通过窗口不断调整子数组起始位置，使窗口内元素满足要求。[动图参考](https://code-thinking.cdn.bcebos.com/gifs/209.%E9%95%BF%E5%BA%A6%E6%9C%80%E5%B0%8F%E7%9A%84%E5%AD%90%E6%95%B0%E7%BB%84.gif)

    === "代码"
        ```c++ linenums="1" hl_lines="10"
        int minSubArrayLen(int target, vector<int>& nums) {
            int max = 0x3f3f3f3f, res = max, sum = 0;
            
            //i代表窗口的起始，j窗口的终止
            for(int i = 0, j = 0; j < nums.size(); j ++) {
                sum += nums[j];
                while (sum >= target) {
                    int sublen = j - i + 1;
                    res = res < sublen ? res : sublen;
                    sum -= nums[i ++];
                }
            }
            return res == max ? 0 : res;
        }
        ```


## [59.螺旋矩阵II](https://leetcode.cn/problems/spiral-matrix-ii/)

模拟填充的过程，循环遍历，注意特殊情况和区间边界

```c++ linenums="1"
vector<vector<int>> generateMatrix(int n) {
    vector<vector<int>> res(n, vector<int>(n));
    int r1 = 0, r2 = n - 1, c1 = 0, c2 = n - 1, val = 0;
    // 特殊处理n = 1
    if (n = 1) res[r1][c1] = 1;
    while (r1 <= r2 && c1 <= c2) {
        // left --> right (区间左闭右开进行处理)
        for (int i = c1; i < c2; i ++) res[r1][i] = ++ val;
        // top --> down
        for (int i = r1; i < r2; i ++) res[i][c2] = ++ val;
        // right --> left
        for (int i = c2; i > c1; i --) res[r2][i] = ++ val;
        // down --> top
        for (int i = r2; i > r1; i --) res[i][c1] = ++ val;

        r1 ++, c1 ++, r2 --, c2 --;
        // 填充中间的值
        if (r1 == r2 && c1 == c2) res[r1][c1] = ++ val;
    }
    return res;
}
```