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

递归: 不采用累加,而是目标值每次减去节点上的值,看最后为0么  --> [回溯精简前参考](https://programmercarl.com/0112.%E8%B7%AF%E5%BE%84%E6%80%BB%E5%92%8C.html#%E6%80%9D%E8%B7%AF:~:text=9%0A10%0A11-,%E6%95%B4%E4%BD%93%E4%BB%A3%E7%A0%81%E5%A6%82%E4%B8%8B%EF%BC%9A,-class%20Solution%20%7B)
```cpp linenums="1"
bool hasPathSum(TreeNode* root, int targetSum) {
    if(!root) return false;
    if(root->val == targetSum && !root->left && !root->right) return true;
    return hasPathSum(root->left, targetSum - root->val)
        || hasPathSum(root->right, targetSum - root->val);
}
```

迭代: 利用栈模拟,需要存该节点的指针和从头到该节点的路径和 --> 利用`pair`
```cpp linenums="1"
bool hasPathSum(TreeNode* root, int targetSum) {
    stack<pair<TreeNode*, int>> st;
    if(!root) return false; 
    st.push({root, root->val});
    while(!st.empty()) {
        auto node = st.top(); st.pop();
        if(!node.first->left && !node.first->right 
            && targetSum == node.second)  return true;
        if(node.first->left) 
            st.push({node.first->left, node.second + node.first->left->val});
        if(node.first->right)
            st.push({node.first->right, node.second + node.first->right->val});
    }
    return false;
}
```

## [113.路径总和II](https://leetcode.cn/problems/path-sum-ii/)

> 找所有满足路径和的路径

```cpp linenums="1" hl_lines="6 7"
void dfs(vector<vector<int>>& res, vector<int> &path, 
            TreeNode* root, int sum) {
    if(!root) return;
    path.push_back(root->val);
    if(!root->left && !root->right && sum == root->val) res.push_back(path);
    dfs(res, path, root->left, sum - root->val);
    dfs(res, path, root->right, sum - root->val);
    path.pop_back();
}
vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
    vector<vector<int>> res;
    vector<int> path;
    dfs(res, path, root, targetSum);
    return res;
}
```

## [105.从前中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

先从前序第一个节点开始,即头节点,之后在中序中进行切分,进行递归
```cpp linenums="1"
TreeNode* build(vector<int>& pre, int preBegin, int preEnd,
                vector<int>& in, int inBegin, int inEnd) {
    if(preBegin > preEnd || inBegin > inEnd) return nullptr;
    auto node = new TreeNode(pre[preBegin]);
    int inRootIdx = inBegin;
    while(node->val != in[inRootIdx]) inRootIdx ++;
    node->left = build(pre, preBegin + 1, preBegin + inRootIdx - inBegin,
                        in, inBegin, inRootIdx - 1);
    node->right = build(pre, preBegin + inRootIdx - inBegin + 1, preEnd,
                        in, inRootIdx + 1, inEnd);
    return node;
}
TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    return build(preorder, 0, preorder.size() - 1, 
                    inorder, 0, inorder.size() - 1);
}
```

## [106.从中后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)

同理,先从后序最后一个节点开始,即头节点,之后在中序中递归切分
```cpp linenums="1"
TreeNode* build(vector<int>& in, int inBegin, int inEnd,
                vector<int>& post, int postBegin, int postEnd) {
    if(inBegin > inEnd || postBegin > postEnd) return nullptr;
    auto node = new TreeNode(post[postEnd]);
    int inRootIdx = inBegin;
    while(node->val != in[inRootIdx]) inRootIdx ++;
    node->left = build(in, inBegin, inRootIdx - 1, 
                       post, postBegin, postBegin + inRootIdx - inBegin - 1);
    node->right = build(in, inRootIdx + 1, inEnd, 
                        post, postBegin + inRootIdx - inBegin, postEnd - 1);
    return node;
}
TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
    return build(inorder, 0, inorder.size() - 1, 
                    postorder, 0, postorder.size() - 1);
}
```