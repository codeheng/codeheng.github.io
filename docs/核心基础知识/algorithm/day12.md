---
comments: true
---

## [102.层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

需要借助队列 --> [动图参考](https://code-thinking.cdn.bcebos.com/gifs/102%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86.gif)
```cpp linenums="1"
vector<vector<int>> levelOrder(TreeNode* root) {
    queue<TreeNode*> q;
    vector<vector<int>> res;
    if (root != nullptr) q.push(root);
    while(!q.empty()) {
        int size = q.size();
        vector<int> vec;
        while(size --) {
            TreeNode* node = q.front(); q.pop();
            vec.push_back(node->val);
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
        res.push_back(vec);
    }
    return res;
}
```


## [226.翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)

每个节点左右孩子进行交换即可
```cpp linenums="1"
TreeNode* invertTree(TreeNode* root) {
    if (root == nullptr) return root;
    swap(root->left, root->right);
    invertTree(root->left); invertTree(root->right);
    return root;
}
```

也可以利用层序
```cpp linenums="1"
TreeNode* invertTree(TreeNode* root) {
    queue<TreeNode*> q;
    if(root != nullptr) q.push(root);
    while(!q.empty()) {
        int size = q.size();
        while(size --) {
            TreeNode* node = q.front(); q.pop();
            swap(node->left, node->right);
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
    }
    return root;
}
```


## [101.对称二叉树](https://leetcode.cn/problems/symmetric-tree/)

要比较内侧和外侧节点是否分别对应相等 --> [如图](https://code-thinking-1253855093.file.myqcloud.com/pics/20210203144624414.png)

- 即左节点的左孩子与右节点的右孩子(外侧) & 左节点的右孩子与右节点的做孩子(内侧)

```cpp linenums="1"
bool compare(TreeNode* left, TreeNode* right) {
    if (!left || !right) return left == right;
    return (left->val == right->val) 
            && compare(left->left, right->right) 
            && compare(left->right, right->left);
}
bool isSymmetric(TreeNode* root) {
    if (!root) return true;
    return compare(root->left, root->right);
}
```