---
comments: true
---

## [110.平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/)

> 每一个节点的左右子树高度差的绝对值不超过1  --> [高度VS深度](https://code-thinking-1253855093.file.myqcloud.com/pics/20210203155515650.png)

```cpp linenums="1"
int getDepth(TreeNode* node) {
    if(!node) return 0;
    return 1 + max(getDepth(node->left), getDepth(node->right));
}
bool isBalanced(TreeNode* root) {
    if(!root) return true;
    int l = getDepth(root->left), r = getDepth(root->right);
    return abs(l - r) <= 1 && isBalanced(root->left) && isBalanced(root->right);
}
```

## [257.二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

需要 **回溯** 即回退进入另一个路径,可作为函数参数进行传递,[参考](https://programmercarl.com/0257.%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E6%89%80%E6%9C%89%E8%B7%AF%E5%BE%84.html#%E6%80%9D%E8%B7%AF:~:text=%E5%A6%82%E4%B8%8A%E4%BB%A3%E7%A0%81%E4%B8%AD%EF%BC%8C-,%E8%B2%8C%E4%BC%BC%E6%B2%A1%E6%9C%89%E7%9C%8B%E5%88%B0%E5%9B%9E%E6%BA%AF%E7%9A%84%E9%80%BB%E8%BE%91,-%EF%BC%8C%E5%85%B6%E5%AE%9E%E4%B8%8D%E7%84%B6%EF%BC%8C%E5%9B%9E%E6%BA%AF)

递归: 
```cpp linenums="1"
void traverse(vector<string>& res, TreeNode* root, string t) {
    if(!root->left && !root->right) {
        res.push_back(t);
        return;
    }
    if(root->left)
        traverse(res, root->left, t + "->" + to_string(root->left->val));
    if(root->right) 
        traverse(res, root->right, t + "->" + to_string(root->right->val));
}
vector<string> binaryTreePaths(TreeNode* root) {
    vector<string> res;
    if(!root) return res;
    traverse(res, root, to_string(root->val));
    return res;
}
```

迭代: 利用栈一个保存遍历的节点一个保存路径对应的值
```cpp linenums="1" hl_lines="6"
vector<string> binaryTreePaths(TreeNode* root) {
    vector<string> res;
    stack<TreeNode*> st;
    stack<string> path;
    if(!root) return res;
    st.push(root), path.push(to_string(root->val));
    while(!st.empty()) {
        auto node = st.top(); st.pop();
        string p = path.top(); path.pop();
        if(!node->left && !node->right) res.push_back(p);
        if(node->left) {
            st.push(node->left);
            path.push(p + "->" + to_string(node->left->val));
        }
        if(node->right) {
            st.push(node->right);
            path.push(p + "->" + to_string(node->right->val));
        }
    }
    return res;
}
```

## [404.左叶子之和](https://leetcode.cn/problems/sum-of-left-leaves/)

> 节点A的左孩子不为空,且左孩子的左右孩子都为空,则A节点的左孩子为左叶子节点

递归: 
```cpp linenums="1"
int sumOfLeftLeaves(TreeNode* root) { 
    if(!root) return 0;
    if(!root->left && !root->right) return 0;
    int l = sumOfLeftLeaves(root->left), r = sumOfLeftLeaves(root->right);
    if(root->left && !root->left->left && !root->left->right) 
        l = root->left->val;
    return l + r;
}
```

迭代:
```cpp linenums="1"
int sumOfLeftLeaves(TreeNode* root) {
    int sum = 0;
    stack<TreeNode*> st;
    if(!root) return 0;
    st.push(root);
    while(!st.empty()) {
        TreeNode* node = st.top(); st.pop();
        if(node->left && !node->left->left && !node->left->right)
            sum += node->left->val;
        if(node->left) st.push(node->left);
        if(node->right) st.push(node->right);
    }
    return sum;
}
```

