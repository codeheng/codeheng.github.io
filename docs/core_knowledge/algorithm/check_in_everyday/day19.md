---
comments: true
---


## [669.修剪二叉搜索树](https://leetcode.cn/problems/trim-a-binary-search-tree/)

递归: (关于多余节点如何移除 --> [参考](https://code-thinking-1253855093.file.myqcloud.com/pics/20210204155327203-20230310120126738.png))
```cpp linenums="1"
TreeNode* trimBST(TreeNode* root, int low, int high) {
    if(!root) return nullptr;
    if(root->val < low) return trimBST(root->right, low, high);
    if(root->val > high) return trimBST(root->left, low, high);
    root->left = trimBST(root->left, low, high);
    root->right = trimBST(root->right, low, high);
    return root;
}
```

## [108.将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)

> 找分割点,因为有序即是中间的, 然后递归左右区间即可

```cpp linenums="1"
TreeNode* build(vector<int>& nums, int l, int r) {
    if(l > r) return nullptr;
    int mid = l + r >> 1;
    auto node = new TreeNode(nums[mid]);
    node->left = build(nums, l, mid - 1);
    node->right = build(nums, mid + 1, r);
    return node; 
}
TreeNode* sortedArrayToBST(vector<int>& nums) {
    return build(nums, 0, nums.size() - 1);
}
```


## [538.将二叉搜索树转为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)

>累加的顺序是右中左，只需要反中序遍历这个二叉树，然后顺序累加即可

```cpp linenums="1"
void addValue(TreeNode* root, int& sum) {
    if(!root) return;
    addValue(root->right, sum);
    sum += root->val; // 累加获得新的值
    root->val = sum; // 再赋值给root
    addValue(root->left, sum);
}
TreeNode* convertBST(TreeNode* root) {
    int sum = 0;
    addValue(root, sum);
    return root;
}
```