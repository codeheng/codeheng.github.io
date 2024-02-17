---
comments: true
---

## [654.最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/)

```cpp linenums="1"
TreeNode* build(vector<int>& nums, int l, int r) {
    if(l > r) return nullptr;
    int max_idx = l;
    for (int i = l; i <= r;i ++) 
        if(nums[i] > nums[max_idx]) max_idx = i;
    auto root = new TreeNode(nums[max_idx]);
    root->left = build(nums, l, max_idx - 1);
    root->right = build(nums, max_idx + 1, r);
    return root;
}
TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
    return build(nums, 0, nums.size() - 1);
}
```

## [617.合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)

```cpp linenums="1"
TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
    if(!root1) return root2;
    if(!root2) return root1;
    TreeNode* root = new TreeNode(0); // 定义新节点
    root->val = root1->val + root2->val;
    root->left = mergeTrees(root1->left, root2->left);
    root->right = mergeTrees(root1->right, root2->right);
    return root;
}
```

## [700.二叉搜索树中的搜索](https://leetcode.cn/problems/search-in-a-binary-search-tree/)

> 二叉搜索树: 若左不空,则左都小于根,若右不空,则右都大于根

递归: 
```cpp linenums="1"
TreeNode* searchBST(TreeNode* root, int val) {
    if(!root || root->val == val) return root;
    if(root->val > val) return searchBST(root->left, val);
    if(root->val < val) return searchBST(root->right, val);
    return nullptr;
}
```

迭代:
```cpp linenums="1"
TreeNode* searchBST(TreeNode* root, int val) {
    while(root) {
        if(root->val > val) root = root->left;
        else if(root->val < val) root = root->right;
        else return root;
    }
    return nullptr;
}
```

## [98.验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

看中序遍历是否有序即可
```cpp linenums="1"
void dfs(TreeNode* root, vector<int>& vec) {
    if(!root) return;
    dfs(root->left, vec);
    vec.push_back(root->val); // 转为数组
    dfs(root->right, vec);
}
bool isValidBST(TreeNode* root) {
    vector<int> vec;
    dfs(root, vec); // 中序转为数组,看有序否
    for(int i = 1; i < vec.size(); i ++)
        if(vec[i] <= vec[i - 1]) return false;
    return true;
}
```