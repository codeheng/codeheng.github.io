---
comments: true
---


## [491.递增子序列](https://leetcode.cn/problems/non-decreasing-subsequences/) 

求自增子序列，不能对原数组进行排序的，因为排完序都是自增子序列了(利用set去重) [示意图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201124200229824-20230310131640070.png)

```cpp linenums="1" hl_lines="7 8"
vector<vector<int>> res;
vector<int> path;
void backtracking(vector<int>& nums, int idx) {
    if(path.size() > 1) res.push_back(path);// 不return，要取树上的所有节点
    unordered_set<int> set; // 去重
    for(int i = idx; i < nums.size(); i ++) {
        if(!path.empty() && nums[i] < path.back() 
                        || set.find(nums[i]) != set.end()) continue;
        set.insert(nums[i]); //记录此元素在本层用过了，后面层不能用了
        path.push_back(nums[i]);
        backtracking(nums, i + 1);
        path.pop_back();
    }
}
vector<vector<int>> findSubsequences(vector<int>& nums) {
    backtracking(nums, 0);
    return res;
}
```

因为数据范围[-100, 100], 可直接使用数组去重
```cpp linenums="1" hl_lines="5"
vector<vector<int>> res;
vector<int> path;
void backtracking(vector<int>& nums, int idx) {
    if(path.size() > 1) res.push_back(path);
    int used[201] = { 0 }; // 去重
    for(int i = idx; i < nums.size(); i ++) {
        if(!path.empty() && nums[i] < path.back() 
                        || used[nums[i] + 100] == 1) continue;
        used[nums[i] + 100] = 1;
        path.push_back(nums[i]);
        backtracking(nums, i + 1);
        path.pop_back();
    }
}
vector<vector<int>> findSubsequences(vector<int>& nums) {
    backtracking(nums, 0);
    return res;
}
```

## [46.全排列](https://leetcode.cn/problems/permutations/)

排列是有序的,即[1,2]和[2,1]两个集合. 此时需要used数组，来标记已经选择的元素

- 因为从头搜索,则不需要idx参数了, [参考图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201209174225145.png)

```cpp linenums="1"
vector<vector<int>> res;
vector<int> path;
void backtracking(vector<int>& nums, vector<bool>& used) {
    if(path.size() == nums.size()) {
        res.push_back(path);
        return;
    }
    for(int i = 0; i < nums.size(); i ++) {
        if(used[i] == true) continue;
        used[i] = true;
        path.push_back(nums[i]);
        backtracking(nums, used);
        path.pop_back();
        used[i] = false;
    }
}
vector<vector<int>> permute(vector<int>& nums) {
    vector<bool> used(nums.size(), false);
    backtracking(nums, used);
    return res;
}
```

## [47.全排列II](https://leetcode.cn/problems/permutations-ii/)

> 区别: 给定一个可包含 **重复数字** 的序列，要返回所有不重复的全排列, 即去重

```cpp linenums="1" hl_lines="9"
vector<vector<int>> res;
vector<int> path;
void backtracking(vector<int>& nums, vector<bool>& used) {
    if(path.size() == nums.size()) {
        res.push_back(path);
        return;
    }
    for(int i = 0; i < nums.size(); i ++) {
        if(i > 0 && nums[i] == nums[i - 1] && used[i - 1] == true) continue;
        if(used[i] == false) {
            used[i] = true;
            path.push_back(nums[i]);
            backtracking(nums, used);
            path.pop_back();
            used[i] = false;
        }
    }
}
vector<vector<int>> permuteUnique(vector<int>& nums) {
    sort(nums.begin(), nums.end()); // 排序
    vector<bool> used(nums.size(), false);
    backtracking(nums, used);
    return res;
}
```

如果要对树层中前一位去重，用`used[i - 1] == false`

如果要对树枝前一位去重,用`used[i - 1] == true`

对于排列问题，树层上去重和树枝上去重，都是可以的，但是树层上去重效率更高

- 参考[示例图树层上](https://code-thinking-1253855093.file.myqcloud.com/pics/20201124201406192.png) 和 [示例图树枝上](https://code-thinking-1253855093.file.myqcloud.com/pics/20201124201431571.png)