---
comments: true
---

## [93.复原IP地址](https://leetcode.cn/problems/restore-ip-addresses/)

切割问题, [示意图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201123203735933.png)
```cpp linenums="1"
vector<string> res;
//判断字符串s在区间[l,r]组成的数字是否合法
bool isVaild(string s, int l, int r) {
    if(l > r) return false;
    if(s[l] == '0' && l != r) return false; // 以0开头不合法
    int num = 0;
    for(int i = l; i <= r; i ++) {
        if(s[i] > '9' || s[i] < '0') return false; //非正整数不合法
        num = num * 10 + (s[i] - '0'); 
        if(num > 255) return false; //对应数字>255不合法
    }
    return true;
}
void backtracking(string s, int idx, int pointNum) {
    //pointNum表示.数量，pointNum=3即字符串分成了4段
    if(pointNum == 3) {
        //判断第四段子字符串是否合法
        if(isVaild(s, idx, s.size() - 1)) res.push_back(s);
        return;
    }
    for(int i = idx; i < s.size(); i ++) {
        // 若[idx, i]的字串合法
        if(isVaild(s, idx, i)) {
            s.insert(s.begin() + i + 1, '.'); // 插入.
            pointNum ++;
            backtracking(s, i + 2, pointNum); // 因为插入.了故+2
            pointNum --;
            s.erase(s.begin() + i + 1); // 回溯删除.
        } else break;
    }
}
vector<string> restoreIpAddresses(string s) {
    backtracking(s, 0, 0);
    return res;
}
```

## [78.子集](https://leetcode.cn/problems/subsets/)

抽象为树形结构, [参考示意图](https://code-thinking.cdn.bcebos.com/pics/78.%E5%AD%90%E9%9B%86.png)
```cpp linenums="1"
vector<vector<int>> res;
vector<int> path;
void backtracking(vector<int>& nums, int idx) {
    res.push_back(path); // 可以为空集合,先收集
    if(idx >= nums.size()) return;
    for(int i = idx; i < nums.size(); i ++) {
        path.push_back(nums[i]);
        backtracking(nums, i + 1); // 元素不重复取,从i+1开始
        path.pop_back();
    }
}
vector<vector<int>> subsets(vector<int>& nums) {
    backtracking(nums, 0);
    return res;
}
```

## [90.子集II](https://leetcode.cn/problems/subsets-ii/)

> 和上题区别: 集合里有重复元素，而且求取的子集要去重

```cpp linenums="1" hl_lines="7"
vector<vector<int>> res;
vector<int> path;
void backtracking(vector<int>& nums, int idx) {
    res.push_back(path);
    if(idx >= nums.size()) return;
    for(int i = idx; i < nums.size(); i ++) {
        if(i > idx && nums[i] == nums[i - 1]) continue;
        path.push_back(nums[i]);
        backtracking(nums, i + 1);
        path.pop_back();
    }
}
vector<vector<int>> subsetsWithDup(vector<int>& nums) {
    sort(nums.begin(), nums.end()); // 去重需要排序
    backtracking(nums, 0);
    return res;
}
```

**子集问题 VS 组合问题、分割问题** : 

- 子集是收集树形结构中树的 ^^所有节点^^ 的结果
- 组合问题、分割问题是收集树形结构中 ^^叶子节点^^ 的结果