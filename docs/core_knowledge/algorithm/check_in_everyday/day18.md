---
comments: true
---

## [235.二叉搜索树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-search-tree/)

利用二叉搜索树中序是有序的,故公共祖先一定在两者中间

递归: 
```cpp linenums="1"
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if(root->val > p->val && root->val > q->val)
        return lowestCommonAncestor(root->left, p, q);
    else if(root->val < p->val && root->val < q->val)
        return lowestCommonAncestor(root->right, p, q);
    return root;
}
```

迭代: 
```cpp linenums="1"
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    while(root) {
        if(root->val > p->val && root->val > q->val) root = root->left;
        else if(root->val < p->val && root->val < q->val) root = root->right;
        else return root;
    }
    return nullptr;
}
```

## [701.二叉搜索树中的插入操作](https://leetcode.cn/problems/insert-into-a-binary-search-tree/)

```cpp linenums="1"
TreeNode* insertIntoBST(TreeNode* root, int val) {
    if(!root) {
        auto node = new TreeNode(val);
        return node;
    }
    if(root->val > val) root->left = insertIntoBST(root->left, val);
    else if(root->val < val) root->right = insertIntoBST(root->right, val);
    return root;      
}
```

## [450.删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)

先找,若没找到直接返回, 若找到了,几种情况

1. 左右都空,直接删除
2. 左空右不空，删除后,右孩子补位, 返回右孩子
3. 左不空右空, 删除后,左孩子补位, 返回左孩子
4. 左右都不空, 将删除左子树的头节点放到删除节点的右子树最左边的左孩子上, 返回删除节点右孩子 --> [动图](https://code-thinking.cdn.bcebos.com/gifs/450.%E5%88%A0%E9%99%A4%E4%BA%8C%E5%8F%89%E6%90%9C%E7%B4%A2%E6%A0%91%E4%B8%AD%E7%9A%84%E8%8A%82%E7%82%B9.gif)

```cpp linenums="1"
TreeNode* deleteNode(TreeNode* root, int key) {
    if(!root) return root; // 没找到
    if(root->val == key) {
        // 左右都空
        if(!root->left && !root->right) {
            delete root;
            return nullptr;
        } 
        // 左空右不空 
        else if(!root->left && root->right) {
            auto tmp = root->right;
            delete root;
            return tmp;
        }
        // 左不空右空
        else if(root->left && !root->right) {
            auto tmp = root->left;
            delete root;
            return tmp;
        }
        // 左右都不空
        else {
            auto cur = root->right; // 找右子树最左边的节点
            while(cur->left) cur = cur->left;
            cur->left = root->left; //删除节点的左放到右的最左边
            auto tmp = root; // 暂存root, 删除
            root = root->right;
            delete tmp;
            return root;
        }
    }
    if(root->val > key) root->left = deleteNode(root->left, key);
    if(root->val < key) root->right = deleteNode(root->right, key);
    return root;
}
```