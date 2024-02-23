---
comments: true
---

## [39.组合总和](https://leetcode.cn/problems/combination-sum/)

抽象成树形结构,[如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201223170730367.png)
```cpp linenums="1"
vector<vector<int>> res;
vector<int> path;
void backtracking(vector<int>& candidates, int target, int sum, int idx) {
    if(sum > target) return;
    if(sum == target) {
        res.push_back(path);
        return;
    }
    for(int i = idx; i < candidates.size(); i ++) { 
        sum += candidates[i];
        path.push_back(candidates[i]);
        backtracking(candidates, target, sum, i); // i不需要+1了
        sum -= candidates[i];
        path.pop_back();
    }
}
vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
    backtracking(candidates, target, 0, 0);
    return res;
}
```

## [40.组合总和II](https://leetcode.cn/problems/combination-sum-ii/)

> 与上题,vector中有重复,但不能有重复的组合,即要去重

```cpp linenums="1"
vector<vector<int>> res;
vector<int> path;
void backtracking(vector<int>& candidates, int target, int sum, int idx) {
    if(sum > target) return;
    if(sum == target) {
        res.push_back(path);
        return;
    }
    for(int i = idx; i < candidates.size() && sum + candidates[i] <= target; i ++) {
        if(i > idx && candidates[i] == candidates[i - 1]) continue;  // 去重
        sum += candidates[i];
        path.push_back(candidates[i]);
        backtracking(candidates, target, sum, i + 1);
        sum -= candidates[i];
        path.pop_back();
    }
}
vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
    sort(candidates.begin(), candidates.end()); // 需要排序,相同元素挨在一起
    backtracking(candidates, target, 0, 0);
    return res;
}
```

## [131.分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)

```cpp linenums="1"

```