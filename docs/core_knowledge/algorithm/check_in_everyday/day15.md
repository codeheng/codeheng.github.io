---
comments: true
---

## [513.找树左下角的值](https://leetcode.cn/problems/find-bottom-left-tree-value/)

层序遍历记录最后一行的第一个节点值即可(迭代), 也可以用for循环记录第一个 --> [参考](https://programmercarl.com/0513.%E6%89%BE%E6%A0%91%E5%B7%A6%E4%B8%8B%E8%A7%92%E7%9A%84%E5%80%BC.html#%E6%80%9D%E8%B7%AF:~:text=%23-,%E8%BF%AD%E4%BB%A3%E6%B3%95,-%E6%9C%AC%E9%A2%98%E4%BD%BF%E7%94%A8%E5%B1%82)
```cpp linenums="1" hl_lines="7 8"
int findBottomLeftValue(TreeNode* root) {
    queue<TreeNode*> q;
    if(!root) return 0;
    q.push(root);
    while(!q.empty()) {
        root = q.front(); q.pop();
        if(root->right) q.push(root->right); // 顺序不能变,若改变就找最右下角
        if(root->left) q.push(root->left);
    }
    return root->val;
}
```

递归:
```cpp linenums="1"
void traverse(TreeNode* root, int& res, int& depth, int h) {
    if(!root) return;
    if(h > depth) {
        depth = h;
        res = root->val;
    }
    traverse(root->left, res, depth, h + 1);
    traverse(root->right, res, depth, h + 1);
}
int findBottomLeftValue(TreeNode* root) {
    int res = 0, depth = -1;
    traverse(root, res, depth, 0);
    return res;
}
```

## [112.路径总和](https://leetcode.cn/problems/path-sum/)

```cpp linenums="1"
bool hasPathSum(TreeNode* root, int targetSum) {
    if(!root) return false;
    if(root->val == targetSum && !root->left && !root->right) return true;
    return hasPathSum(root->left, targetSum - root->val)
        || hasPathSum(root->right, targetSum - root->val);
}
```

## [113.路径总和II](https://leetcode.cn/problems/path-sum-ii/)

## [105.从前中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

## [106.从中后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)