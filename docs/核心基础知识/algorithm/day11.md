---
comments: true
---

## 概述

- 满二叉树: 只有度为0的结点和度为2的结点，并且度为0的结点在同一层上e.g.[示例](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806185805576.png)
- 完全二叉树: 除了最底层节点可能没填满外，其余每层节点数都达到最大值
- 二叉搜索树: 左 < 根 < 右, 中序遍历有序,e.g.[示例](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806190304693.png)
- 平衡二叉搜索树(AVL): 一棵空树或它的左右子树的高度差的绝对值不超过1，并且左右也都是一棵平衡二叉树e.g.[示例](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806190511967.png)

!!! Note

    **C++中`map,set,multimap,multiset`的底层实现都是平衡二叉搜索树**

    而`unordered_map,unordered_set`则底层实现是哈希表

### 存储方式

可以顺序也可以链式,一般都是采用链式

用数组的话, 若父节点的数组下标是`i`，那么它的左孩子`i * 2 + 1`，右孩子`i * 2 + 2`

### 遍历方式

深度DFS: 先向深走，遇到叶子节点再往回走(e.g.前,中,后序)
广度BFS: 一层一层去遍历(e.g.层序)

### 定义

```c++ linenums="1"
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

## 二叉树的递归遍历

### [144.前序](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

```c++ linenums="1"
void traversal(TreeNode* root, vector<int>& res) {
    if (root == nullptr) return;
    res.push_back(root->val);
    traversal(root->left, res);
    traversal(root->right, res);
}
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    traversal(root, res);
    return res;
}
```

### [94.中序](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

在前序基础上修改对应语句顺序即可
```c++ linenums="1"
void traversal(TreeNode* root, vector<int>& res) {
    if (root == nullptr) return;
    traversal(root->left, res);
    res.push_back(root->val);
    traversal(root->right, res);
}
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> res;
    traversal(root, res);
    return res;
}
```

### [145.后序](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

同样修改语句顺序即可
```c++ linenums="1"
void traversal(TreeNode* root, vector<int>& res) {
    if (root == nullptr) return;
    traversal(root->left, res);
    traversal(root->right, res);
    res.push_back(root->val);
}
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    traversal(root, res);
    return res;
}
```