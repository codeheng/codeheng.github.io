---
comments: true
---

## [454.四数相加 II](https://leetcode.cn/problems/4sum-ii/description/)

第一想法直接暴力，时间$O(n^4)$ --> TLE,只过了39/132

**利用哈希 --> ^^key:两个vector元素之和;value:对应出现的个数^^**

- 遍历`nums1`和`nums2` 统计里面的元素`i+j`出现的次数，放入map
- 再遍历`nums3`和`nums4`，对`-(k+l)`进行计数

```c++ linenums="1"
int fourSumCount(vector<int>& nums1, vector<int>& nums2, 
                 vector<int>& nums3, vector<int>& nums4) {
    int cnt = 0;
    unordered_map<int, int> map;
    for (int i : nums1) 
        for (int j : nums2) map[i + j] ++;
    for (int k : nums3) 
        for (int l : nums4) cnt += map[-(k + l)];
    return cnt;
}
```

## [383.赎金信](https://leetcode.cn/problems/ransom-note/)

1. 暴力做法 --> $O(n^2)$
```c++ linenums="1"
bool canConstruct(string ransomNote, string magazine) {
    for (int i = 0; i < magazine.length(); i ++) {
        for (int j = 0; j < ransomNote.length(); j ++) {
            if (magazine[i] == ransomNote[j]) {
                ransomNote.erase(ransomNote.begin() + j); // 去除重复的
                break;
            }
        }
    }
    if (ransomNote.length() == 0) 
        return true;
    return false;
}
```
2. 利用数组哈希 --> $O(n)$
```c++ linenums="1"
bool canConstruct(string ransomNote, string magazine) {
    int cnt[26] = {0};
    for (auto c : magazine) cnt[c - 'a'] ++;
    for (auto c : ransomNote)
        if ( --cnt[c - 'a'] < 0) return false;
    return true;
}
```

## [15.三数之和](https://leetcode.cn/problems/3sum/)

排序，利用双指针（注意去重） --> [动图参考](https://code-thinking.cdn.bcebos.com/gifs/15.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.gif)，（PS: [哈希解法](https://programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E6%80%9D%E8%B7%AF:~:text=%23-,%E5%93%88%E5%B8%8C%E8%A7%A3%E6%B3%95,-%E4%B8%A4%E5%B1%82for)）
```c++ linenums="1" hl_lines="12"
vector<vector<int>> threeSum(vector<int>& nums) {
    vector<vector<int>> res;
    sort(nums.begin(), nums.end());
    for (int i = 0; i < nums.size(); i ++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue; // 去重
        int l = i + 1, r = nums.size() - 1;
        while (l < r) {
            int sum = nums[i] + nums[l] + nums[r];
            if (sum < 0) l ++;
            else if (sum > 0) r --;
            else {
                res.push_back(vector<int>{nums[i], nums[l], nums[r]});
                while(l < r && nums[l] == nums[l + 1]) l ++;
                while(l < r && nums[r] == nums[r - 1]) r --;
                l ++, r --; 
            }
        }
    }
    return res;
}
```
## [18.四数之和](https://leetcode.cn/problems/4sum/)

在三数之和基础上外面再套个循环
```c++ linenums="1" hl_lines="11"
vector<vector<int>> fourSum(vector<int>& nums, int target) {
    vector<vector<int>> res;
    sort(nums.begin(), nums.end());
    for (int i = 0; i < nums.size(); i ++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue; // 去重
        for(int j = i + 1; j < nums.size(); j ++) {
            if (j > i + 1 && nums[j] == nums[j - 1]) continue;// 去重
            int l = j + 1, r = nums.size() - 1;
            while (l < r) {
                // 会溢出，需要转为long 
                auto sum = (long) nums[i] + nums[j] + nums[l] + nums[r];
                if (sum > target) r --;
                else if (sum < target) l ++;
                else {
                    res.push_back(vector<int>{nums[i], nums[j], nums[l], nums[r]});
                    while(l < r && nums[r] == nums[r - 1]) r --;
                    while(l < r && nums[l] == nums[l + 1]) l ++;
                    l ++, r --;
                }
            }
        }
    }
    return res;
}
```
