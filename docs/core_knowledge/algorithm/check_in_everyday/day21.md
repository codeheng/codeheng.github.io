---
comments: true
---

## [216.组合总和II](https://leetcode.cn/problems/combination-sum-iii/)


相比[77.组合](./day20.md/#77),加了元素总和的限制。选取过程[示意图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201123195717975.png)
```cpp linenums="1"
vector<vector<int>> res;
vector<int> path;
void backtracking(int k, int n, int idx, int sum) {
    if (sum > n) return; // 剪枝
    if(path.size() == k) {
        if(sum == n) res.push_back(path);
        return;
    }
    for(int i = idx; i <= 9; i ++) {
        sum += i;
        path.push_back(i);
        backtracking(k, n, i + 1, sum);
        sun -= i; // 回溯
        path.pop_back(i);
    }
}
vector<vector<int>> combinationSum3(int k, int n) {
    backtracking(k, n, 1, 0);
    return res;
}
```

当然, sum参数可以不要, 而是通过n减去每次值看最终是否为0
```cpp linenums="1" hl_lines="11"
vector<vector<int>> res;
vector<int> path;
void backtracking(int k, int n, int idx) {
    if(n < 0 || path.size() > k) return;
    if(n == 0 && path.size() == k) {
        res.push_back(path);
        return;
    }
    for(int i = idx; i <= 9; i ++) {
        path.push_back(i);
        backtracking(k, n - i, i + 1);
        path.pop_back();
    }
}
vector<vector<int>> combinationSum3(int k, int n) {
    backtracking(k, n, 1);
    return res;    
}
```

## [17.电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)

要有数字到字符串的映射关系, 用数组或vector --> [示意图](https://code-thinking-1253855093.file.myqcloud.com/pics/20201123200304469.png)
```cpp linenums="1"
vector<string> digit_map = {
    "", "", "abc", "def", "ghi", 
    "jkl","mno", "pqrs", "tuv", "wxyz",
};
void backtracking(string digits,vector<string>& res, int idx, string &s) {
    if(digits.size() == 0) return;
    if(idx == digits.size()) {
        res.push_back(s);
        return;
    }
    int d = digits[idx] - '0'; // idx指向的数字转为int
    string d_to_s = digit_map[d]; // 数字对应的字符串
    for(int i = 0; i < d_to_s.size(); i ++) {
        s.push_back(d_to_s[i]);
        backtracking(digits, res, idx + 1, s);
        s.pop_back(); // 回溯
    }
}
vector<string> letterCombinations(string digits) {
    vector<string> res;
    string s;
    backtracking(digits, res, 0, s);
    return res;
}
```