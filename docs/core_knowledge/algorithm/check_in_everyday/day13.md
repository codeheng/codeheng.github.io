---
comments: true
---

## [104.二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)

递归 : 
```cpp linenums="1"
int maxDepth(TreeNode* root) {
    if (!root) return 0;
    int maxL = maxDepth(root->left), maxR = maxDepth(root->right);
    return max(maxL, maxR) + 1;
}
```

层序遍历进行迭代 : 
```cpp linenums="1" hl_lines="8"
int maxDepth(TreeNode* root) {
    int depth = 0;
    queue<TreeNode*> q;
    if(!root) return 0;
    q.push(root);
    while(!q.empty()) {
        int size = q.size();
        depth ++;
        while( size -- ) {
            TreeNode* node = q.front(); q.pop();
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
        }
    }
    return depth; 
}
```

## [559.n叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/)

```cpp linenums="1"
int maxDepth(Node* root) {
    if(!root) return 0;
    int depth = 0;
    for(auto i : root->children) depth = max(depth, maxDepth(i));
    return 1 + depth;
}
```

## [111.二叉树的最小深度](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)

> 最小深度是从根节点到最近 **叶子节点** 的最短路径上的节点数量

递归: 
```cpp linenums="1" hl_lines="4 5"
int getDepth(TreeNode* node) {
    if(!node) return 0;
    int l = getDepth(node->left), r = getDepth(node->right); // 左 & 右
    if(!node->left && node->right) return 1 + r;
    if(node->left && !node->right) return 1 + l;
    return 1 + min(l, r);
}
int minDepth(TreeNode* root) {
    return getDepth(root);
}
```

迭代: 在上题基础上修改: 当左右孩子都空，即最低点的一层, 进行return
```cpp linenums="1" hl_lines="13"
int minDepth(TreeNode* root) {
    int depth = 0;
    queue<TreeNode*> q;
    if(!root) return 0;
    q.push(root);
    while(!q.empty()) {
        int size = q.size();
        depth ++;
        while(size --) {
            TreeNode * node = q.front(); q.pop();
            if(node->left) q.push(node->left);
            if(node->right) q.push(node->right);
            if(!node->left && !node->right) return depth;
        }
    }
    return depth;
}
```

## [222.完全二叉树的节点个数](https://leetcode.cn/problems/count-complete-tree-nodes/)

直接左右递归计算左右子树的长度相加即可, 或者利用层序遍历,每次`pop`统计数量
```cpp linenums="1"
int countNodes(TreeNode* root) {
    if(!root) return 0;
    return 1 + countNodes(root->left) + countNodes(root->right);
}
```

!!! Note
    在完全二叉树中，如果递归向左遍历的深度 = 递归向右遍历的深度，则是满二叉树