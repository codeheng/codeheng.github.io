---
comments: true
---

## [530.二叉搜索树的最小绝对差](https://leetcode.cn/problems/minimum-absolute-difference-in-bst/)

转为有序数组,然后求相邻的最小值即可
```cpp linenums="1"
void dfs(TreeNode* root, vector<int>& vec) {
    if(!root) return;
    dfs(root->left, vec);
    vec.push_back(root->val);
    dfs(root->right, vec);
}
int getMinimumDifference(TreeNode* root) {
    vector<int> vec;
    dfs(root, vec);
    if(vec.size() <= 1) return 0;
    int res = INT_MAX;
    for(int i = 1; i < vec.size(); i ++)
        res = min(res, vec[i] - vec[i - 1]);
    return res;
}
```

## [501.二叉搜索树中的众数](https://leetcode.cn/problems/find-mode-in-binary-search-tree/)

```cpp linenums="1"
void help(TreeNode* root, int& val, int& cnt, int& max_cnt, vector<int>& res) {
    if(!root) return;
    help(root->left, val, cnt, max_cnt, res);
    cnt = root->val == val ? cnt + 1 : 1;
    if(cnt == max_cnt) res.push_back(root->val);
    else if (cnt > max_cnt) {
        max_cnt = cnt;
        res.clear(); // 清空res,之前的都失效了
        res.push_back(root->val);
    }
    val = root->val;
    help(root->right, val, cnt, max_cnt, res);
}
vector<int> findMode(TreeNode* root) {
    int curr_val = 0, curr_cnt = 0, max_cnt = 0;
    vector<int> res;
    help(root, curr_val, curr_cnt, max_cnt, res);
    return res;
}
```

## [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

```cpp linenums="1"
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(root == q || root == p || !root) return root;
    auto left = lowestCommonAncestor(root->left, p, q);
    auto right = lowestCommonAncestor(root->right ,p , q);
    if(left && right) return root;
    else if(!left && right) return right;
    else if(left && !right) return left; 
    else return nullptr;
}
```