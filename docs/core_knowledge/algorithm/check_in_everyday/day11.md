---
comments: true
---

## 概述

- 满二叉树: 只有度为0的结点和度为2的结点，并且度为0的结点在同一层上(e.g.[示例](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806185805576.png))
- 完全二叉树: 除了最底层节点可能没填满外，其余每层节点数都达到最大值
- 二叉搜索树: 左 < 根 < 右, 中序遍历有序(e.g.[示例](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806190304693.png))
- 平衡二叉搜索树(AVL): 空树或左右子树高度差的绝对值不超过1，且左右也是AVL(e.g.[示例](https://code-thinking-1253855093.file.myqcloud.com/pics/20200806190511967.png))

!!! Note

    **C++中`map,set,multimap,multiset`的底层实现都是平衡二叉搜索树**

    而`unordered_map,unordered_set`则底层实现是哈希表

### 存储方式

**顺序 & 链式**,一般都是采用链式

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

## 二叉树的递归/迭代遍历

### [144.前序](https://leetcode.cn/problems/binary-tree-preorder-traversal/)

递归 : 根左右
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

迭代: 按照 ^^根 右 左^^ 的顺序入栈,这样才可以保证出栈先访问左 -->[动图](https://code-thinking.cdn.bcebos.com/gifs/%E4%BA%8C%E5%8F%89%E6%A0%91%E5%89%8D%E5%BA%8F%E9%81%8D%E5%8E%86%EF%BC%88%E8%BF%AD%E4%BB%A3%E6%B3%95%EF%BC%89.gif)
```c++ linenums="1"
vector<int> preorderTraversal(TreeNode* root) {
    stack<TreeNode*> st;
    vector<int> res;
    if (root == nullptr) return res;
    st.push(root);
    while(!st.empty()) {
        TreeNode* node = st.top(); st.pop();
        res.push_back(node->val);
        if (node->right) st.push(node->right);
        if (node->left) st.push(node->left);
    }
    return res;
}
```

### [94.中序](https://leetcode.cn/problems/binary-tree-inorder-traversal/)

递归: 在前序基础上修改对应语句顺序即可(左根右)
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

迭代 : [动图](https://code-thinking.cdn.bcebos.com/gifs/%E4%BA%8C%E5%8F%89%E6%A0%91%E4%B8%AD%E5%BA%8F%E9%81%8D%E5%8E%86%EF%BC%88%E8%BF%AD%E4%BB%A3%E6%B3%95%EF%BC%89.gif)
```c++ linenums="1"
vector<int> inorderTraversal(TreeNode* root) {
    vector<int> res;
    TreeNode* cur = root;
    stack<TreeNode*> st;
    while(cur != nullptr || !cur.empty()) {
        if (cur != nullptr) st.push(cur), cur = cur->left;
        else {
            cur = st.top(); st.pop();
            res.push_back(cur->val);
            cur = cur->right;
        }
    }
    return res;
}
```

### [145.后序](https://leetcode.cn/problems/binary-tree-postorder-traversal/)

递归: 同样修改语句顺序即可(左右根)
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

迭代: 在前序基础上进行修改

- 后序(左右根) --> 逆序(根右左),即 **前序中修改为先左入栈, 最后逆序** 
```c++ linenums="1"
vector<int> postorderTraversal(TreeNode* root) {
    vector<int> res;
    stack<TreeNode*> st;
    if (root == nullptr) return res;
    st.push(root);
    while(!st.empty()) {
        TreeNode* node = st.top(); st.pop();
        res.push_back(node->val);
        if(node->left) st.push(node->left);
        if(node->right) st.push(node->right);
    }
    reverse(res.begin(), res.end());
    return res;
}
```