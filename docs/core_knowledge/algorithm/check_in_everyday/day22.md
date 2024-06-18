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

```cpp linenums="1" hl_lines="10"
vector<vector<int>> res;
vector<int> path;
void backtracking(vector<int>& candidates, int target, int sum, int idx) {
    if(sum > target) return;
    if(sum == target) {
        res.push_back(path);
        return;
    }
    for(int i = idx; i < candidates.size(); i ++) {
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

树形结构示意图 --> [参考](https://code-thinking.cdn.bcebos.com/pics/131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.jpg)
```cpp linenums="1"
vector<vector<string>> res;
vector<string> path;
bool help(string s, int l, int r) { //判断[l,r]是回文吗
    for(int i = l, j = r; i < j; i ++, j --)
        if(s[i] != s[j]) return false;
    return true;
}
void backtracking(string s, int idx) {
    if(idx >= s.size()) {
        res.push_back(path);
        return;
    }
    for(int i = idx; i < s.size(); i ++) {
        if(help(s, idx, i)) { // 判断是回文吗? 获取[idx, i]在s中的字串
            string str = s.substr(idx, i - idx + 1); //第二个参数是长度
            path.push_back(str);
        } else continue;
        backtracking(s, i + 1);
        path.pop_back();
    }
}
vector<vector<string>> partition(string s) {
    backtracking(s, 0);
    return res;
}
```